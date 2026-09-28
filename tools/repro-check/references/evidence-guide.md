# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: Unit 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; the rubric swap showed you what that feels
like. Write the map you wish your grader had.
-->

## Environment

Where it lives: In an eval bundle, the repro report's "Environment" or
"Setup" subsection, read alongside the repo-facts block for the
version(s) the issue was filed against. In live mode, the environment
lines in the student's draft repro report, checked against the issue
thread's own text (the issue body, and any maintainer replies naming a
version or platform) and the repo's README/CONTRIBUTING for supported
versions.

What good looks like: The record names the OS or platform, the
runtime/language version, the package manager, and the exact
dependency or app version — not "latest" or "current main" alone. If
that differs from what the issue names, the report says so explicitly
rather than silently substituting a different version.

## Steps

Where it lives: In an eval bundle, the repro report's numbered steps
section, read alongside the issue's own repro steps (if it gives any)
for the starting state. In live mode, the steps subsection of the
draft repro report, and the issue thread for any steps the original
author already provided.

What good looks like: Each step names one action and the state it
starts from; a stranger who has only the environment record and these
steps, and nothing else, could execute them in order and land on the
same trigger point. A step that assumes a config change, a data file,
a prior click, or any other action not itself listed is not followable
— it is a summary of steps, not the steps.

## Behavior shown

Where it lives: In an eval bundle, the report's output excerpt, log, or
screenshot description, read against the issue's stated symptom (its
error text, wrong output, or crash description) and against the
report's own stated outcome. In live mode, the artifact the student
pastes or links in the draft, read the same way against the issue
body's exact wording of the bug.

What good looks like depends on what the report is claiming — check
the stated outcome before judging the artifact:

- **Claims it reproduced the issue** (fully or partially): the excerpt
  must contain the same error text, the same wrong value, or the same
  crash signature the issue names — not a different error hit along
  the way, and not the feature quietly behaving correctly while the
  write-up narrates it as a match. This is where a wrong-target package
  gets caught: a confident conclusion sitting on top of an artifact
  that shows something adjacent, not the thing itself.
- **Claims it could NOT reproduce the issue**: the feature behaving
  correctly in the artifact is not a failure here — it is the expected
  shape of an honest cannot-reproduce. What matters is whether a real
  attempt is actually shown (a specific command run, a specific
  output, not just an assertion that it didn't happen) so a stranger
  can see what was tried, same as any other evidence. A cannot-reproduce
  claim backed by nothing at all — no attempt shown, just "works fine
  for me" — is the no-evidence failure mode, not a pass.

Whether the *conclusion itself* is honest given what the artifact shows
(claiming more than the evidence supports, in either direction) is the
Honesty check below; this check only asks whether an artifact matching
the claimed outcome is actually present.

## Honesty

Where it lives: In both modes, the report's concluding line(s)
("reproduced", "could not reproduce", "reproduced partially: X but not
Y"), read directly against the artifacts included immediately above
them in the same report.

What good looks like: The conclusion never claims more than the
artifact shown actually supports. "Could not reproduce after 5
attempts on the environment recorded above, full attempt log follows"
backed by a real attempt log is a pass. "Confirmed — this is a race
condition in the mutex" with no artifact showing concurrent access or
timing is a fail, no matter how confidently it reads.

## Comms

The rubric's bug-report/PR *template* fields (OS, version, current vs.
expected behavior, logs) are not a separate check here on purpose:
they are the same substance the Environment, Steps, and Behavior-shown
checks above already test for. A repo's template is written for
someone *filing* a new issue, not for a stranger replying with a
reproduction; nothing in the gold-labeled set turns on whether a repro
comment literally echoes a template's field names; a report can give
equivalent information in its own words (prose environment line
instead of pasted `tool info` output, for instance) and still be
exactly as followable. Grading template-literalism as its own required
check risks failing a substantively complete package for a cosmetic
reason — judge the artifacts and the report's content, not whether it
happens to mirror the repo's filing form.

**Discloses AI when required** — Where it lives: In an eval bundle,
the repo-facts block's stated AI-related policy, read against the
claim/repro comment text for an explicit disclosure line. In live
mode, the repo's CONTRIBUTING.md or contribution policy, read the same
way.

Not every AI-related policy is a disclosure mandate, and this check
only fires on the ones that are. Sort the policy before grading:

- **No policy stated at all** → pass vacuously.
- **Permissive-with-responsibility, no disclosure asked** (e.g.
  "AI-assisted contributions are welcome, contributors are responsible
  for what they submit") → pass vacuously. The repo is fine with AI
  help and never asked to be told about it.
- **Authorship rule, not a disclosure rule** (e.g. "comments to
  maintainers must be written by humans in their own words,
  AI-generated comments may be hidden") → this asks that the comment
  itself be genuinely human-voiced, not that you declare anything.
  Pass if the comment reads as substantive, specific, first-person
  writing engaging with this issue's actual details — do not fail this
  check just because some AI-related policy exists in the repo-facts
  block.
- **Explicit disclosure mandate** (e.g. "you must disclose any AI
  assistance used") → now the check is live. Pass if the comment
  contains a plain disclosure statement; fail if it contains none.
  Silence here is a fail, not a neutral or borderline result — this is
  the one check that catches a disclosure-wall package outright, so
  treat "the repo didn't ask" and "the repo asked and got nothing" as
  sharply different outcomes, not points on the same scale.

Separately from the disclosure check, also read the comment against
the issue's own title and description: specific, evidenced language
(names the issue's actual symptom, quotes a real line) reads
differently from boilerplate generic enough to post on any issue
unchanged — that distinction feeds the rubric's "Claim is specific,
not boilerplate" check, not the disclosure check above.
