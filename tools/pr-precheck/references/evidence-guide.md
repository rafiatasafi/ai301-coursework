# Evidence guide: where proof lives in a PR package

## Plan fidelity

Where it lives: in a bundle, the Plan section's stated scope and its
`## Deviations` notes, read against the Diff section's changed files.
In your own work, `plan.md` (including `## Deviations`) read against
`git diff main...HEAD` on your branch.

What good looks like: every file the diff touches is either named in
the plan's scope or recorded in `## Deviations` with a stated reason.
A diff that quietly does more or less than the plan, with no
deviation note, is drift — the single most common way a PR goes from
ready to hold.

## Test evidence

Where it lives: in a bundle, the package's Test evidence section, read
against the Plan's stated test plan. In your own work, the
description's Testing section (or `test_evidence.md`) read against
`plan.md`'s Test plan section.

What good looks like: for every verification step the plan named, a
pasted command and its real before/after output — not a sentence
asserting it passed. The repo's own required checks (unit tests,
integration tests, lint, typecheck) are run and their actual results
pasted, including an honest failure with a reason, not a silently
skipped or glossed-over one.

## Diff quality

Where it lives: in a bundle, the Diff section itself. In your own
work, `git diff main...HEAD --stat` for a quick file list, then the
full diff for content.

What good looks like: the diff contains only the files and hunks the
plan's scope calls for. No debug prints left in, no commented-out
code, no unrelated file touched "while I was in there." A diff that
quietly expands beyond the plan is unreviewable even when the core fix
is correct, because a reviewer cannot tell which parts are the change
and which are noise.

## Standards and comms

Where it lives: in a bundle, the repo-facts block's stated PR template
and AI-use policy, read against the candidate PR's title and
description. In your own work, the repo's actual
`.github/PULL_REQUEST_TEMPLATE.md` and `docs/CONTRIBUTING.md`, read
against your draft in `pr_draft.md`.

What good looks like: every section the template defines has real,
specific content — not a placeholder left in, not an unexplained
"N/A." If the repo's policy requires disclosing AI assistance, the
description states plainly how AI was used and what was independently
verified, in the reviewer-facing text itself, not a separate unlinked
place.
