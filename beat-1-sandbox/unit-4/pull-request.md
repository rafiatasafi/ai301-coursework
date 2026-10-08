# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/110

**Branch**

fix/72-verify-password-fail-closed

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, `--save-run eval-run.txt`: `agreement: 13/20 scored items (bar: 18/20: below the bar; category floor unmet: no match in clear-accept)`
2. `--only pkg-02,pkg-08` (diagnostic, no files changed): `agreement: 1/2` — pkg-02 passed (model variance from the full run, where it had also failed on `evidence-demonstrates-fix` with no later change); pkg-08 failed again, isolating the cause to `repo-checks-run-and-honestly-reported` specifically, not `evidence-demonstrates-fix`

**Package analysis**

pkg-08: my rubric's final verdict is `reject`; the gold label is `accept`; they disagree.

The failing check, `repo-checks-run-and-honestly-reported`, found that the PR's Requirements section states "code formatted" with no pasted gofmt or lint output anywhere in the description or test evidence — unlike the tests and integration-test checklist items in the same PR, which do have pasted command output ("go test" and the integration test run). My check's pass condition treats every template checklist item identically: it requires pasted command output for each one, with no pasted output counting as "silently skipped." But "code formatted" in this repo's template reads more like a self-attested tick-box (did you run your formatter before committing) than a "run this and show me" item the way the test checks are. Gold treats this PR as ready; my rubric can't currently tell the difference between a checklist item that demands pasted output and one that's a self-attestation, so it fails every PR that completes the self-attestation item honestly without extra proof — which turned out to affect every clear-accept package in the set, not just this one.

**Check rationale**

The current pass condition for `repo-checks-run-and-honestly-reported` reads:

> "Pass if every required check was actually run and its real result pasted, including an honestly-reported failure with a stated reason; fail if a required check is silently skipped, or its result is asserted without pasted output, or a failure is hidden"

It reads this way because the honest-outcome principle from weeks 2 and 3 (an evidenced cannot-reproduce or an honestly-reported failure still passes; an unsubstantiated success claim does not) needed a PR-level equivalent: a checklist that's ticked without evidence is the PR version of a confident claim with nothing behind it. I wrote it to require pasted output uniformly across every checklist item, on the assumption that any claimed check is equally verifiable by its output — which turned out to be false for at least one item ("code formatted") in at least one repo's template.

**Trade-offs**

Requiring pasted output for every checklist item, with no distinction for a self-attested one, costs exactly what the eval run shows: a uniform fail across all 7 clear-accept packages, the one category where the bar explicitly can't be bought back on volume. The check correctly catches an unsubstantiated test claim (which is the failure mode it was built for, and which none of the reject-category packages slipped past), but it can't yet distinguish that from an honestly-ticked self-attestation item with no command to paste output from. A looser version — pasted output required only for checklist items that name a runnable command, pass-by-ticking accepted for ones that don't — would likely recover the clear-accept category without reopening the honest-outcome gap, but confirming that costs another full run, which this submission doesn't spend on.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
