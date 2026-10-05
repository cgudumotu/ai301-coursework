# Procedure: how this skill grades a plan package

These steps grade a plan someone else wrote. They do not write or
improve the plan. Follow them in order, and keep a scratch list of
notes as you go: later steps refer back to notes made in earlier ones.

## Read order

1. Read `rubric.md` and `references/evidence-guide.md`. Write down the
   name of every check, its weight, and the verdict rule.
2. In live mode only: read `scope.md`. If the issue is not in the
   scoped repo, or the `Repo:` line is still a placeholder, stop and
   say so without grading. Note the house rules.
3. Read the **issue** (title, body, labels, who opened it). Note in
   one line: the reported behavior, the expected behavior, and the
   trigger.
4. Read the **repro evidence** before the plan, so the plan cannot
   frame what the evidence means. For each step and each control run,
   note what was changed and what result it gave. Then write a
   **"ruled out" list**: every component a control run shows working,
   and every component the failure happens without (for example, "the
   tokenizer: same items parse fine without -v"). If there are no
   control runs, write "no controls".
5. Read the **thread highlights**. List every maintainer comment (see
   the rubric's definition) that names a culprit, proposes or rejects
   an approach, posts a build, asks for testing, or points at a PR or
   prior art. If there are none, write "no maintainer direction".
6. Read the **repo facts**. Copy the contribution policy and AI-policy
   wording exactly, and note whether its disclosure ask applies to
   comments, only to pull requests, or not at all.
7. Only now read the **candidate plan**, then the **candidate plan
   comment**.

Order matters because the wrong-cause check depends on judging the
plan against evidence read first, and the comms checks depend on
knowing the thread and policy before reading the comment.

## Evidence gathering

For each check, pull exactly this and record a short quote:

1. Diagnosis fits the repro evidence: the plan's sentence that names
   the cause, plus the "ruled out" list from Read order step 4.
2. Fix addresses the diagnosed cause: the plan's change or approach
   lines, plus the issue's expected behavior from step 3.
3. One bounded change: every item the plan says it will change (the
   in-scope list, numbered changes, files list), and every explicit
   "not in scope" or deferral line. Number the in-scope items.
4. A stranger could start: the files/areas named and the sentence
   that says which mechanism the change uses. Note any "or", "maybe",
   "somewhere", or "not sure" left on a core decision.
5. Test plan is decisive: each line of the test plan, marked
   "observable before/after" or "generic".
6. Comment engages maintainer direction: the list from Read order step
   5, and the comment's sentences that mention any item on it.
7. Comment meets the repo's AI and comment policy: the policy wording
   from step 6, and any sentence in the comment about AI use or own
   words.
8. Preferred checks: the plan's risk/unknowns lines; the comment's
   promises and dates.

In live mode, the plan side comes from `plan.md` and the comment
draft; the issue, thread, and repo facts come from GitHub (issue page,
`docs/CONTRIBUTING.md` or `CONTRIBUTING.md`, any AI policy file); the
repro evidence comes from the student's posted repro comment as quoted
in the drafts. In eval mode, use only the bundle text.

## Check execution

1. Run the required checks in rubric order, then the preferred ones.
2. For each check, compare the gathered quote to the pass condition
   and grade `pass` or `fail`. Write one line of evidence: the quote
   or fact that decided it.
3. Diagnosis check: for each item on the "ruled out" list, ask "does
   the plan blame this, or put its fix there?" Any yes is a fail. A
   confident tone, a long explanation, or agreement with the thread
   does not change this.
4. Scope check: for each numbered in-scope item, ask "would the
   reported bug still be fixed and tested without this?" If yes, that
   item is extra scope and the check fails. Deferred items do not
   count against the plan.
5. Grade `unclear` only when the evidence the check needs is absent
   from the package (for example, no test plan section and no test
   line anywhere). If you looked and found it, grade it.
6. Once a check is graded from its gathered quote, do not re-read the
   whole package for it. Re-read only if two of your notes contradict
   each other.

## Verdict assembly

1. List the required checks with their grades. Turn every `unclear`
   into `fail` for the verdict.
2. If all required checks pass, the verdict is `accept`. Otherwise it
   is `reject`.
3. Preferred checks are reported but never change the verdict.
4. In the summary before the JSON, name the deciding check(s) for a
   reject and quote the evidence line that failed them. For an accept,
   say which required check was closest to failing.
5. In live mode only, then check the comment against `voice-guide.md`
   and list any broken rule in the summary. This never changes the
   verdict.
6. Output the JSON block exactly as SKILL.md specifies, with every
   check in rubric order, and nothing after it.
