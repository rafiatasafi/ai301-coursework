# Rubric: is this reproduction package ready to post?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment section (OS/runtime, dependency or app version, repo commit/release), read against the specific version or platform the issue names | Pass if the recorded environment matches what the issue targets, or the report explicitly reconciles a difference; fail if no environment is recorded, or it silently contradicts the issue's target; unclear only if the issue itself names no specific environment and none is recorded | required |
| Steps followable | The repro report's step-by-step section, read for a starting state and each action taken | Pass if a stranger with only the recorded environment and these steps could execute them in order and reach the same trigger point, with no unstated setup assumed; fail if a step skips a needed action, assumes something not listed, or is out of order; unclear only if no steps are given at all | required |
| Behavior matches issue | The repro report's output/log/screenshot excerpts, read against the exact behavior the issue describes (error text, symptom, expected vs. actual), and the report's own stated outcome (reproduced vs. could not reproduce) | First read what the report claims. If it claims the issue's behavior occurred (fully or partially): pass if the artifact shown is the same fault the issue describes; fail if the artifact shows a different, merely adjacent fault narrated as a match. If it claims it could NOT reproduce the issue: pass if a real attempt artifact is shown, documenting what was tried and what actually happened, even though — correctly — it does not exhibit the issue's behavior; fail if no such artifact is given at all (a bare "could not reproduce" with nothing shown). Unclear only if the report's own stated outcome is itself ambiguous about what it is claiming | required |
| Honest outcome | The report's stated conclusion (reproduced / could not reproduce / partially reproduced), read against the artifacts actually shown above it | Pass if the conclusion claims exactly as much as the evidence shows, including an evidenced cannot-reproduce; fail if the report asserts reproduction the artifacts don't show, or asserts a cause/diagnosis the artifacts don't establish | required |
| Discloses AI when required | The claim/repro comments, read against the repo's stated AI-related policy (the repo-facts block in eval mode), specifically for whether that policy demands a disclosure *statement* | Pass vacuously if the repo states no AI policy, or states one that does not itself demand a disclosure statement (e.g., a policy that restricts what may be posted — "comments must be written by humans in their own words," "AI-generated comments may be hidden" — without asking contributors to declare AI use); pass if the policy does demand a disclosure statement and the comment includes one; fail only if the policy explicitly requires disclosing AI assistance and the comment contains no such disclosure | required |
| Claim is specific, not boilerplate | The claim comment, read against the issue's own title and description | Pass if the claim names something concrete from this specific issue and states a concrete next action without asserting a fix or a date; fail if the claim is generic enough to paste onto any issue unchanged, or promises more than investigation | required |

## Verdict rule

Accept if every `required` check grades `pass`. A `fail` on any
`required` check holds the package at `reject`, regardless of how the
other checks grade. `preferred` checks are reported but never flip the
verdict. `unclear` on a `required` check counts as `fail` (proof you
cannot verify is proof that is not ready to post), except where
SKILL.md's claim-only-draft rule marks a check `unclear: not yet
applicable` and excludes it from the verdict rule entirely.
