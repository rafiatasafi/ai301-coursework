# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

````
Accepted, in fit order:

#72 — verify_password raises UnknownHashError on malformed stored hashes — best fit: it's
a production-grade Python backend fix in core/security.py (fail-closed error handling on a
real auth path, labeled api), which is squarely the backend-architecture/Python growth you
named, and it involves no infra or hardware work.
  maintainer-alive: pass — last 5 commits all by Aburke225 (User, non-bot), newest 2026-09-16
  repo-in-use: pass — archived: false, pushed 2026-09-16 (within 90 days)
  bounded-newcomer-scope: pass — one bug, two named files, "Estimated effort: 1–2 hours",
    no debate, no umbrella framing
  not-already-claimed: pass — assignees: [], no linked PRs (repo has 0 PRs), 0 comments
  contribution-policy-compatible: pass — no AI mention anywhere in docs/CONTRIBUTING.md or
    the PR template; silence passes
  issue-vetted: pass — 4 labels (bug, good first issue, api, tier-1), opener is COLLABORATOR
  friendly-signal (preferred): pass — good first issue label + COLLABORATOR opener
  existing-test-coverage (preferred): pass — tests/unit/test_security.py has 25 tests, incl.
    the @pytest.mark.xfail at line 218 naming this issue

#64 — Relevance scorer "partial overlap" test fixture has full overlap — also a clean accept
with identical check grades, but it ranks second on fit: the whole change lives in a test
fixture (tests/unit/test_relevance_scorer.py:47), so it exercises less of the backend Python
you want to build up.
  maintainer-alive: pass — same commit evidence
  repo-in-use: pass — same repo facts
  bounded-newcomer-scope: pass — one fixture to correct, with a repro command and observed
    failure (assert 1.0 < 0.9)
  not-already-claimed: pass — assignees: [], no linked PRs, 0 comments
  contribution-policy-compatible: pass — no stated AI policy
  issue-vetted: pass — 4 labels (bug, good first issue, tests, tier-1), opener is COLLABORATOR
  friendly-signal (preferred): pass — good first issue label + COLLABORATOR opener
  existing-test-coverage (preferred): pass — tests/unit/test_relevance_scorer.py has 19 tests

