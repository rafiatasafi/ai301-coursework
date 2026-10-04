# Procedure: how this skill grades a plan package

## Read order

1. Read the **Issue** section first: the title, the body, and the
   exact behavior reported as the bug. Note it as "the reported
   behavior" — every later check measures against this, not against
   whatever the plan claims the issue is about.
2. Read **Thread highlights** next, before the repro evidence or the
   plan. For each comment, note the author's association (OWNER,
   MEMBER, CONTRIBUTOR, COLLABORATOR count as maintainer signal; NONE
   does not). Record every maintainer statement that proposes,
   confirms, or rejects a direction, a cause, or an approach. This list
   is "the maintainer directions" and gates `engages-thread-direction`
   later. If there are none, record that explicitly (an empty list is
   itself a fact, not a gap to fill in later).
3. Read **Repro evidence** next, still before the candidate plan. Note,
   as a flat list of facts: the environment, each numbered step, every
   artifact shown, every control run and what it isolates, and the
   stated expected vs. actual. This list is "the evidence facts" — the
   ground truth every diagnosis/scope/test check is measured against.
   Note explicitly what the evidence rules out (a control run that
   isolates one variable rules out every variable it holds constant).
4. Read **Repo facts** next. Record the stated bug-report template
   fields and the exact wording of the contribution policy, especially
   any AI-use clause. Classify it now (see `discloses-ai-when-required`
   in `references/evidence-guide.md`) as: no policy, permissive/no
   disclosure asked, authorship-only, or explicit disclosure mandate.
5. Read the **Candidate plan** last among the package's non-comment
   parts: its diagnosis, its scope (whatever it says about what will
   and will not be done, however phrased), its approach/files, its test
   plan, and any stated risks.
6. Read the **Candidate plan comment** last of all. This is the text a
   maintainer actually sees; it is what `engages-thread-direction` and
   `discloses-ai-when-required` grade directly, and what a live-mode
   voice-guide pass checks separately.

Reading the evidence facts and maintainer directions before the plan
matters: you need to already know what "contradicts the evidence" and
"ignores the thread" look like before the plan's own framing can
anchor you into accepting its account of itself.

## Evidence gathering

Gather all seven before grading any of them, using the notes from Read
order:

1. **diagnosis-matches-evidence**: from the evidence facts, list every
   fact that bears on causation (control-run isolations, before/after
   deltas, timing numbers). From the plan's diagnosis, extract the
   causal claim in one sentence. Mark each evidence fact as
   *consistent*, *silent*, or *contradicts*.
2. **scope-bounded**: from the plan, list every proposed change, file,
   or area, however it is organized (numbered steps, prose, a "Proposed
   changes" list). Separately, from the reported behavior and the
   evidence facts, name the narrowest change that would address what
   was actually observed broken. Compare the two lists: anything in the
   plan's list that the narrower fix would not need (version bumps
   un-related to the bug, new abstractions, CI additions, a stated
   intent to fix "the whole pipeline" or "this class of bug" rather
   than the reported instance) is scope the evidence does not call for.
3. **scope-exclusions-accurate**: from the plan, find every sentence
   describing what will NOT be done, under any phrasing ("Not in
   scope", "Explicitly deferred, with reasons", "leave to a follow-up",
   "won't touch", "I cannot test X so..."). For each one, check it
   against the evidence facts: does anything gathered actually
   implicate the excluded area? A stated reason for deferring
   (untestable environment, a cost trade-off named for later
   measurement) counts as support for the exclusion even without direct
   evidence against it.
4. **executable-approach**: from the plan's approach/files section,
   record every named file, directory, or function, and every decision
   point. For each decision point, record whether exactly one path is
   chosen or whether it is left open ("upstream or vendored, whichever
   is easier", "gocui? tcell? not sure"). Record any step that is only a
   restated goal rather than an action ("speed it up", "fix the
   pipeline") with nothing underneath naming how.
5. **test-plan-targets-and-observable**: from the plan's test plan,
   record the stated verification step and its named outcome. Check it
   twice: (a) against the diagnosis from gathering item 1 — would this
   verification actually exercise that cause, not just re-trigger the
   symptom through whatever path happens to show it; (b) against the
   outcome's own wording — is it a specific, checkable condition (an
   exit code, an assertion, a metric threshold, a specific log line) or
   a subjective feeling with no pass/fail line ("should feel faster",
   "should look right").
6. **engages-thread-direction**: take the maintainer-directions list
   from Read order step 2. For each direction, check the plan and
   comment: does the chosen approach match it, or does it diverge with
   a stated reason, or does it go unmentioned while doing something
   else? Record which of the three applies for each direction.
7. **discloses-ai-when-required**: take the AI-policy classification
   from Read order step 4. If it is "explicit disclosure mandate,"
   search the plan comment for a disclosure sentence (names AI
   assistance and the human's review of it). For the other three
   classifications, no further search is needed — the check's answer
   follows directly from the classification.

## Check execution

1. Grade the seven checks in the table's order. Every check is
   `required`; there are no `preferred` checks to skip or defer.
2. Grade `pass` only when the gathered evidence directly supports the
   pass condition; grade `fail` when gathered evidence directly
   contradicts it. Grade `unclear` only when the package gives nothing
   to gather at all (no diagnosis stated, no approach section, no test
   plan) — not when what is gathered is merely terse, or thin, or less
   detailed than you would have written yourself. Terseness is not a
   failure mode this rubric grades.
3. `engages-thread-direction` has no `unclear` outcome: grade it `pass`
   whenever the maintainer-directions list from Read order is empty,
   and apply the fail condition only when that list is non-empty and
   ignored or contradicted.
4. Once a check's evidence is gathered per Evidence gathering, grade it
   from that record; do not re-read the whole package per check.
5. When the plan engages a maintainer's comment but misreads or
   misrepresents what it said, grade `engages-thread-direction` `fail`,
   not `unclear` — a mistaken engagement is not an absence of one.
6. For every grade, record the one fact that decided it: a quoted
   evidence line, a quoted maintainer comment, or a quoted repo-facts
   line. "Looks fine" or "seems thorough" is never a recorded fact.

## Verdict assembly

1. Apply the rubric's verdict rule exactly: `accept` only if all seven
   required checks grade `pass`.
2. A single `fail` (or `unclear`, which counts as `fail`) on any
   required check holds the package at `reject`, no matter how well
   the rest of the package reads.
3. When more than one check fails, name all of them in the output, not
   just the first one found while grading — the student's package
   analysis and revise loop depend on seeing every reason, not one.
4. The evidence quoted for each failing check in the final output is
   the same fact recorded during Check execution step 6 — never a new
   justification invented at verdict time.
