Environment: macOS 14.6 (Sonoma, arm64), Python 3.13.2, passlib 1.7.4, bcrypt 4.3.0, repo at commit `2f4e82f` (my fork, matches upstream `main` at time of testing). Set up with `python3 -m venv .venv` + `pip install -e ".[dev]"` — didn't need Docker/Postgres/Redis running for this, since `core.security` and `core.config` don't touch the DB at import time.

Steps:

```
$ .venv/bin/python -c "
from core.security import verify_password
result = verify_password('password', 'not_a_valid_bcrypt_hash')
print('result:', result)
"
```

Actual: raises instead of returning, exactly as the issue says:

```
Traceback (most recent call last):
  File "<string>", line 3, in <module>
    result = verify_password('password', 'not_a_valid_bcrypt_hash')
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 2343, in verify
    record = self._get_or_identify_record(hash, scheme, category)
  ...
passlib.exc.UnknownHashError: hash could not be identified
```

Expected (per the issue): `verify_password` should fail closed and return `False`, not raise.

Control run, to isolate that this is specifically about the hash *format* and not just "wrong password" in general — a real bcrypt hash with the wrong password still returns `False` cleanly:

```
$ .venv/bin/python -c "
from core.security import verify_password, hash_password
real_hash = hash_password('correct_password')
print(verify_password('wrong_password', real_hash))
"
False
```

Also ran the existing suite for confirmation: `pytest tests/unit/test_security.py -v -m unit` → 24 passed, 1 xfailed. The one xfail is `test_verify_with_wrong_hash_format`, marked `strict=True` with the reason `"issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False"` — same failure, same wording as what I hit directly above.

One thing worth flagging so it doesn't get mistaken for part of the bug: passlib prints a harmless `"(trapped) error reading bcrypt version"` warning on this bcrypt version (a known passlib/bcrypt metadata-reading quirk). It's unrelated — the control run above still returns `False` correctly despite it.