No rejections. The house rule about classmate claim comments didn't come into play — neither
issue has a single comment yet.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "All 5 most recent main commits authored by Aburke225 (User, non-bot), newest 2026-09-16T21:42:18Z"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=False and pushed_at=2026-09-16T21:50:20Z, 4 days before the 2026-09-20 grading date"},
      {"name": "bounded-newcomer-scope", "grade": "pass", "evidence": "One fix in core/security.py plus removing an xfail marker; body states 'Estimated effort: 1-2 hours'; no comments, no design debate, no umbrella framing"},
      {"name": "not-already-claimed", "grade": "pass", "evidence": "assignees: [], 0 comments, and the repo has 0 pull requests of any state so no linked PR exists"},
      {"name": "contribution-policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI/generated/disclosure mention and .github/PULL_REQUEST_TEMPLATE.md has no AI-disclosure field; silence passes"},
      {"name": "issue-vetted", "grade": "pass", "evidence": "Labels ['bug','good first issue','api','tier-1'] and opener Aburke225 has author_association COLLABORATOR"},
      {"name": "friendly-signal", "grade": "pass", "evidence": "Carries the 'good first issue' label and was opened by a COLLABORATOR"},
      {"name": "existing-test-coverage", "grade": "pass", "evidence": "tests/unit/test_security.py has 25 test functions including the xfail at line 218 referencing issue #72 (manifest H-05)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/64",
    "checks": [
      {"name": "maintainer-alive", "grade": "pass", "evidence": "All 5 most recent main commits authored by Aburke225 (User, non-bot), newest 2026-09-16T21:42:18Z"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "archived=False and pushed_at=2026-09-16T21:50:20Z, 4 days before the 2026-09-20 grading date"},
      {"name": "bounded-newcomer-scope", "grade": "pass", "evidence": "One test fixture to correct, with repro 'pytest tests/unit/test_relevance_scorer.py -q' and observed 'assert 1.0 < 0.9'; no comments, no design debate"},
      {"name": "not-already-claimed", "grade": "pass", "evidence": "assignees: [], 0 comments, and the repo has 0 pull requests of any state so no linked PR exists"},
      {"name": "contribution-policy-compatible", "grade": "pass", "evidence": "docs/CONTRIBUTING.md has no AI/generated/disclosure mention and .github/PULL_REQUEST_TEMPLATE.md has no AI-disclosure field; silence passes"},
      {"name": "issue-vetted", "grade": "pass", "evidence": "Labels ['bug','good first issue','tests','tier-1'] and opener Aburke225 has author_association COLLABORATOR"},
      {"name": "friendly-signal", "grade": "pass", "evidence": "Carries the 'good first issue' label and was opened by a COLLABORATOR"},
      {"name": "existing-test-coverage", "grade": "pass", "evidence": "tests/unit/test_relevance_scorer.py has 19 test functions, including test_query_with_partial_overlap at line 47"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--limit 3` (issues 01–03): `agreement: 2/3 scored items`
2. `--only issue-01` (after loosening `bounded-newcomer-scope`): `agreement: 1/1 scored items`
3. `--limit 10` (issues 01–10): `agreement: 10/10 scored items`
4. `--only issue-11,...,issue-20`: `agreement: 7/10 scored items`
5. `--only issue-15,issue-19,issue-20` (after adding `issue-vetted` + abandonment-history
   clause): `agreement: 1/3 scored items`
6. `--only issue-15,issue-19,issue-20` (after excluding "candidate causes" from scope
   failure): `agreement: 2/3 scored items`
7. `--limit 20`, full unsaved check: `agreement: 20/20 scored items  (bar: 18/20: PASS)`
8. Final official run, `--save-run eval-run.txt`:
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`

**Issue analysis**

issue-15: my rubric's final verdict is `reject`; the gold label is `reject`; they agree.

This wasn't always true. Before I added a check for abandonment history, my rubric graded
issue-15 as `accept`, because every individual required check passed on its own terms:
`not-already-claimed` saw "assignees: none" and two closed PRs and called it unclaimed;
`bounded-newcomer-scope` saw one coherent feature and called it bounded. What my rubric
missed was the pattern across those facts together: the `not-already-claimed` evidence for
this issue actually read "at least 8 claim-then-unassign cycles (2021-2024) plus two
closed-unmerged linked PRs" — eight different people had tried and dropped this, which the
evidence guide names directly as a signal about real difficulty, not just current
availability. I added a clause to `bounded-newcomer-scope` specifically for a repeating
pattern of abandoned attempts, and the check now fails issue-15 for that reason instead of
missing it.

**Check rationale**

The current pass condition for `bounded-newcomer-scope` reads:

> "Fails if the issue is explicitly framed as an umbrella/tracking issue meant to be split
> into separate work items (not merely a list of related sub-tasks under one cohesive
> change), if the thread shows unsettled design debate with no maintainer decision, if a
> maintainer states the fix touches core internals, if the issue is a usage question rather
> than a requested change, or if the issue's history shows a repeating pattern of abandoned
> attempts (multiple closed-unmerged PRs, or several claim-then-unassign cycles). A bug
> report that lists several candidate causes or possible fixes for one problem is still one
> bounded task, not multiple — it does not fail this check merely for naming more than one
> idea. Otherwise passes — brevity or missing repro steps alone do not fail this check"

This went through three revisions. It started too strict (any list of sub-items failed it,
which wrongly rejected a cohesive multi-file documentation update). I loosened it, which
then let through a genuinely difficult issue with an abandonment history, so I added the
abandonment clause. That in turn made the check briefly over-trigger on a bug report that
just listed several candidate causes for one problem, so I added the last carve-out
distinguishing "multiple ideas for one fix" from "multiple separate tasks."

**Trade-offs**

The "several candidate causes ≠ multiple tasks" carve-out trades away some ability to catch
issues that really are too broad because they bundle several genuinely large, independent
fixes disguised as "options" for one bug. I re-ran issue-19 with `--only` after adding this
carve-out and got a different verdict (`reject`) from what the same wording produced on a
later full run (`accept`) — the boundary here still depends partly on model judgment
variance on this specific issue, not purely on the check's wording, which is a limit worth
naming rather than papering over.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit to interests and time available.** I've worked on similar bugs before — malformed
   or unexpected input hitting a code path that doesn't handle it gracefully — so I have a
   real sense of what this fix will involve. Backend work specifically is an area I'm trying
   to grow in, since my day-to-day leans more full-stack/frontend, and this is a small,
   contained backend fix rather than a sprawling one, so it fits well within the time I have
   for Unit 2.

2. **What the verdict identified correctly, and what I weighed that the rubric could not.**
   The verdict correctly identified this as bounded, unclaimed, and squarely backend Python
   — matching what I was looking for. Beyond that, I preferred working with a real
   security/auth code path specifically over a test-fixture fix like #64: getting
   comfortable reading and safely modifying auth-adjacent code feels like a more direct way
   to build the backend confidence I'm after, even though both issues were equally valid
   accepts from the rubric's perspective.

3. **Anticipated difficulty in claiming it.** The repo currently has zero PRs and zero
   comments on this issue, so there's no established pattern yet for how review or feedback
   works here — that's a small unknown going in. The fix itself touches `core/security.py`,
   an auth code path, so even though the scope is small I'll want to be precise rather than
   just fast. I also need to make the existing `@pytest.mark.xfail`-marked test actually
   pass once I remove the marker, which gives me a concrete, unambiguous bar for "done"
   rather than something I have to judge myself.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
