# Contribution: Add a Jazz beat pattern to DrumBeatRepo

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Rafia Tasafi
**GitHub Username:** rafiatasafi
**Issue:** https://github.com/Babali42/DrumBeatRepo/issues/270
**Pull Request:** [not yet opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: Built issue-scout by upgrading my unit 1 issue-select skill for real repos and running it on a live search.**

I renamed issue-select to issue-scout, replaced its sandbox scope with my real-world fit profile (Python/Flask, Next.js/React Native, Supabase -- avoiding CI/DevOps work), and wrote search-strategy.md with five gh search issues queries plus ranking filters (drop assigned/stale candidates, order by fewest comments first). My first scan graded 5 candidates and caught a real rubric bug: not-already-claimed was passing issues where a linked PR had already merged the requested work, because my pass condition only checked for open PRs and assignees, not merged ones. I rewrote the check to also fail on a merged PR and logged the fix with a dated entry in scan-notes.md, then confirmed it held up when re-grading DrumBeatRepo#270 -- the scout correctly flagged it with a tension (a standing "not meant to be closed" issue) instead of a false accept.

**Unit 6: [one line on the tool work]**

[2-4 sentences: the voice-guard, the repo-memory file. What broke or
fell short, and what changed because of it. "Nothing broke" counts when
it is true - say how you know.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

I found this issue through the required AI301 Issue Selection site, filtered to my profile (Python/Flask, TypeScript/React, Supabase). DrumBeatRepo#270 is a standing "add a drum beat" invitation carrying a good first issue label, and my issue-scout graded it accept, with one tension flagged under scope-feasible: the issue is explicitly "not meant to be closed," so it's a perpetual invitation rather than a task that finishes, though each beat genre is still its own bounded piece of work. The difficulty note came back straightforward: the issue names exactly where the change lives (a JSON file under frontend/src/assets/beats/<genre>/ plus a beat-metadata.json entry), and I'm adding Jazz specifically, reusing the repo's existing samples. It's a frontend/TypeScript codebase (Angular, not React, but close enough to my Next.js/React work that reading the existing beat schema was quick), and it's small enough to finish cleanly in the weeks I have. I hope to learn how a real maintainer actually responds to a first-time contributor's claim, since my units 1-4 work was entirely against a classroom sandbox.

My scout's grading history on this issue is itself the most honest story about the tool: after I posted my claim comment, I re-graded the issue and the scout came back reject -- not because anything about the issue had changed, but because its not-already-claimed check read my own successful claim comment (and the maintainer's approving reply) as evidence someone else had already taken the work. That's a real rubric bug: the check was written to catch a stranger beating me to an issue, with no exemption for my own account. I fixed it by excluding comments, assignees, and linked PRs that are mine from the "already claimed" condition, logged the fix in scan-notes.md, and re-ran the grade -- it came back accept again, correctly. That's the second rubric bug this scout has caught and fixed against real repos (the first was not-already-claimed also missing a merged-PR case, in my very first scan), which is exactly the pattern I expected moving from a seeded sandbox to the wild: it under-handles cases the classroom's tidy issues never exercised, but each miss has been catchable and fixable once I actually ran it against something real.

Claim comment: https://github.com/Babali42/DrumBeatRepo/issues/270#issuecomment-6725907234

**Toolkit**

- Tool: issue-scout (my search strategy, my rubric), used to grade DrumBeatRepo#270 both before and after claiming it
- What it produced: an initial accept with a scope-feasible tension (standing issue, perpetual invitation) and a straightforward difficulty note; then, after I claimed it, a reject caused entirely by a rubric bug in not-already-claimed misreading my own claim as a competing one
- Difficulties: not-already-claimed had no exemption for my own account, so a successful claim looked identical to a stranger beating me to it
- What changed: added an exemption so comments, assignees, and linked PRs that are mine never fail that check; logged it with a dated line in scan-notes.md

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

[What a stranger would need to stand this up: your operating system,
the relevant versions (language, runtime, package manager, the project
itself), and how you set the project up. Then the challenges you faced
and how you solved them, or that none came up.]

**Steps to Reproduce**

1. [Step 1]
2. [Step 2]
3. [Observed result]

**Reproduction Evidence**

- **Commit showing reproduction:** [link to commit in your fork]
- **Screenshots/logs:** [if applicable]
- **My findings:** [what you discovered during reproduction; an
  honest, evidenced cannot-reproduce that led you to re-pick or ask
  the maintainer belongs here and costs you nothing]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
