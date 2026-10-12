# Rubric: is this pull request ready to submit?

Terms used below. "The plan" is the plan context block: the plan's
scope, files, approach, test plan, and any deviation notes (live: my
`plan.md`, `## Deviations` included). "The diff" is every changed line
in the candidate PR (live: `git diff main...HEAD`). "The description"
is the PR description text. "Test evidence" is the PR's test-evidence
section: pasted commands and output. A **deviation note** is a line in
the plan context that says what changed from the plan and why. "The
repo's asks" are the repo-facts block's PR-template sections, stated
checks, required files, and AI policy.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diff stays inside the plan | Every changed file and every hunk in the diff, read against the plan's scope, files list, approach steps, and its "not in scope" line, then against the deviation notes. Note each file and hunk in turn. | Every changed file and hunk either implements something the plan names, or tests it, or is a change the repo's asks require for it (a changelog entry, a docs line the template asks for), or is covered by a deviation note. **Fails** when the diff carries a behavior change, new option/flag/setting, rewrite or rename pass, or an edit to a file the plan does not name or scopes out, with no deviation note covering it, **even if the extra work is adjacent and the core fix is right**. | required |
| Plan delivered and description matches the diff | Each deliverable in the plan's approach (code change, tests, docs), then each factual claim in the description (`"adds X"`, `"docs now document Y"`, `"implements the plan exactly"`, `"no changes outside Z"`), each checked against the diff. | Every plan deliverable appears in the diff **or** is deferred by a deviation note or a limitation the description states; and every claim in the description is true of the diff. **Fails** when the description claims something the diff does not contain (less than the plan, claimed as done), or claims fidelity (`"exactly as planned"`, `"nothing beyond the plan"`) over a diff that adds more, or the diff silently omits a plan deliverable. **A disclosed shortfall passes:** a limitation or deferral that the plan's note or the description states plainly is honest, however large. | required |
| Evidence exercises the plan's repro | The test evidence read against the plan's test plan and its repro steps (the plan context's repro evidence), then any test the diff adds. | The evidence shows **the plan's own repro** (the command or steps on the path where the bug lives, not a control or an unchanged path) with a visible **before and after** and the expected-after outcome observable in the output. A before taken from the plan context's repro evidence and cited as such is fine when the after is shown. Any test the diff adds must exercise the bug's trigger, not only an adjacent case. **Fails** when the evidence is only a claim (`"tested locally"`, `"works on my machine"`, `"tests pass"`), when it runs only the control or the unchanged path, when the plan's repro is never re-run, or when a failure mode the plan's test plan names goes unrun without disclosure. | required |
| Repo's own checks run | The test evidence read against the repo-facts block's stated checks and the template's testing asks (test command, linter, formatter). | The evidence names at least one of the repo's own check or test commands that covers the changed code **and** shows its outcome (output, pass counts, or a stated result tied to the named command), **or** the description plainly says a check was not run and why. **Fails** when no repo check or test run is mentioned at all and nothing is disclosed. | required |
| Diff is reviewable | Every added and removed line of the diff, and the commit list. Look for the debris tells: debug prints or logging left in, commented-out code, dead or unused functions, TODO or WIP markers, re-printed identical lines, import or whitespace re-ordering, and hunks unrelated to the fix. | Every added or removed line is part of the fix or its tests. Comments that explain the fix are not debris. A `-`/`+` pair that re-prints one unchanged line only because it sits beside a line the fix changes is diff rendering, not churn; a whole block re-printed with no changed line in it is churn. **Fails** when any debris tell above sits in the diff, **even mechanical churn around a correct fix**: the reviewer must dig the fix out of it. | required |
| Template and stated file asks met | The repo-facts block's PR-template sections, checklist items, and stated required files (changelog or release-notes entry, docs, issue-closing line), read against the description and the diff's file list. | Each template section the repo states has real content for this change (not blank, not placeholder text), the issue reference the template asks for is present, and each file the repo states it requires is in the diff or covered by a deviation note. Passes when the repo states no template and no required files. **Fails** when a stated section, checklist, or issue reference is visibly ignored, or a stated required file is absent from the diff without a note. | required |
| AI-use disclosure meets policy | The repo-facts block's contribution policy and AI-policy line, read against the description. Treat every package as AI-assisted work. | Whatever the repo's stated policy requires **of pull requests** is met. A policy that requires disclosing AI usage (in any form, or in PRs) requires the description to state the tool and the extent of the help. A policy that asks only for human understanding or for text in the contributor's own words is met by a description that reads that way. **Silence passes:** no stated policy is no requirement. **Fails** when the policy requires disclosure and the description has none. | required |
| Description states its limits | The description, read against the plan's known limits and unknowns and against the diff. | The description names any limit, deferral, or untested edge the plan or diff shows, in the PR's own text. | preferred |
| Title and commits follow the repo's form | The PR title and commit subjects read against the repo-facts block's stated convention (conventional commits, scope list, closing line). | The title and commit subjects match the repo's stated form, or the repo states none. | preferred |

## Verdict rule

**Accept** (ready to submit) if and only if every `required` check
grades `pass`. One required `fail` holds the package (**reject**).
Grade and report every required check even after one fails; do not
stop early.

`unclear` on a required check counts as `fail`: a PR that cannot be
verified from the package is not ready to submit. Use `unclear` only
when the evidence the check names is genuinely missing from the
package, never as a softer `fail`.

`preferred` checks never change the verdict. On an accepted package
they say how strong the PR is, not whether it ships.

A PR that honestly discloses a shortfall, a limit, or a deviation can
be ready: those checks read the **diff against the plan and the
description**, never "less than everything" as a failure in itself.
