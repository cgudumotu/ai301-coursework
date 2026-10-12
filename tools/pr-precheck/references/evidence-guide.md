# Evidence guide: where evidence lives in a PR package

An eval package is one markdown file with these sections, in order:
`## Repo facts`, `## Issue`, `## Thread highlights`, `## Plan context`,
`## Candidate PR` (with `### Title`, `### Description`, `### Commits`,
`### Diff`, `### Test evidence`). In live mode the same families live
in my working copy and on GitHub, as listed under each heading.

## Plan fidelity (harness category: silent-drift)

- Where it lives (eval): the plan's scope, "Files", approach steps,
  "not in scope" line, and any deviation note are in `## Plan context`.
  What the change actually did is in `### Diff` (read the `---`/`+++`
  file headers, then each `@@` hunk). The description's claims are in
  `### Description`.
- Where it lives (live): `plan.md` (scope, files, approach, and
  `## Deviations`) in the fork's top folder; the diff is
  `git diff main...HEAD` (three dots) from the working copy; the
  description is my draft PR text.
- What good looks like: every file header in the diff is a file the
  plan names or a test or required file for it, and every hunk in it
  is a step of the plan's approach. A deviation is honest when a note
  in the plan says what changed and why. Silent drift is a file, flag,
  option, rename, or rewrite the plan never mentions (more than the
  plan), a plan deliverable missing from the diff with no note (less
  than the plan), or a description line (`"docs now document X"`,
  `"exactly as planned"`) the diff contradicts.

## Test evidence (harness category: not-tested)

- Where it lives (eval): `### Test evidence`, read against the test
  plan and the repro evidence in `## Plan context`, and against the
  stated checks in the `pull requests` line of `## Repo facts`. Tests
  the PR adds are in `### Diff` (`test`/`spec` file headers).
- Where it lives (live): my captured before/after output (the repro
  re-run on the branch, saved from the unit-3 step), the output of the
  repo's own checks (`make lint`, `make typecheck`, `make test-unit`
  or their `python -m` equivalents), and the repo's CI results on the
  PR.
- What good looks like: the plan's own repro command is shown with its
  before and after output, the expected-after result is visible in the
  output itself, and a named repo test or check command is shown with
  its outcome. Not decisive: "tested locally", "tests pass" with no
  command named, a control or unchanged path run in place of the bug's
  path, or a test that checks only a neighboring case.

## Diff quality (harness category: unreviewable)

- Where it lives (eval): `### Diff` and `### Commits`.
- Where it lives (live): `git diff main...HEAD` and
  `git log main..HEAD --oneline`, plus `git status` for stray files.
- What good looks like: each added or removed line is the fix or its
  test, so a reviewer can read the diff once and see the change. The
  debris tells: a debug `print`/`eprintln`/`console.log` left in,
  commented-out code (an old attempt or a disabled line), a function
  nothing calls (often `allow(dead_code)` or an underscore name), a
  `TODO`/`WIP`/`XXX` marker, identical lines re-printed as `-` and
  `+`, import re-ordering or re-indentation hunks, and edits to files
  unrelated to the issue. Commit subjects like "wip" or "misc cleanups
  while debugging" are a hint to look harder, not a verdict.

## Standards and comms (harness category: standards-wall)

- Where it lives (eval): the `pull requests` and `contribution policy`
  lines of `## Repo facts` (template sections, checklist, required
  files, conventions, AI policy), read against `### Title`,
  `### Description`, and the file list in `### Diff`. Maintainer
  direction is in `## Thread highlights`.
- Where it lives (live): `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` in the repo (including its AI-use wording and
  commit-message convention), the issue thread, and my draft PR text.
- What good looks like: each template section has real content for
  this change (not blank, not the template's own comment text), the
  issue-closing line names the right issue number, each file the repo
  says it requires is in the diff (changelog or release-notes entry,
  docs), and, where the policy requires disclosure of AI use, the
  description says which tool and how far it helped. A policy that
  asks only for human understanding or own words is met by a
  description that reads like a person wrote it. Whether the
  description's claims match the diff is plan fidelity, above.
