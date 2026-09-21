# Rubric: is this a good first issue?

All dates are measured against the bundle's stated **capture date** in eval
mode (the `captured:` stamp / Repo facts block), and against **today** in
live mode. "Maintainer" means a commenter whose `author_association` is
OWNER, MEMBER, or COLLABORATOR.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| Maintainer alive | The "last 5 default-branch commits" list (dates + authors) and the "maintainer first-response sample" in the repo-facts block. | The most recent default-branch commit is dated **within 180 days** of the capture date. A commit authored by a bot counts only when its message shows it merged a human's pull request (e.g. "Merge pull request #N from ..."). If the commit list is absent, fall back to: at least one maintainer first-response in the sample is **under 60 days** and dated within 180 days. | required |
| Repo in use | The repo line (`archived:`, stars), "last push to any branch", and "latest release" in the repo-facts block. | `archived: no` **and** last push to any branch is **within 180 days** of the capture date. A missing or older release does not fail this check on its own; an archived repo fails it outright. | required |
| Scope fit | The issue title and body, the labels, the opener's `author_association`, and the full comment thread; plus the `linked PRs:` states in the repo-facts block. | The issue asks for **one bounded deliverable** — work a newcomer could land in a single pull request — and **none** of these disqualifiers is present: (a) it is explicitly a tracking, umbrella, or "mega" issue: it lists sub-issues to be taken separately, or invites many contributors to each take a different slice of an open-ended job ("add X across the codebase", "PRs welcome, big and small"); (b) the thread contains design or product disagreement that **no maintainer has settled** — the newest maintainer comment does not say what to build; (c) it is a **new-feature request with no maintainer endorsement**: no maintainer opened it, commented approvingly, or gave it a good-first-issue label (the reporter's own "success looks like" text or filled-in feature-request template is **not** an endorsement); (d) a maintainer **warns** the change requires reworking core internals, or says it is unsuitable for a newcomer; (e) it is a usage/support question rather than a change to the project; (f) **two or more** of its linked PRs are closed-unmerged and no maintainer has since posted a settled spec; (g) it asks for a product or design decision only maintainers can make (what belongs in the default UI, what a public API should be called), or a required asset or decision is still "TBD". **The following are NOT disqualifiers.** A long body or a detailed content outline: that is a *spec for one deliverable*, and detail makes an issue easier, not bigger. A terse one-line body, especially from a maintainer or under a good-first-issue label. A maintainer naming two or more likely causes of one bug, or adding optional follow-up suggestions to a bug they filed: diagnosis is help, and optional suggestions do not enlarge the required work. A bug report, docs task, or test fix that has no maintainer comment: only **new-feature** requests need endorsement under (c). Grade the size of the work asked for, not the length or polish of the writeup. | required |
| Unclaimed | `assignees:` and `linked PRs:` (with per-PR state) in the repo-facts block, plus every comment in the thread with its date and author_association. | All three hold: (1) `assignees: none`; (2) **no linked PR in `open` state**, and no PR described in the thread as open or in progress (closed or merged PRs do not block); (3) **no claim comment** ("I'll take this", "working on this", "can I work on it", `/assign`, `@bot claim`) dated **within 365 days** of the capture date, unless a maintainer replied after it saying the issue is still free. A maintainer comment inviting takers, or a bot nudge asking the claimant whether they are still active, does **not** clear a claim that is otherwise live (an assignee or an open PR). In live mode, apply any house rule in `scope.md` that changes whose claim comments count. | required |
| AI policy allows this workflow | The "contribution policy" line in the repo-facts block (`CONTRIBUTING.md`, linked contributor docs, `AI_POLICY.md`, `AGENTS.md`, PR-template disclosures). | The policy does **not** state an outright ban on AI-assisted contributions. Silence passes. Conditions pass: disclosure, "you must understand and test every change", human review, "fully AI-generated contributions are not accepted" where assistive AI use is explicitly allowed, and warnings that unreviewed AI output will be closed. Only a flat refusal of AI-written code or documentation with no assistive carve-out ("we do not accept AI-generated code or documentation") fails. | required |
| Beginner-signposted | The issue's labels, and the opener's `author_association`. | The issue carries a `good first issue` / `help wanted` / `easy` label, **or** was opened by a maintainer. | preferred |
| Maintainer answers fast | The "maintainer first-response sample" in the repo-facts block. | At least one sampled issue got a maintainer first response in **7 days or fewer**. | preferred |

## Verdict rule

**Accept** if and only if every `required` check grades `pass`. A single
`required` fail rejects the issue; the remaining required checks are still
graded and reported rather than short-circuited.

`unclear` counts as `fail` on a required check: a first issue whose
liveness, scope, claim state, or contribution policy cannot be verified
from the evidence is not a first issue to take. On a `preferred` check,
`unclear` is recorded and otherwise ignored.

`preferred` checks never change the verdict. They rank accepted
candidates: among issues that pass every required check, prefer the one
with more preferred passes, and say so in the summary.
