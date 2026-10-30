# Procedure: how this tool grades a PR package

## Read order

1. Read the **Issue** first: title, body, and the exact reported
   behavior. This is "the reported behavior" — every later check
   measures the fix against this, not against the plan's own framing
   of it.
2. Read the **Plan** next (`plan.md`, including its `## Deviations`
   section if present): its stated scope, its test plan, and any
   recorded deviation with its reason. Note the scope as a list of
   named files/areas and a one-sentence description of the intended
   change. Note the test plan as a list of specific, checkable
   verification steps.
3. Read the **Diff** next (`git diff main...HEAD`), before the PR
   description. Note every changed file and, for each, whether its
   content plausibly serves the plan's named scope or looks unrelated
   (a different file, a debug print, commented-out code).
4. Read the **repo's PR template and docs/CONTRIBUTING.md** next.
   Record every section the template requires and classify the AI
   policy exactly as Unit 3's procedure did: no policy, permissive/no
   disclosure asked, authorship-only, or explicit disclosure mandate.
5. Read the **draft PR title and description** last among the
   package's non-evidence parts. Note what it claims the diff does,
   section by section.
6. Read the **test evidence** (`test_evidence.md` or the description's
   Testing section) last of all, alongside the plan's test plan from
   step 2.

Reading the diff before the description matters: you need to know
what the diff actually does before the description's own claims about
it can anchor you into accepting its account.

## Evidence gathering

Gather all seven before grading any of them:

1. **diff-within-plan-scope**: from the diff's changed files (step 3),
   mark each as *inside* the plan's named scope, *outside* it but
   recorded in `## Deviations` with a reason, or *outside* it and
   unrecorded.
2. **description-matches-diff**: from the description's Changes
   section, list each claimed change. Cross off each one you can
   verify against the actual diff. Anything left unchecked (claimed
   but absent) or anything in the diff with no corresponding claim
   (present but unnamed) is a mismatch.
3. **evidence-demonstrates-fix**: for each test-plan step from read
   order step 2, find its corresponding before/after pair in the
   pasted evidence. Mark each as *observable-and-matching* (shows the
   exact before/after the plan predicted), *observable-but-different*,
   or *absent* (no pasted output for that step).
4. **repo-checks-run-and-honestly-reported**: for each required check
   named in the repo's PR template, find its pasted result in the
   Testing section. Mark each as *run-and-pastedresult*,
   *run-and-honestly-reported-failure*, or *missing/asserted-without-
   output*.
5. **diff-is-reviewable**: from step 3's file-by-file notes, list
   every file or hunk marked unrelated or debris-like.
6. **template-fully-completed**: for each template section from read
   order step 4, mark it *filled*, *placeholder/blank*, or
   *unexplained N/A*.
7. **discloses-ai-when-required**: take the AI-policy classification
   from read order step 4. If it is "explicit disclosure mandate,"
   search the description's Notes for Reviewers (or equivalent) for a
   disclosure statement. For the other three classifications, no
   further search is needed.

## Check execution

1. Grade all seven checks; none are skipped or deferred.
2. Grade `pass` only when gathered evidence directly supports the
   pass condition; grade `fail` when it directly contradicts it.
   Grade `unclear` only when the package gives nothing to gather at
   all for that check (no scope stated, no description content, no
   test evidence section) — not when what's there is merely terse.
3. Apply the disclosed-gap exception exactly as `rubric.md`'s verdict
   rule states it, for the three checks it names there. Do not
   re-derive or restate the condition here -- if the exception's
   wording ever needs to change, it changes in `rubric.md` only.
4. Once a check's evidence is gathered per Evidence gathering, grade
   it from that record; do not re-read the whole package per check.
5. For every grade, record the one fact that decided it: a quoted
   diff line, a quoted description line, or a quoted plan line.

## Verdict assembly

1. Apply the verdict rule exactly: `accept` only if all seven required
   checks grade `pass`.
2. A single `fail` (or `unclear`, counted as `fail`) on any required
   check holds the package at `reject`, regardless of the rest.
3. When more than one check fails, name all of them in the output, not
   just the first found.
4. The evidence quoted for each failing check in the final output is
   the same fact recorded during Check execution step 5 — never a new
   justification invented at verdict time.
