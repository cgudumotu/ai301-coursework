# Evidence guide: where proof lives in a reproduction package

The map for the checks in `rubric.md`. For each proof family: where to
look, and what good looks like when you get there. Eval bundles and live
GitHub hold the same five families in different places, so each section
names both.

A rule that runs through all five: **read the candidate side against the
issue side.** Nothing in a report is good or bad on its own; it is good
or bad against what the issue says happens, on what, and when.

## Environment

**Where it lives.** In an eval bundle: the opening lines of the repro
report (usually one `Environment:` line or a small table), read against
the issue context's own version/platform statement and against the
`bug reports: template asks for ...` line in the repo-facts block. In
live mode: the environment section of the student's draft report, read
against the issue's body on GitHub and the repo's issue template in
`.github/ISSUE_TEMPLATE/`.

**What good looks like.** The version of the software under test and the
platform are both named, specifically enough to install: `bat 0.26.1
(cargo), Fedora 44 (x86_64)`, not "latest" or "my machine". Anything the
trigger depends on is named too — the driver for a virtualization bug,
the shell for a prompt bug, the browser and its language order for a
localization bug — and the repo's template is the fastest list of what
that repo considers essential. Hardware boasts are not an environment
record: "RTX 4080, 64 GB RAM" names no version of anything. A
cannot-reproduce report needs this *more* than a successful one, because
the environment difference is the finding.

## Steps

**Where it lives.** In an eval bundle: the `Steps` section of the repro
report, plus any setup lines above it. In live mode: the same part of
the draft, read against the reproduction steps in the issue body.

**What good looks like.** A stranger with a clean machine can get from
nothing to the trigger without asking a question. Every file created is
shown with its contents, every command is pasted as run, and any config
that matters is quoted inline. The test: could I paste these commands
into a fresh shell? "Checked out our company monorepo (private, cannot
share)" fails it, and so does a step that quietly drops a condition the
issue names as required — the driver flag on a driver-specific bug, the
`--replace` flag the owner said is needed to trigger it. Short is fine.
Four lines that run beat twenty that cannot.

## Behavior shown

**Where it lives.** In an eval bundle: the fenced output blocks, log
excerpts, and described screenshots in the repro report, read against
the issue's own "actual behavior" text and any error, exit code, or
stack trace it quotes. In live mode: the artifacts pasted into the
draft, read against the issue body and any maintainer comment that
refines what the real trigger is.

**What good looks like.** The artifact is verbatim output, produced by
the steps just above it, and the failure in it is *the issue's* failure.
Compare three things: the **trigger** (same syntax, same operator, same
input shape — `:-18446744073709551614` is not `18446744073709551614:`,
and `1 = {}` is not `1: {}`), the **failure mode** (a panic with exit
101 is not a validation error with exit 1; a compile error is not a
wrong path result), and the **end state** (a process that crashed is not
a process still printing with the prompt returning). An artifact that
proves only that the tool is installed and running — a version banner, a
session list, tabs visible in a screenshot — shows the setup, not the
bug. The most common near-miss is an adjacent symptom narrated
confidently as the reported one; the artifact is what settles it, not
the prose around it.

**Cannot-reproduce.** Here the artifact's job changes: it must show the
attempt really ran and what happened *instead*. Marker files in the
order they came out, a prompt that rendered normally, a control that
behaved as expected. A cannot-reproduce with no artifact is the same
empty comment as a me-too.

## Honesty

**Where it lives.** In the seam between the two: each assertion in the
claim comment and the report's prose, set beside the artifact that is
supposed to back it. In an eval bundle, read the report's summary
sentences last, after the artifacts, and ask what evidence each one
rests on. Same in live mode.

**What good looks like.** Nothing is asserted that is not shown. A
report may say "the 🚧 input collapses to one line (shown above)"; it may
not say "I ran this ten times", "guaranteed reproducible", or "I
verified the root cause is a debounce race" with nothing pasted. Where
the run deviates from the issue's target — a newer release, a different
OS, a different shell — the deviation is **stated**: "filed against
13.0.0, unchanged on 15.2.0" is honest, silently testing pandas 1.5.3
against an issue confirmed on main is not. An evidenced "I could not
reproduce this, here is exactly what I ran and what differed" is a
first-class result and the most useful thing an unsuccessful attempt can
produce. Confidence is not evidence, and polish is not evidence: the
best-formatted report in the set can be the emptiest.

## Comms

**Where it lives.** The claim comment, read against the issue it belongs
to; both comments read against the repo-facts block's `contribution
policy` and any AI-policy line. In live mode: the draft comments against
`CONTRIBUTING.md`, `AI_POLICY.md`, and the PR/issue templates in the
repo, plus `scope.md` for house rules and `voice-guide.md` for the
student's own rules.

**What good looks like.** The claim could only have been written about
this issue: it names the version tested, the symptom, a file, or the
next thing the author will look at, and it promises investigation only.
"I'd like to look at this; reproduced on 0.64.1 (report below); next I
want to read where the stash prompt decides to appear" is specific and
modest. "Kindly assign it to me, I will fix it within 2 days
guaranteed" is boilerplate plus a promise the author cannot keep, and
"+1 any updates??" is neither a claim nor evidence.

**Policy, read precisely.** Three shapes, and the difference decides the
check. An **outright disclosure requirement** ("all AI usage in any form
must be disclosed, stating the tool and the extent") binds these
comments: a package whose comments say nothing about AI fails it, however
good the proof is. A requirement **scoped to pull requests** ("state the
tool and extent of its use in the pull request") does not bind a comment
posted on an issue. A requirement that **comments be human words** is
satisfied by comments that read as the contributor's own. And silence —
most repos — is not a requirement at all. Read the policy line for who
it binds and where, not for whether the word "AI" appears in it.
