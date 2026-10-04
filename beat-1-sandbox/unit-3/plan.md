# Plan: issue #72 — verify_password raises UnknownHashError on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
Builds on my repro comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5861275050

**Prior work:** PR #78 is open on this issue, taking the same catch-and-return-False approach. Per the Path Review house rules, I'm building my own diagnosis and plan from my own reproduction rather than reading or copying that PR's diff; I'll compare against it before opening mine, and I'd rather its author lands first than duplicate effort if that happens.

## Diagnosis

`verify_password` in `core/security.py` calls `pwd_context.verify(plain_password, hashed_password)` with no exception handling. Passlib raises instead of returning `False` in two distinct cases I've reproduced myself:

1. **Unrecognizable format** (already posted in my Unit 2 repro comment, quoted here):
   > `verify_password('password', 'not_a_valid_bcrypt_hash')` → `passlib.exc.UnknownHashError: hash could not be identified`

   Also true for an empty string and a whitespace-only string — same exception.

2. **Correct prefix, corrupted internals** (new — tested while writing this plan, not covered by my Unit 2 report). Actual command and traceback:
   ```
   $ .venv/bin/python -c "
   from core.security import verify_password
   result = verify_password('password', '\$2b\$12\$shorttruncatedhash')
   print('result:', result)
   "
   Traceback (most recent call last):
     File "<string>", line 3, in <module>
       result = verify_password('password', '$2b$12$shorttruncatedhash')
     File ".../core/security.py", line 37, in verify_password
       return bool(pwd_context.verify(plain_password, hashed_password))
     File ".../passlib/context.py", line 2347, in verify
       return record.verify(secret, hash, **kwds)
     File ".../passlib/utils/handlers.py", line 788, in verify
       self = cls.from_string(hash, **context)
     ...
     File ".../passlib/utils/handlers.py", line 1466, in _norm_salt
       raise ValueError(msg)
   ValueError: salt too small (bcrypt requires exactly 22 chars)
   ```
   This carries a valid-looking `$2b$` bcrypt prefix, so passlib gets past identification and into the bcrypt handler's own salt parsing before it fails — a different code path than case 1, raising a plain `ValueError` instead. (I also tried an out-of-range cost factor, `$2b$99$` + 53 chars, which raises a second `ValueError`: `"bcrypt: rounds (99) is too large, cannot be more than 31"` — same category of failure, different message, confirming this isn't a one-off string fluke.)

Both are "malformed stored hash" per the issue's wording, and neither is caught today, so both crash the caller instead of returning `False`.

Control, from my Unit 2 report: a real bcrypt hash with the *wrong* password still returns `False` cleanly — this bug is specifically about unrecognized/corrupted hash *format*, not password mismatches in general, and that path isn't touched by this fix.

