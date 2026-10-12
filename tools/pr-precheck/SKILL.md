---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer one question about one PR package: **is this ready to submit?**
A PR package is a candidate pull request (its title, description,
commits, diff, and test evidence) read against the plan it claims to
implement and the issue that plan belongs to. Never answer a different
question, never grade more than one package per run, and never answer
from gut feel: answer by executing `procedure.md` with the rubric in
`rubric.md` and the evidence map in `references/evidence-guide.md`.

## Inputs and modes

Run in exactly one of two modes.

- **Live mode**: my own submission, checked before it goes out. The
  inputs are my `plan.md` (its deviation notes included), the diff on
  my branch, my draft PR title and description, my test evidence (the
  repro re-run before and after, and the repo's own checks), and my
  issue. The branch's diff is everything the branch changes relative
  to the repo's default branch: run `git diff main...HEAD` (three
  dots) from the working copy, and `git log main..HEAD --oneline` for
  the commits. Gather issue-side evidence from the real repo: the
  issue thread, `.github/PULL_REQUEST_TEMPLATE.md`, and
  `docs/CONTRIBUTING.md` (including any AI-use wording). A house-chain
  student reads the house plan and the house repro pack in place of
  `plan.md` and a posted repro; the same checks grade the same things.
  Read the package the way a maintainer reads the PR: it is what the
  drafts and the diff contain, not other files in the working copy.
- **Eval mode**: a package bundle is the whole world. Every fact comes
  from the bundle text. Fetch nothing, run no commands, read no other
  file. Always grade a complete package: every check and the full
  verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. It names the repo
a pull request may target and the house rules there; apply those rules
when reading evidence. Refuse to grade a PR that targets any other
repo. If the `Repo:` line still holds a placeholder (it contains
`<ORG>` or `<PATH-REVIEW-REPO>`), stop without grading and tell me to
fill the `Repo:` line in `scope.md` with my section's Path Review
repo. Never guess a scope. In eval mode, ignore `scope.md` entirely.

## The voice seam (live mode only)

In live mode, after the verdict is assembled, read `voice-guide.md`
and hold the outgoing PR text, the **title and the description**,
against its rules. Report each rule the draft breaks in the summary,
quoting the rule. The voice guide never changes the verdict on its
own: only a rubric check that reads it can. In eval mode, ignore
`voice-guide.md` entirely: voice is personal and carries no gold
labels.

## Component reads

Three files carry the work, and I execute them as written.

- `rubric.md` defines the checks (each `required` or `preferred`) and
  the verdict rule.
- `references/evidence-guide.md` maps where each evidence family lives
  in a package, in eval mode and in live mode.
- `procedure.md` is the operating procedure: read order, evidence
  gathering, check execution, verdict assembly. Follow it step by
  step, without improvising.

Where the procedure is silent on a step, say so in the summary: report
the gap and do not invent a step. If `rubric.md` has no checks, or
`procedure.md` has no steps, refuse to grade and say so: this tool
does not grade without a rubric and a procedure.

## Verdict and output

The verdict space is binary: `accept` means ready to submit, `reject`
means hold. There is no third verdict and no score; reservations go in
a check's evidence line. Before the JSON you may show a short readable
summary: one line per check, the deciding check named and quoted, and
(live mode) any voice-guide notes. End the reply with the fenced JSON
block below, valid and last, with nothing after it. The harness reads
the last fenced JSON block.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- **Evidence first.** Never grade a check without naming the fact or
  quote that decided it. "Looks fine" is not evidence.
- **Grade the thing, not the polish.** A terse complete PR can be
  ready and a beautiful confident one can hide drift. Read the diff,
  the evidence, and the plan themselves, never the formatting or the
  confidence of the description.
- **The rubric decides, not me.** If a check passes by its stated
  condition but feels wrong, it still passes. Note the tension in the
  summary; the fix belongs in the rubric, not in the run.
- **The procedure decides how, not me.** Follow `procedure.md` as
  written and report its gaps.
- **Unclear is a fail** unless the rubric's verdict rule says
  otherwise: a PR I cannot verify from the package is not ready to
  submit.
