# Procedure: how this tool grades a PR package

These steps grade a pull request someone else wrote. They do not write
or improve it. Follow them in order and keep a scratch list; later
steps refer to lists made in earlier ones.

## Read order

1. Read `rubric.md` and `references/evidence-guide.md`. Write down each
   check's name and weight, and the verdict rule.
2. Live mode only: read `scope.md`. If the PR's repo is not the scoped
   repo, or the `Repo:` line is still a placeholder, stop without
   grading and say so. Note the house rules.
3. Read the **issue** and **thread highlights**. Write one line: what
   the bug is, and any explicit maintainer direction.
4. Read the **repo facts**. Copy exactly: the PR-template sections and
   checklist, any required files (changelog, whatsnew, docs), stated
   check commands, the title/commit convention, and the AI-policy
   wording. Note whether the AI policy requires disclosure in PRs.
5. Read the **plan context before the PR**, so the plan cannot be
   bent to fit the diff. Write three lists: **(A) planned files and
   deliverables** (every file and every approach step the plan names),
   **(B) not in scope** (everything the plan scopes out), **(C)
   deviation notes** (each recorded change and its reason, or "none").
   Also copy the plan's test plan and the repro steps it relies on.
6. Read the **candidate PR**: title, description, commits, diff, test
   evidence, in that order. While reading the description, list every
   factual claim it makes about the change (`"adds"`, `"documents"`,
   `"exactly as planned"`, `"nothing else"`).

Order matters because the side-by-side checks compare the diff with
list A, B and C, and compare the description's claims with the diff.
Reading the plan first keeps a polished description from deciding what
the plan said.

## Evidence gathering

For each check, pull exactly this and record a short quote:

1. **Diff stays inside the plan**: go through the diff one file header
   at a time. For each file write "in plan (A)", "required by the repo
   (changelog/test)", "covered by deviation (C)", or "unplanned". Then
   go through each hunk of each file and write the same for the hunk.
   Any item in list B is "unplanned".
2. **Plan delivered and description matches the diff**: for each item
   in list A, find the lines of the diff that deliver it, or the note
   in (C) or the description that defers it. For each claim in the
   description list, find the diff lines that make it true, or mark it
   "unsupported".
3. **Evidence exercises the plan's repro**: copy the command(s) in the
   test evidence. Compare each with the plan's repro steps: same
   command, same path where the bug lives? Record the before output,
   the after output, and the expected-after result. Then list the
   tests the diff adds and what trigger each exercises.
4. **Repo's own checks run**: list each check or test command the
   repo-facts block names, then the commands in the test evidence and
   their outcomes.
5. **Diff is reviewable**: walk every added and removed line once. Mark
   each as fix, test, or debris, using the tells in the evidence
   guide. Note the commit subjects.
6. **Template and stated file asks met**: for each template section,
   checklist item, and required file from Read order step 4, find the
   description text or the diff file header that satisfies it.
7. **AI-use disclosure meets policy**: the policy wording from step 4,
   and any sentence of the description about AI use or own words.
8. Preferred checks: the description's limit statements against the
   plan's stated limits; the title and commit subjects against the
   stated convention.

In live mode, the plan is `plan.md`; the diff is `git diff main...HEAD`;
the description and title are my draft; the test evidence is my saved
output; the repo facts come from `.github/PULL_REQUEST_TEMPLATE.md`,
`docs/CONTRIBUTING.md`, and the issue thread. In eval mode, use only
the bundle text.

## Check execution

1. Run the required checks in rubric order, then the preferred ones.
2. Grade each `pass` or `fail` by comparing its gathered quote with the
   pass condition. Write one line of evidence: the fact or quote that
   decided it.
3. Plan-fidelity checks: a single unplanned file or hunk (and not
   covered by a deviation note) fails "Diff stays inside the plan". A
   single unsupported description claim fails "Plan delivered and
   description matches the diff". A deferral that the plan or
   description states plainly does not fail either check.
4. Evidence check: if the output shown is not the plan's repro, or has
   no before-and-after, or only a control, grade `fail`. A before cited
   from the plan context counts when the after is shown.
5. Reviewable check: one debris tell anywhere in the diff fails it, no
   matter how small or mechanical.
6. Grade `unclear` only when the evidence the check names is truly
   absent from the package. If you looked and found it, grade it.
7. Once a check is graded from its quote, do not re-read the whole
   package for it. Re-read only if two notes contradict each other.

## Verdict assembly

1. List the required checks with their grades. Treat each `unclear` as
   `fail`.
2. If all required checks pass, the verdict is `accept`. Otherwise it
   is `reject`.
3. Preferred checks are reported and never change the verdict.
4. In the summary before the JSON, name the **first failing required
   check in rubric order** and quote its evidence line as the deciding
   check. For an accept, say which required check came closest to
   failing.
5. Live mode only: hold the title and description against
   `voice-guide.md` and list any broken rule in the summary, quoting
   the rule. This never changes the verdict.
6. Output the JSON exactly as SKILL.md specifies, every check in rubric
   order, and nothing after it.