**One clause covers both cases.** `UnknownHashError` and `PasswordSizeError` (an oversized-password case I hadn't considered until checking this) are both subclasses of `ValueError` in this version of passlib — verified directly, not assumed:
```
$ .venv/bin/python -c "from passlib.exc import UnknownHashError, PasswordSizeError; print(issubclass(UnknownHashError, ValueError), issubclass(PasswordSizeError, ValueError))"
True True
```
So a single `except ValueError` catches everything case 1 and case 2 raise, with no need to import or name `UnknownHashError` specifically.

**Real production impact, not just a unit test.** I grepped the codebase for other callers of `verify_password`: there is exactly one, `api/routes/auth.py`'s `login` route (`if not user or not verify_password(form_data.password, user.hashed_password):`). I first assumed this call had no exception handling around it at all; checking the full function showed that's wrong — `login` has an outer `except HTTPException: raise` / `except Exception as exc: log.error(...)` that catches everything else and returns a generic `500`. I verified the actual behavior directly (called `login` with a mock DB returning a user whose stored hash is `"not_a_valid_bcrypt_hash"`, before and after the fix):

```
# before (on main, verify_password uncaught):
login_error  error='hash could not be identified'
HTTPException: 500 - Login failed

# after (this branch):
login_failed_invalid_credentials  email=test@example.com
HTTPException: 401 - Invalid email or password
```

So today a malformed stored hash doesn't crash the server unhandled — it's masked by the route's catch-all into a vague `500 "Login failed"`, hiding the real cause and giving the client the wrong status entirely, instead of the specific `401` the route already has a branch for. This fix routes it to that existing branch instead of letting it fall through to the catch-all. The caller doesn't need any change itself.

## Scope

**In scope:** catching `ValueError` inside `verify_password` and returning `False`. Removing the `@pytest.mark.xfail` marker on `test_verify_with_wrong_hash_format` (it should pass once the fix lands) and adding one new regression test for the corrupted-internals shape, which the existing xfail test doesn't cover.

**Not in scope, with reasons:**
- `hash_password`, `create_access_token`, `decode_access_token` — not implicated by any evidence I've gathered; the control run above shows the only other password-verification path (wrong password against a real hash) already works correctly.
- The bcrypt scheme configuration itself (`CryptContext(schemes=["bcrypt"])`) — untouched; this is a call-site exception-handling fix, not a hashing-config change.
- `api/routes/auth.py`'s `login` route itself — no change needed there; it already has the `401` branch ready, and its outer catch-all doesn't need touching either (see the production-impact note above, verified by actually calling the route, not by assuming its behavior).

## Files and approach

- `core/security.py`: wrap `return bool(pwd_context.verify(plain_password, hashed_password))` in a `try`/`except ValueError` block, returning `False`. One clause, no new import needed — see the verified subclass relationship above.
- `tests/unit/test_security.py`:
  1. Remove the `@pytest.mark.xfail(...)` decorator from `test_verify_with_wrong_hash_format`.
  2. Add a new test (e.g. `test_verify_with_malformed_bcrypt_internals`) asserting `verify_password('password', '$2b$12$shorttruncatedhash')` returns `False` instead of raising — covering the `ValueError` path case 2 above.

## Test plan

Re-run my Unit 2 repro steps against the real fix, before and after:

- **Before** (already posted in Unit 2): `verify_password('password', 'not_a_valid_bcrypt_hash')` raises `passlib.exc.UnknownHashError`.
- **After (expected):** same call returns `False`.
- Same before/after shape for the new `ValueError` case found above (`'$2b$12$shorttruncatedhash'`).
- `pytest tests/unit/test_security.py -v -m unit`: before = 24 passed, 1 xfailed; after = 26 passed, 0 xfailed (one more than originally estimated, since removing the `xfail` marker plus adding the new regression test are both now real tests, not 25 — counted wrong in the original plan).
- Re-confirm the control case (real hash, wrong password → `False`) still passes unchanged, to confirm the fix doesn't touch legitimate wrong-password detection.
- Exercise the `login` route directly (mock DB returning a user with a malformed stored hash, no real Postgres needed) before and after: before, the route's catch-all masks it into a generic `500 "Login failed"`; after, the route's existing branch returns a clean `401 Invalid email or password`.

## Risks and unknowns

- Catching `ValueError` is broad by design (see above — it's the common base of both named passlib exceptions, verified), but passlib's bcrypt backend could in principle raise some other, unrelated `ValueError` I haven't hit in testing. If so this fix would also turn that into a `False` (invalid credentials) rather than a surfaced error. I don't have evidence this happens, so I'm not narrowing the catch preemptively, but I'll say so in the PR description as a named trade-off rather than silently widen scope.
- All of the above was tested on Python 3.13.2 / passlib 1.7.4 / bcrypt 4.3.0 (same environment as my Unit 2 repro) — haven't verified on other dependency-pinned environments.

## Deviations

The build matched the plan's scope exactly: one `try`/`except ValueError` wrapped around the single call in `verify_password` (`core/security.py`), the `xfail` marker removed, and one new regression test added (`tests/unit/test_security.py`). Nothing outside those two files changed.

Two things in the plan's wording turned out to be wrong once I actually checked them, both caught before the fix shipped (and after the plan comment was already posted, so I edited that comment too, not just this file):

1. **The `login` route claim.** I originally wrote that `login` "has no exception handling around the call at all." Checking the full function showed that's wrong — it has an outer `except HTTPException: raise` / `except Exception as exc: ...` that catches everything else and returns a generic `500`. I verified the real before/after by calling `login` directly with a mock DB returning a user whose stored hash is malformed: before the fix, the catch-all masks it into `500 "Login failed"`; after, it correctly hits the route's own `401 "Invalid email or password"` branch. The production-impact conclusion (wrong status, real bug) was right; the mechanism I described was not. Fixed in this file and in the posted comment.
2. **Test count.** I estimated "25 passed, 0 xfailed" after the fix. The real count is 26 — I forgot that the new regression test adds to the total on top of the `xfail` marker's removal, not instead of it. Arithmetic slip, not a scope or evidence problem.

Everything else in the test plan ran exactly as written: both malformed-hash shapes return `False` instead of raising, the control case (real hash, wrong password) is unchanged, and the full `test_security.py` suite passes (26/26, 0 xfailed, up from 24 passed/1 xfailed before).
