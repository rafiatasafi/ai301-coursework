# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

Where it lives: in an eval bundle, the plan's stated cause (usually
under a "Diagnosis" heading, sometimes just the plan's opening
sentence), read against every fact in the **Repro evidence** section —
environment, steps, artifacts, and especially control runs. A control
run is the strongest evidence here: it isolates one variable, so if the
plan's stated cause requires that variable to matter and the control
shows it doesn't, the diagnosis is directly contradicted, not just
unsupported. In live mode: the same, with repro evidence coming from
the student's own posted repro comment (or the house repro pack) on
the issue thread.

What good looks like: the stated cause explains every observed fact,
including the control runs — not just the headline symptom. "Plan
blames the request-item tokenizer; the control run (same items, no `-v`
flag) parses fine" is a direct contradiction: the diagnosis says the
tokenizer is at fault, but the control shows the same items parse
correctly under different conditions, which a tokenizer-level bug
would not allow. A diagnosis that never engages the control runs at all
— that only restates the symptom as if it were the cause ("the git
modules have performance problems") — is not yet a diagnosis; whether
that counts as "no diagnosis stated" or a diagnosis this check then
fails depends on whether it names a mechanism. "Performance problems"
names no mechanism; "the TTL constant is never read, so the cache never
invalidates" does.

## Scope

This family splits into two independently-graded checks, because a
plan can be wrong in either direction without being wrong in the other:
the stated exclusions can be accurate while the *included* work is
still a rewrite, and vice versa.

**scope-bounded** — Where it lives: the plan's full list of proposed
changes, wherever it appears (a "Proposed changes" list, numbered
steps, prose) — not just the files named, but the stated ambition
("fix the pipeline properly", "address this class of bug", "while
we're at it"). Read it against the Issue and Repro evidence: what is
the narrowest change that would make the reported, reproduced behavior
go away? What good looks like: the plan's work is that narrow change,
even if it touches two or three related call sites. A plan that adds
a version bump unrelated to the bug, a new cross-runtime abstraction, a
CI matrix, or explicitly reframes a single bug as evidence of a
systemic problem it will now also fix, is scope the evidence never
asked for — "the preload/save pipeline for containerd is structurally
unsound... this plan addresses the pipeline as a whole" is the
tell: the issue and its repro evidence are about one empty-tar bug, not
a pipeline.

**scope-exclusions-accurate** — Where it lives: wherever the plan
describes what it will NOT do. This is not always the literal phrase
"not in scope" — it also reads as "Explicitly deferred, with reasons:
...", "I cannot test X so I am leaving it out", or "leave to a
follow-up." Read each exclusion against the Repro evidence and Issue.
What good looks like: the excluded area really is uninvolved (nothing
in the evidence implicates it), or the plan gives a real, stated reason
for deferring it (an environment the author cannot test, a cost
trade-off flagged for later measurement) rather than just waving it
away. A plan with no scope statement of any kind, in any phrasing, has
nothing to grade here — that is `unclear`, which this rubric's verdict
rule treats as `fail`.

## Executability

Where it lives: the plan's approach/files section — whatever names (or
fails to name) concrete files, directories, or functions, and whatever
names (or fails to name) a single chosen method at each decision point.

What good looks like: a stranger holding only this plan could start
working without asking the author anything. Every real decision is made
in the plan, not deferred to build time: "upstream or vendored,
whichever turns out to be easier" and "gocui? tcell? not sure" are both
decisions the plan should have made and did not. A restated goal with
no concrete action under it ("speed it up", "make the panic go away",
"investigate the input stack") is not an approach, even if it is
confidently worded. Good executability often reads short: "one-line
change in the command builder... in `crates/cli/src/decompress.rs`" is
fully executable in one sentence, while a much longer plan that never
names a file or commits to a path is not.

## Test plan

Where it lives: the plan's Test plan section, read against two things
at once: the plan's own stated diagnosis, and the Repro evidence's
artifacts (for what a "before" looks like).

What good looks like, checked both ways: first, the proposed
verification actually exercises the stated cause — re-running the
exact repro steps and checking the specific thing the diagnosis says
was wrong, not a looser re-run that would also pass if a different bug
were fixed. Second, the named outcome is something a stranger could
check pass/fail without asking the author: an exit code, an assertion,
a specific log line's absence, a metric crossing a stated threshold.
"The prompt should feel fast" and "should look much better" name no
such line — nobody but the author could judge whether either passed.
"`rg -z sometext` in the fixture directory prints a match... exit 0"
does.

## Honesty

Where it lives: this family has no dedicated required check this
week — every package in the practice set where overconfidence alone
decided the verdict turned out to already fail `diagnosis-matches-evidence`
or `scope-bounded` on the merits. But it is what makes those two checks
readable, so read for it anyway: risks and unknowns, usually under a
"Risk" heading or a sentence naming what the author has not yet
verified, read against how certain the rest of the plan's language is.

What good looks like: a stated unknown is treated as a fact to plan
around, not hidden. "I have not yet measured the per-print cost of the
generation comparison; if it shows up in the print benchmark I will
move the check... and I flag that trade-off for review" is an honest
unknown with a stated fallback — it does not weaken the plan, because
it tells a reader exactly what to watch for. A plan that is "long and
confident" while its own step-4 evidence already shows the stated cause
can't be right is false certainty dressed up as diagnosis — which is
exactly what `diagnosis-matches-evidence` catches directly, so grade it
there rather than inventing a separate "sounds too sure" check.

## Comms

This family also splits into two independently-graded checks, matching
the practice set's `thread-convention` category directly: one package
in it fails only on thread engagement, the other fails only on
disclosure.

**engages-thread-direction** — Where it lives: every comment in Thread
highlights from an account with maintainer standing (OWNER, MEMBER,
CONTRIBUTOR, COLLABORATOR — not NONE) that proposes, confirms, or
rejects a direction, a cause, or an approach. Read the plan and plan
comment against each one. What good looks like: if a maintainer already
pointed at a file, confirmed a diagnosis, or rejected an approach as
too expensive, the plan either follows that lead or explicitly diverges
from it with a stated reason — "my plan follows the direction proposed
here: a page generation counter..." or "I saw the open PR #3314... my
intent is not to race it" are both engagement, one adopting and one
coordinating. A plan that proposes something adjacent (a docs-only
workaround) while the thread already named the actual culprit file and
never mentions that the thread said anything is the failure this
catches — not because the docs plan is bad on its own terms, but
because it silently ignores direction that was already given. No
maintainer comment in the thread at all means this check has nothing to
fail on: grade it `pass`, not `unclear`.

**discloses-ai-when-required** — Where it lives: the repo-facts block's
stated AI-related policy, read against the plan comment's text for an
explicit disclosure line. Sort the policy before grading, same as
week 2: no policy, or a permissive one that never asks to be told, both
pass with nothing to check; an authorship-only rule ("comments must be
in your own words") is not a disclosure mandate either. Only an
explicit instruction to disclose AI assistance makes this check live —
and when it is live, silence is a fail, not a neutral: "ghostty's
stated policy requires disclosing all AI usage and the comments do not
disclose" is the exact failure shape, on an otherwise excellent,
thread-following plan.
