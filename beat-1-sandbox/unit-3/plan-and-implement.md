# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

rafiatasafi

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5975775490

(edited after posting to correct an inaccurate claim — see Deviations in plan.md)

Plan for #72, built from my reproduction above (https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5861275050). verify_password doesn't catch anything around pwd_context.verify(...), and there are two distinct ways a malformed hash crashes it — a totally unrecognized format raises UnknownHashError (shown in the repro above), and a hash with a valid-looking $2b$ prefix but corrupted internals (bad salt length, out-of-range cost) raises a plain ValueError instead, which I found and reproduced while writing this plan.

Turns out one clause covers both: UnknownHashError (and PasswordSizeError, for oversized passwords) are both ValueError subclasses in this passlib version — verified with issubclass(), not assumed. So the fix is except ValueError, no new import needed. I also grepped for other callers of verify_password: there's exactly one, the login route. I verified its actual behavior directly (mock DB, malformed stored hash): today it's caught by the route's outer catch-all and masked into a generic 500 "Login failed", hiding the real cause — not a crash, but still the wrong status instead of the 401 the route already has a branch for. So this isn't just a unit-test gap. That route doesn't need any changes itself; it already has the right branch waiting for a False.

I've seen PR #78, which takes the same approach — building my own from my own repro per the house rules, and I'll compare before opening mine. Test plan: my Unit 2 repro steps re-run against the real fix (both malformed-hash shapes), the full test_security.py suite before/after, and the login route exercised directly with a malformed hash to confirm the masked 500 becomes a clean 401.

---

## Your branch

**Branch**

fix/72-verify-password-fail-closed

**Evidence**

Case 1 — unrecognized hash format:

```
$ .venv/bin/python -c "
from core.security import verify_password
result = verify_password('password', 'not_a_valid_bcrypt_hash')
print('result:', result)
"
```

Before (posted in Unit 2):
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

After (this branch):
```
result: False
```

Case 2 — valid bcrypt prefix, corrupted internals:

```
$ .venv/bin/python -c "
from core.security import verify_password
result = verify_password('password', '\$2b\$12\$shorttruncatedhash')
print('result:', result)
"
```

Before:
```
Traceback (most recent call last):
  File "<string>", line 3, in <module>
    result = verify_password('password', '$2b$12$shorttruncatedhash')
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  ...
  File ".../passlib/utils/handlers.py", line 1466, in _norm_salt
    raise ValueError(msg)
ValueError: salt too small (bcrypt requires exactly 22 chars)
```

After:
```
result: False
```

`login` route, exercised directly (mock DB returning a user with a malformed stored hash — no real Postgres needed):

Before (on `main`):
```
login_error  error='hash could not be identified'
HTTPException: 500 - Login failed
```

After (this branch):
```
login_failed_invalid_credentials  email=test@example.com
HTTPException: 401 - Invalid email or password
```

Full test suite, `pytest tests/unit/test_security.py -v -m unit`:

Before: `24 passed, 1 xfailed`
After: `26 passed, 0 xfailed` (one more than the original plan's estimate of 25 — the new regression test adds to the total on top of the `xfail` marker's removal, not instead of it)

Control (unchanged both before and after, confirming the fix doesn't touch legitimate wrong-password detection):
```
$ .venv/bin/python -c "
from core.security import verify_password, hash_password
real_hash = hash_password('correct_password')
print(verify_password('wrong_password', real_hash))
"
False
```

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 3` (pkg-01–03): `agreement: 3/3 scored items`
2. `--only pkg-04,pkg-06,pkg-10,pkg-12,pkg-15,pkg-17,pkg-18,pkg-19,pkg-20` (targeting the `scope-creep`, `unbuildable`, and `thread-convention` categories — the new checks not yet exercised): `agreement: 9/9 scored items`
3. `--limit 20`, full unsaved run: `agreement: 18/20 scored items (bar: 18/20: PASS)` — disagreed on pkg-09 (failed `engages-thread-direction`) and pkg-14 (failed `diagnosis-matches-evidence`)
4. `--only pkg-09,pkg-14` (re-run with no rubric changes, to check whether the disagreement was a rubric gap or model-judgment variance): `agreement: 2/2 scored items` — both flipped to agree with no files touched
5. Final official run, `--save-run eval-run.txt`: `agreement: 19/20 scored items (bar: 18/20: PASS)` — pkg-14 agreed again; pkg-09 disagreed again, same check (`engages-thread-direction`)

**Package analysis**

pkg-09: my rubric's final verdict is `reject`; the gold label is `accept`; they disagree.

Gold's own note calls this one "arguable on the deferral, ready as scoped" — it's one of the
intentionally-arguable packages, not a loud-signal miss. The thread has one maintainer
(tmccombs, COLLABORATOR) who lists three fix options for a globset/Windows path-separator bug
without endorsing one over the others ("(1) match with the glob directly... (2) normalize the
path... (3) a third variant a contributor has since implemented"). The candidate plan picks
option 2, explains why (simpler, matches an existing PR, defers option 1 as a bigger rework),
but never mentions option 3 at all. My rubric's `engages-thread-direction` check has no
`unclear` outcome by design — it must grade pass or fail — so whether silence on one of three
unranked options counts as "ignoring a direction" depends on a judgment call the grading model
didn't make consistently: re-run in isolation (`--only pkg-09,pkg-14`) it passed, citing the
plan's explicit engagement with options 1 and 2; in both full runs it failed, apparently
weighing the silence on option 3 as the deciding fact. Same rubric, same procedure, two
different grades — this is the check's own designed-in binary meeting a package where the
"maintainer direction" isn't actually singular, and the gold label's own hedge ("arguable")
confirms that's a fair description of the package, not just a model quirk.

**Check rationale**

The current pass condition for `engages-thread-direction` reads:

> "Pass if no maintainer direction exists in the thread, or if one does and the plan either
> follows it or explicitly diverges from it with a stated reason; fail if a maintainer
> direction exists and the plan silently ignores or contradicts it without acknowledgment;
> unclear is not available for this check — the thread either has a maintainer direction or
> it does not, and that alone decides which pass condition applies"

It reads this way because the category it targets (`thread-convention`, the 2-package category
the assignment flags as the one no rubric can buy back on volume) needs a check that actually
fires on silence, not one that lets "the package didn't give me enough to tell" default to a
pass. I wrote out the `unclear` carve-out explicitly, rather than leaving it implicit, after
deciding that a maintainer direction either is or isn't in the thread — there's no genuine
absence-of-evidence case here the way there is for, say, a missing test plan. The `pass if no
maintainer direction exists` clause specifically was added so a quiet thread (no maintainer
comments at all) can't accidentally fail a plan for having nothing to engage with — confirmed
directly on pkg-14, where all four thread commenters are NONE association and the check passed
vacuously.

**Trade-offs**

Writing `engages-thread-direction` with no `unclear` outcome trades away a middle ground for
packages like pkg-09, where the thread gives a menu of options rather than a single clear
direction. A check that allowed `unclear` here could have scored pkg-09 as `unclear` (counted
as fail under my verdict rule, so no change in outcome) with an honest "ambiguous" label instead
of a confident but inconsistent pass/fail call. I accepted this trade-off because the thing the
check exists to catch — pkg-04's plan, which proposes a docs-only workaround while the thread
already named the actual culprit file and never engages it at all — is unambiguous, and I
didn't want a soft `unclear` path available for that case to slip through on a technicality. The
cost shows up concretely on pkg-09 (run-to-run disagreement, documented above) rather than on
the case the check was built for.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
