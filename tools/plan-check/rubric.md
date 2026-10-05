# Rubric: is this plan ready to post and build from?

Terms used below. "Repro evidence" means the reproduction the plan
builds on: its steps, its verbatim artifacts, and especially its
**control runs** (a second run that changes one input and shows a
different result). "Maintainer" means a thread participant marked
OWNER, MEMBER, COLLABORATOR, or CONTRIBUTOR, or the repo's named
maintainer. "The comment" is the candidate plan comment; "the plan"
is the candidate plan.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Diagnosis fits the repro evidence | The plan's stated cause, read against every control run and step in the repro evidence block (in live mode, the student's posted repro comment), using the "ruled out" list made during read order. | The cause the plan names is **consistent with every control run** in the repro evidence, and the plan's fix site is where the evidence points. Fails when a control run or step in the evidence already shows the blamed component working, or shows the failure happening **before or without** it (`"the same items parse fine without -v"`, `"the zeros are already gone before the cast runs"`, `"26s with no pager in the loop"`). Fails the same way when the plan adopts a thread member's or the issue's confident diagnosis that the package's own evidence contradicts: the evidence outranks the thread. A plan whose cause simply restates the issue's cause passes when no control run contradicts it. A plan that names **no cause at all** (only "investigate", "profile", "find out why") fails: there is nothing for the fix to follow from. | required |
| Fix addresses the diagnosed cause | The plan's change/approach section, read against its own diagnosis and the issue's expected behavior. | The planned change alters the behavior the diagnosis names, so after it lands the issue's expected behavior occurs without user workarounds. Fails when the plan documents a workaround, suppresses or hides the symptom, or changes only docs or messages while the bug stays in place, **unless** the issue or a maintainer in the thread asks for exactly that. | required |
| One bounded change | The plan's in-scope / not-in-scope statements, its numbered changes, and its files list, each item read against what the issue asks for. | Every in-scope item is needed to fix the reported bug or to test that fix. Explicit deferrals ("not in scope", "left to a follow-up", "a larger rework the maintainers may prefer") are honest scope and pass. Fails when the plan bundles work the issue did not ask for alongside the fix (dependency or library migrations, refactors "while in the area", new options or settings, UI redesigns, new frameworks or retry layers, CI-matrix changes, module restructures, rewriting a subsystem to fix one defect), **even if the core fix in it is correct**. | required |
| A stranger could start | The plan's files/areas and approach sections, read as a contributor who has never seen the thread. | The plan names where the change goes (files, or a specific function/module/code path) **and** commits to one chosen mechanism. Pinning exact function names during the build is fine when the area and the mechanism are already chosen. Fails when the plan is an investigation instead of a change (`"profile and optimize"`, `"poke around"`), leaves the core decision open (`"gocui? tcell? not sure"`, `"upstream or vendored, whichever is easier"`, `"recover() somewhere"`), or names no location at all. | required |
| Test plan is decisive | The plan's test plan, read against the repro evidence's steps and its expected-vs-actual lines. | The test plan names at least one **observable outcome that differs before and after the fix**, tied to the bug: a repro command re-run with its expected output, exit code, or visible behavior, or a named regression test that encodes the issue's case. Fails when the only test is generic (`"run the full test suite"`, `"nothing regresses"`), subjective (`"should feel fast"`, `"nothing else should feel broken"`), or absent. A suite run **in addition to** a decisive check passes. | required |
| Comment engages maintainer direction | The thread highlights (live: the issue thread), read for any explicit maintainer direction, then the comment read against it. | If a maintainer in the thread has given explicit direction (named the culprit or fix site, proposed or rejected an approach, posted a patched or test build, asked for testing, or pointed at an open PR or prior art), the comment acknowledges that direction and either follows it or says why it departs. Passes automatically when the thread holds no maintainer direction. Fails when the comment proposes a different route as if that direction did not exist. | required |
| Comment meets the repo's AI and comment policy | The repo-facts block's contribution policy and AI-policy line (live: CONTRIBUTING and any AI policy file), read against the comment's text. Treat every package as AI-assisted work. | Whatever the repo's stated policy requires **of comments** is satisfied. A policy that requires disclosing **all** AI usage in any form, or AI use in issues and comments, requires the comment to state the tool and the extent of the help. A policy that requires disclosure only in pull requests does not bind a comment. A policy requiring comments in the contributor's own words is satisfied by a comment that reads that way. Silence passes: no stated policy is no requirement. | required |
| Risks and unknowns stated | The plan's risk/unknowns lines and any hedges in the approach. | At least one real risk, open question, or verified-vs-unverified distinction is named, rather than everything stated as certain. | preferred |
| Comment matches the plan and stays modest | The comment, read against the plan. | The comment summarizes the same diagnosis and change as the plan and promises no merge, date, or outcome the author cannot deliver alone. | preferred |

## Verdict rule

**Accept** (ready to post and build from) if and only if every
`required` check grades `pass`. One required `fail` holds the package
(**reject**). Grade and report every required check even after one
fails; do not stop early.

`unclear` on a required check counts as `fail`: a plan that cannot be
verified from the package is not ready to build from. Use `unclear`
only when the evidence the check names is truly missing from the
package, never as a softer `fail`.

`preferred` checks never change the verdict. On an accepted package
they say how strong the plan is, not whether it ships.
