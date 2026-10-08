# Rubric: is this pull request ready to submit?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-within-plan-scope | The diff's changed files and behavior, read against the plan's stated scope and its `## Deviations` section | Pass if every change in the diff falls inside what the plan said it would do, or is recorded as a deviation with a stated reason; fail if the diff does something the plan neither claimed nor disclosed as a deviation (more, less, or different work); unclear only if the plan states no scope at all | required |
| description-matches-diff | The PR description's Summary/Changes sections, read against the actual diff | Pass if every change the description claims is actually present in the diff, and every substantive diff change is named in the description; fail if the description claims something the diff doesn't do, or omits a change the diff makes; unclear only if the description has no Changes content at all | required |
| evidence-demonstrates-fix | The description's Testing section (or `test_evidence.md`), read against the plan's test plan | Pass if the pasted before/after output actually shows the stated problem occurring before and resolved after, for every case the plan's test plan named; fail if the evidence is a claim of success with no observable before/after, or shows a different behavior than what the plan's test plan specified | required |
| repo-checks-run-and-honestly-reported | The description's Testing section, read against the repo's required checks (its PR template's checklist, e.g. unit tests, integration tests, lint, typecheck) | Pass if every required check was actually run and its real result pasted, including an honestly-reported failure with a stated reason; fail if a required check is silently skipped, or its result is asserted without pasted output, or a failure is hidden | required |
| diff-is-reviewable | The full diff, read for files/hunks unrelated to the plan's named scope | Pass if the diff contains only the change the plan describes — no debug prints, no commented-out code, no unrelated file touches; fail if any such debris or unrelated hunk is present, even if the core fix is otherwise correct | required |
| template-fully-completed | The PR description, read against the repo's stated PR template (every section the template defines) | Pass if every template section has real, specific content (not a placeholder, not "N/A" without a stated reason); fail if any template section is blank, left as placeholder text, or given an unexplained "N/A" | required |
| discloses-ai-when-required | The PR description's Notes for Reviewers (or equivalent), read against the repo's stated AI-use policy | Pass vacuously if the repo states no AI policy, or one that does not demand a disclosure statement; pass if the policy demands disclosure and the description includes a plain, specific disclosure of how AI was used; fail only if the policy explicitly requires disclosure and the description contains none | required |

## Verdict rule

Accept (submit) only if every required check grades `pass`. A single
`fail` on any required check holds the package at `reject`, regardless
of how the others grade. `unclear` counts as `fail` on every required
check: a claim this tool cannot verify from the package is not ready
to submit. An honestly disclosed shortfall is not automatically a
fail — `diff-within-plan-scope`, `evidence-demonstrates-fix`, and
`repo-checks-run-and-honestly-reported` are each written to pass a
disclosed, reasoned gap; what fails them is an undisclosed one.
