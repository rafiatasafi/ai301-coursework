# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

rafiatasafi

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5861175111

Hey! I'd like to take this one on — verify_password in core/security.py lets passlib's UnknownHashError bubble up when it hits a hash it can't recognize, instead of failing closed and returning False.

I will reproduce it locally first: throw a malformed/unrecognized hash at verify_password and confirm the exception surfaces instead of returning a clean False. There's an @pytest.mark.xfail test in tests/unit/test_security.py (referencing manifest H-05) that looks like the right anchor point; I'll start there and report back with what I find.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5861275050

Environment: macOS 14.6 (Sonoma, arm64), Python 3.13.2, passlib 1.7.4, bcrypt 4.3.0, repo at commit 2f4e82f (my fork, matches upstream main at time of testing). Set up with python3 -m venv .venv + pip install -e ".[dev]" — didn't need Docker/Postgres/Redis running for this, since core.security and core.config don't touch the DB at import time.

Steps:

$ .venv/bin/python -c "
from core.security import verify_password
result = verify_password('password', 'not_a_valid_bcrypt_hash')
print('result:', result)
"

Actual: raises instead of returning, exactly as the issue says:

Traceback (most recent call last):
  File "<string>", line 3, in <module>
    result = verify_password('password', 'not_a_valid_bcrypt_hash')
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 2343, in verify
    record = self._get_or_identify_record(hash, scheme, category)
  ...
passlib.exc.UnknownHashError: hash could not be identified

Expected (per the issue): verify_password should fail closed and return False, not raise.

Control run, to isolate that this is specifically about the hash format and not just "wrong password" in general — a real bcrypt hash with the wrong password still returns False cleanly:

$ .venv/bin/python -c "
from core.security import verify_password, hash_password
real_hash = hash_password('correct_password')
print(verify_password('wrong_password', real_hash))
"
False

Also ran the existing suite for confirmation: pytest tests/unit/test_security.py -v -m unit → 24 passed, 1 xfailed. The one xfail is test_verify_with_wrong_hash_format, marked strict=True with the reason "issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False" — same failure, same wording as what I hit directly above.

One thing worth flagging so it doesn't get mistaken for part of the bug: passlib prints a harmless "(trapped) error reading bcrypt version" warning on this bcrypt version (a known passlib/bcrypt metadata-reading quirk). It's unrelated — the control run above still returns False correctly despite it.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. `--limit 3` (pkg-01–03): `agreement: 2/3 scored items` — pkg-03 disagreed: "Discloses AI
   when required" treated any AI-related repo policy as a disclosure mandate, wrongly
   failing a repo whose policy only requires human-authored comments, not a disclosure
   statement.
2. `--only pkg-03,pkg-05,pkg-07,pkg-20` (after narrowing the disclosure check to fire only on
   explicit disclosure mandates): `agreement: 3/4 scored items` — pkg-05 newly disagreed, on
   a different, pre-existing check ("Follows template"), which penalized a repro comment for
   not literally echoing a bug-*filing* template's field names.
3. `--only pkg-05` (after removing "Follows template" as its own check — no gold label in the
   set turns on template literalism, and the substance it asked for was already covered by
   Environment/Steps/Behavior): `agreement: 1/1 scored items`
4. `--only pkg-01,pkg-03,pkg-05,pkg-07,pkg-09,pkg-10,pkg-11,pkg-12` (all 8 gold-accept
   packages, after raising "Claim is specific, not boilerplate" from `preferred` to
   `required`, since gold's reject on pkg-19 turns entirely on that check): `agreement: 6/8
   scored items` — pkg-09 and pkg-10 (the two honest cannot-reproduce accepts) newly
   disagreed, both failing "Behavior matches issue," which assumed every report claims
   success.
5. `--only pkg-09,pkg-10` (after splitting "Behavior matches issue" into a reproduced-branch
   and a cannot-reproduce-branch): `agreement: 2/2 scored items`
6. `--limit 20`, full unsaved run: `agreement: 20/20 scored items (bar: 18/20: PASS)`
7. Final official run, `--save-run eval-run.txt`: `agreement: 20/20 scored items (bar: 18/20:
   PASS)`

**Package analysis**

pkg-09: my rubric's final verdict is `accept`; the gold label is `accept`; they agree.

This wasn't always true. Before I split "Behavior matches issue" into two branches, my
rubric graded pkg-09 as `reject`. The check's pass condition only had one shape: pass if the
artifact shows the same fault as the issue, fail if it shows the feature "working correctly."
pkg-09 is an honest cannot-reproduce — the student tried to trigger a `--exec-batch`
command-ordering bug in `fd`, ran it 5 times plus an argument-padding variant, and in every
run the commands stayed in order, which is exactly what "working correctly" looks like. My
original wording read that as a fail, which directly contradicted my own "Honest outcome"
check, which explicitly wants an evidenced cannot-reproduce to pass. I rewrote "Behavior
matches issue" to read the report's stated outcome first: if it claims success, judge the
artifact against the issue's fault; if it claims it could not reproduce, judge only whether a
real attempt artifact is shown (it is — the marker-order log and the padding variant), not
whether the bug appeared.

**Check rationale**

The current pass condition for `Behavior matches issue` reads:

> "First read what the report claims. If it claims the issue's behavior occurred (fully or
> partially): pass if the artifact shown is the same fault the issue describes; fail if the
> artifact shows a different, merely adjacent fault narrated as a match. If it claims it
> could NOT reproduce the issue: pass if a real attempt artifact is shown, documenting what
> was tried and what actually happened, even though — correctly — it does not exhibit the
> issue's behavior; fail if no such artifact is given at all (a bare "could not reproduce"
> with nothing shown). Unclear only if the report's own stated outcome is itself ambiguous
> about what it is claiming"

It reads this way because the single-branch version I started with only had room for "the
artifact matches the issue" or "the artifact doesn't" — right for a report claiming success,
wrong for an honest cannot-reproduce, where the artifact is *supposed* to show the feature
behaving correctly. That version failed both pkg-09 and pkg-10 outright. I rejected keeping a
single pass condition and instead branched on the report's own stated outcome, so an
evidenced cannot-reproduce is judged on whether a real attempt is shown, not on whether the
bug appeared.

**Trade-offs**

Making "Claim is specific, not boilerplate" `required` (it started as `preferred`) is a
tightening, and tightening can turn a previously-fine package into a new false reject. Before
spending a confirming full run, I re-ran all 8 gold-`accept` packages as canaries (`--only
pkg-01,pkg-03,pkg-05,pkg-07,pkg-09,pkg-10,pkg-11,pkg-12`) specifically to check whether any
of their claim comments were weak enough to trip the now-gating check. None were — the run
agreed on all 8 (aside from the separate pkg-09/pkg-10 issue caught in the same run, on an
unrelated check). That confirms the tightening doesn't cost anything on this set, but it does
mean any future package with an otherwise-perfect repro paired with a generic, boilerplate
claim will now correctly reject — which is exactly the case (pkg-19) that made the tightening
necessary in the first place.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
