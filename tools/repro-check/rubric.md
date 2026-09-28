# Rubric: is this reproduction package ready to post?

"The issue's target" below means the version, platform, and trigger the
issue itself names. "Artifact" means verbatim output pasted into the
report: a transcript, log excerpt, error text, or described screenshot.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Environment recorded | The repro report's environment record, read against the issue's stated target and against the repo-facts block's bug-report template asks. | The report names the version of the software under test **and** the platform it ran on (OS or browser), plus any additional component the issue's own trigger depends on and the repo's template asks for (driver, shell, language runtime). A cannot-reproduce report passes this check the same way. Absent version, absent platform, or a machine description that names no versions at all (`"my machine is high end, RTX 4080"`) fails. | required |
| Steps a stranger can re-run | The repro report's steps and setup, read as a stranger with a clean machine and no access to the author's files. | Every input needed to re-run is **available** to a stranger: the commands are pasted, and each file or config created is either shown, described specifically enough to recreate (`"a minimal env.yml with a valid dependencies: list plus a category: section"`, `"the exact 12 lines from the issue"`), or taken from the issue itself. The specific trigger condition the issue names is present. Fails only when an essential input is **unavailable** — private or unshared (`"our internal config, not shareable"`, a company monorepo), when the steps omit a condition the issue says is required to trigger it, or when there are no steps at all. A described file is not a missing file. | required |
| Artifact shows the issue's behavior | The report's artifacts read side by side with the behavior the issue describes: the same error text, exit status, or symptom, produced by the same trigger the issue names. | The report shows a verbatim artifact produced by its own stated steps, and what the artifact shows **is the behavior the issue describes** — same failure mode, same trigger syntax. Fails when the artifact shows a *different* failure than the issue reports (a graceful validation error where the issue reports a panic, a compile error where the issue reports a wrong result, output still scrolling with the process alive where the issue reports a crash), when the input differs from the issue's trigger (a different operator, a prefix range where the issue uses offset-from-end), or when the artifact only shows that the tool ran (a version banner, a session list) rather than the failure. **Cannot-reproduce carve-out:** when the report's stated outcome is that the bug did *not* reproduce, this check passes if the artifacts show the attempt actually ran and what happened instead; it is not required to show the bug. | required |
| Stated outcome matches the evidence | The report's **headline result** (reproduced / not reproduced, and what was observed) checked against the primary artifact that is supposed to back it; then any version or platform deviation from the issue's target checked against whether the report states it. | The headline result rests on a shown artifact, and the narration of that artifact says what it shows — no more. Each of these fails: narrating an artifact as the reported failure when it shows something else; a headline confirmation or root-cause claim (`"guaranteed reproducible"`, `"I verified the cause"`) **with no shown artifact standing behind it**; and running against a version, platform, or configuration different from the issue's target **without saying so** — stating the deviation passes, hiding it fails. **Not a failure:** a secondary observation stated in a sentence beside a shown primary artifact (a control run summarized, "ran it 5 times with the same result"), because the headline rests on the artifact, not on the sentence. An explicit cannot-reproduce that names what differed is the honest outcome and passes. | required |
| Repo comment conventions | The repo-facts block's contribution policy and AI-policy lines, read against the text of the claim comment and the repro report. | Whatever the repo's stated policy requires **of comments** is satisfied. If the policy requires disclosing AI assistance in any form, at least one comment states the tool and the extent of the assistance; a policy that requires disclosure only in pull requests does not bind a comment, and a policy that requires comments be the contributor's own human words is satisfied by comments that read that way. Silence passes: most repos state nothing, and that is not a requirement. | required |
| Claim comment is specific and modest | The claim comment alone, read against the issue. | The claim names something specific to *this* issue — a version tested, the symptom, a file or code path, or a concrete next step — **and** promises only investigation. Fails on a bare `+1`/me-too with no stated intent, on interchangeable boilerplate that would fit any issue, on demanding assignment, and on promising a fix or a delivery date (`"I will fix it within 2 days guaranteed"`). | required |
| Control or contrast run | The report's artifacts: a second run that isolates the trigger (the same command without the triggering input, or the neighbouring case that works). | A contrasting run is shown, so the trigger is isolated rather than asserted. | preferred |
| Concrete next step | The closing lines of the claim comment or report. | A specific next action is named (a file, a code path, a test, a configuration to try), not just willingness to help. | preferred |

## Verdict rule

**Accept** (ready to post) if and only if every `required` check grades
`pass`. A single required fail holds the package; the remaining required
checks are still graded and reported rather than short-circuited.

`unclear` counts as `fail` on a required check: proof that cannot be
verified from the package is proof that is not ready to post. The one
exception is the skill's claim-only draft mode, where checks whose
evidence is the repro report are reported `unclear` with evidence
`not yet applicable: claim-only draft` and are **left out** of this
verdict rule entirely; the verdict then answers only whether the claim
comment is ready.

`preferred` checks never change the verdict. Report their grades; on an
accepted package they say how strong the proof is, not whether it ships.
