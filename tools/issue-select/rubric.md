# Rubric: is this a good first issue?

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer-alive | Repo facts: "last 5 default-branch commits" (author + date); "maintainer first-response sample" | At least one of the last 5 default-branch commits is authored (not just merged) by a human, non-`[bot]` account, OR the maintainer first-response sample shows a reply from an Owner/Member/Collaborator within 30 days | required |
| repo-in-use | Repo facts: "latest release" date, "last push to any branch", "archived:" flag | `archived:` is not true, AND at least one of (latest release within 180 days) or (last push within 90 days) is true | required |
| bounded-newcomer-scope | Issue body and Comments section | Fails if the issue is explicitly framed as an umbrella/tracking issue meant to be split into separate work items (not merely a list of related sub-tasks under one cohesive change), if the thread shows unsettled design debate with no maintainer decision, if a maintainer states the fix touches core internals, if the issue is a usage question rather than a requested change, or if the issue's history shows a repeating pattern of abandoned attempts (multiple closed-unmerged PRs, or several claim-then-unassign cycles). A bug report that lists several candidate causes or possible fixes for one problem is still one bounded task, not multiple — it does not fail this check merely for naming more than one idea. Otherwise passes — brevity or missing repro steps alone do not fail this check | required |
| not-already-claimed | Repo facts: "this issue: assignees:"; "linked PRs:" with state; Comments section | Fails if assignees is non-empty, OR a linked PR is open, OR a comment claims the work ("I'll take this" / "working on this") with no maintainer pushback and no sign of abandonment (e.g. closed unmerged PR, stale claim). Otherwise passes | required |
| contribution-policy-compatible | Repo facts: "contribution policy" line (from CONTRIBUTING.md, AI_POLICY.md, or PR/issue template AI-disclosure fields) | Fails only if the policy states an outright ban on AI-generated/AI-assisted contributions. Conditions (disclosure, personal understanding, testing, human review) pass. Silence (no stated policy) passes | required |
| issue-vetted | Repo facts: issue labels; Comments section (author_association of any commenter); opener's author_association | Fails if the issue has zero labels, zero comments, AND the opener's author_association is NONE or the opener is a bot account. Otherwise passes | required |
| friendly-signal | Issue labels; Comments section author_association of the issue opener | Passes if the issue carries a label such as good-first-issue/help-wanted/beginner-friendly, OR the issue was opened by an Owner/Member/Collaborator | preferred |
| existing-test-coverage | Repo facts / repo structure for the touched area | Passes if the area the issue touches has existing tests, per repo facts | preferred |

## Verdict rule

Accept only if every required check passes. `unclear` on any required check counts as fail (per the default). Preferred checks never change the verdict — they only rank issues that already passed every required check, more preferred passes ranking higher.
