# Evidence guide: where evidence lives in a plan package

An eval package is one markdown file with these sections, in order:
`## Repo facts`, `## Issue`, `## Thread highlights`, `## Repro
evidence`, `## Candidate plan`, `## Candidate plan comment`. In live
mode the same families live on GitHub and in the student's drafts, as
listed under each heading.

## Diagnosis and grounding

- Where it lives (eval): the cause is in the candidate plan's first
  lines, usually under "Diagnosis" or "Cause". The behavior it must
  explain is in `## Repro evidence`: the numbered steps, the pasted
  output, the **"Control" lines**, and the closing "Expected / Actual"
  lines.
- Where it lives (live): the cause is in `plan.md`'s diagnosis
  section; the evidence is the student's posted repro comment on the
  issue (quoted in the plan).
- What good looks like: the stated cause explains every step and
  control in the repro. If a control run shows a component working, or
  shows the failure happening before that component runs, a cause
  that blames that component is wrong, however confident or detailed
  the plan sounds. A cause copied from the issue or the thread is fine
  when nothing in the repro contradicts it.

## Scope

- Where it lives (eval): the candidate plan's "Scope", "Change",
  "Proposed changes", "In scope / Not in scope", and "Files" lines.
  The comment often restates scope ("plan is a one-change fix", "happy
  to split into a PR series").
- Where it lives (live): `plan.md`'s scope and files sections.
- What good looks like: one change aimed at the reported bug, plus the
  tests for it, with a not-in-scope line naming what is left alone. A
  drive-by rewrite lists several unrelated fronts: "while touching
  this", "also", migrations, new settings, redesigns, CI changes. A
  stated deferral of a bigger rework is a good sign, not creep.

## Executability

- Where it lives (eval): the candidate plan's "Files", "Approach", and
  numbered steps.
- Where it lives (live): `plan.md`'s files and approach sections.
- What good looks like: a named file or code path and one chosen
  mechanism ("add the commits context to the post-push refresh scope
  in `sync_controller.go`"). A stranger could open the editor and
  start. Vague looks like a question list, "investigate", "somewhere",
  or two options with "whichever is easier".

## Test plan

- Where it lives (eval): the candidate plan's "Test" or "Test plan"
  lines, compared with the repro's steps and its Expected line.
- Where it lives (live): `plan.md`'s test plan section, compared with
  the repro comment's commands.
- What good looks like: the repro command re-run with the exact
  output, exit code, or visible change expected after the fix (for
  example "step 3 shows the pushed color without leaving the view"),
  or a named regression test encoding the issue's case. Not decisive:
  "run the full test suite", "nothing regresses", "should feel fast".

## Honesty

- Where it lives (eval): the candidate plan's "Risk", "Unknowns", or
  "Open question" lines, and hedges inside the approach ("verified
  gzip, xz, zstd locally; will check the rest").
- Where it lives (live): `plan.md`'s risks and unknowns section, and
  its `## Deviations` section after the build.
- What good looks like: the plan separates what was verified from what
  was assumed, and names at least one thing that could go wrong. False
  confidence is a cause or approach stated as certain with nothing in
  the evidence behind it. An honest mid-build change is recorded under
  `## Deviations` in the plan, not only in the diff.

## Comms

- Where it lives (eval): `## Thread highlights` (look for OWNER,
  MEMBER, COLLABORATOR, CONTRIBUTOR comments that name a culprit,
  propose or reject an approach, post a build, or link a PR), the
  `contribution policy` bullet in `## Repo facts`, and the text of
  `## Candidate plan comment`.
- Where it lives (live): the issue thread on GitHub, the repo's
  `CONTRIBUTING.md` (Path Review: `docs/CONTRIBUTING.md`) and any AI
  policy file, the PR template, and the student's draft `comment.md`.
- What good looks like: the comment names the maintainer direction and
  follows it or explains the departure ("my plan follows the direction
  proposed here", "I read PR #2089, which takes the same route"). On
  AI policy: if the repo requires disclosing all AI use, the comment
  states the tool and the extent. If it requires disclosure only in
  PRs, the comment is not bound. If it requires comments in the
  contributor's own words, the comment reads like a person and may
  say so. Boilerplate that would fit any issue and ignores the thread
  is the failure.
