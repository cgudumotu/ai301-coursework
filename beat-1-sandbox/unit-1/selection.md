# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66

**Verdict output**

Live-mode output, pasted verbatim. Three candidates were graded in one run
(#73, #71, #66); the skill's multi-candidate form emits one fenced JSON block
holding an array, accepted candidates first in fit order, so the selected
issue #66 is the first object and its verdict is `accept`.

All three candidates pass every required check, so the ranking is by fit profile only.

Repo-level evidence (applies to all three, measured against 2026-09-20)
- Maintainer alive: newest default-branch commit is 2026-09-16 by a human collaborator, 4 days ago.
- Repo in use: not archived, last push 2026-09-16. No releases exist, which does not fail the check.
- AI policy: docs/CONTRIBUTING.md and the PR template say nothing about AI use. Silence passes.
- Maintainer answers fast: issues 52 and 43 each got a collaborator reply 6 days after opening.

Ranked read-out

Accepted, in fit order:
1. #66 structlog not captured by pytest caplog. Python test configuration in tests/conftest.py, with an exact reproduction command and a named cause. This is the closest match to the profile's fastest area and its stated preference for a clear reproduction.
2. #71 heading hierarchy fixture indented. Python test fix with a named cause and an explicit acceptance step, removing the xfail marker. Slightly narrower learning value than #66, and labeled tier-2.
3. #73 README and .env.example disagree. Docs and small-config work, which the profile is happy to take, but the least Python content of the three.

Rejected: none.

Both preferred checks pass on all three, so they do not separate the candidates. No claim comments, assignees, or linked pull requests exist on any of them, so the Path Review house rule never came into play.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16T21:42Z by Aburke225 (human User), 4 days before today"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48Z; no release exists (not disqualifying)"},
      {"name": "Scope fit", "grade": "pass", "evidence": "One bounded change: configure structlog in tests/conftest.py so caplog assertions work; repro command given; no comments, no disagreement, no linked PRs"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; zero comments; timeline shows no cross-referenced or connected PRs"},
      {"name": "AI policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI-use statement; silence passes"},
      {"name": "Beginner-signposted", "grade": "pass", "evidence": "Opened by Aburke225 with author_association COLLABORATOR; labels bug, tests, tier-1"},
      {"name": "Maintainer answers fast", "grade": "pass", "evidence": "Issue #52 opened 2026-09-10, first COLLABORATOR reply 2026-09-16 (6 days)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/71",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16T21:42Z by Aburke225 (human User), 4 days before today"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48Z; no release exists (not disqualifying)"},
      {"name": "Scope fit", "grade": "pass", "evidence": "One bounded change: remove 8-space indentation from test fixture and drop the xfail marker in tests/unit/test_readme_parser.py; no comments, no linked PRs"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; zero comments; timeline shows no cross-referenced or connected PRs"},
      {"name": "AI policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI-use statement; silence passes"},
      {"name": "Beginner-signposted", "grade": "pass", "evidence": "Opened by Aburke225 with author_association COLLABORATOR; labels bug, ingestion, tier-2 (no good-first-issue label, maintainer-opened suffices)"},
      {"name": "Maintainer answers fast", "grade": "pass", "evidence": "Issue #52 opened 2026-09-10, first COLLABORATOR reply 2026-09-16 (6 days)"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "Maintainer alive", "grade": "pass", "evidence": "Newest main commit 2026-09-16T21:42Z by Aburke225 (human User), 4 days before today"},
      {"name": "Repo in use", "grade": "pass", "evidence": "archived: false; pushed_at 2026-09-16T21:48Z; no release exists (not disqualifying)"},
      {"name": "Scope fit", "grade": "pass", "evidence": "One bounded change: make README.md and .env.example agree on OPENROUTER_API_KEY / LLM_PROVIDER; two files named, 1-2 hour estimate; no comments, no linked PRs"},
      {"name": "Unclaimed", "grade": "pass", "evidence": "assignees: none; zero comments; timeline shows no cross-referenced or connected PRs"},
      {"name": "AI policy allows this workflow", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and .github/PULL_REQUEST_TEMPLATE.md contain no AI-use statement; silence passes"},
      {"name": "Beginner-signposted", "grade": "pass", "evidence": "Carries good first issue label; also opened by COLLABORATOR Aburke225; labels bug, good first issue, docs, tier-1"},
      {"name": "Maintainer answers fast", "grade": "pass", "evidence": "Issue #52 opened 2026-09-10, first COLLABORATOR reply 2026-09-16 (6 days)"}
    ],
    "verdict": "accept"
  }
]
```

---

## Eval iterations

**Run history**

1. `--only issue-01` — ERROR, no score. The `claude` CLI was not authenticated;
   no bundle was graded. No rubric change.
2. `--only issue-01` — agreement 1/1 (not a scored bar run). Confirmed the
   harness worked end to end before spending a full run. No rubric change.
3. Full run — **agreement 17/20 scored items, below the bar.** Categories:
   claimed 4/4, clear-accept 6/8, dead-repo 3/3, policy 1/1, scope 3/4.
   Three disagreements, all on the `Scope fit` check: issue-01 and issue-19
   graded reject against gold accept, issue-20 graded accept against gold
   reject. Rewrote the `Scope fit` pass condition in response.
4. `--only issue-01,issue-19,issue-20` — agreement 3/3 on the three repaired
   issues (partial run, no bar printed). Confirmed the rewrite before paying
   for a full run. No further rubric change.
5. Full run — **agreement 19/20 scored items, bar 18/20: PASS.** Categories:
   claimed 4/4, clear-accept 7/8, dead-repo 3/3, policy 1/1, scope 4/4 — the
   category floor holds. The `Scope fit` rewrite flipped issue-19 and issue-20
   to agree without regressing any of the seventeen that already agreed.
   One disagreement remains, issue-01, analysed below. This is the run
   recorded in the committed `eval-run.txt`.

Three failures in runs 1–2 were environment, not rubric: `subprocess.run(["claude", …])`
raised `WinError 2` because `CreateProcess` does not apply `PATHEXT` on Windows;
`text=True` then encoded the prompt with `cp1252`, which cannot represent the `✨`
in issue-03's `Enhancement ✨` label; and the CLI reported "Not logged in" on stdout
while the harness echoed only stderr, hiding the reason. I fixed all three in
`eval/run_eval.py` before any grading happened.

**Issue analysis**

`issue-01` (conda/conda#16475, "Add permanent docs for installing PyPI packages with
`conda install`"). **My rubric's decision: reject. Gold label: accept.** This is the
one disagreement left in the committed run, and it is a `Scope fit` failure.

The issue is a documentation task. Its body states the problem in a paragraph, then
opens a "Proposed changes" section that asks for a new standalone task page and lists
what that page should cover — the workflow itself, the `conda-pypi` channel and its
governance, required setup and channel priority, basic usage, `environment.yml`
migration, behaviour under `conda list`/export/remove, and troubleshooting — and then
adds that the existing pages mentioning older guidance should be updated too.

My `Scope fit` check asks for "one bounded deliverable — work a newcomer could land in
a single pull request". The model reads that outline, counts eight content areas plus
edits to other pages, and concludes the work exceeds one pull request. Both preferred
checks also fail here (no beginner label, and conda's fastest sampled first response is
32.9 days), so nothing in the issue pushes back against that reading — though as
preferred checks they did not cause the reject.

Gold says accept, and on reflection gold is right: the eight bullets describe sections
of *one page*, and the issue names exactly where that page goes. A detailed outline is
a specification for a single deliverable, not a list of separate deliverables. My
run-3 rewrite already added "a long body or a detailed content outline: that is a spec
for one deliverable, and detail makes an issue easier, not bigger" as an explicit
non-disqualifier, and it moved issue-19 and issue-20 but was not enough to move this
one. The signal it still misses is *who the list is for*: a list of topics one person
writes in one sitting versus a list of tasks several people divide.

I stopped here deliberately rather than pushing for 20/20. The remaining fix would mean
loosening `Scope fit` a third time, and the two issues it must keep rejecting —
issue-05 ("PRs welcome both big and small" across the codebase) and issue-10 (an
explicit list of sub-issues) are *also* long lists inside one issue body. The
distinction between them and issue-01 is real but narrow, and a looser rule risks
trading one agreement for two. With the bar met at 19/20 and the category floor held,
the honest move is to record the miss and the reason rather than overfit the wording to
a single bundle.

**Check rationale**

The check, quoted as it is currently written in `tools/issue-select/rubric.md`:

> | Unclaimed | `assignees:` and `linked PRs:` (with per-PR state) in the repo-facts block, plus every comment in the thread with its date and author_association. | All three hold: (1) `assignees: none`; (2) **no linked PR in `open` state**, and no PR described in the thread as open or in progress (closed or merged PRs do not block); (3) **no claim comment** ("I'll take this", "working on this", "can I work on it", `/assign`, `@bot claim`) dated **within 365 days** of the capture date, unless a maintainer replied after it saying the issue is still free. A maintainer comment inviting takers, or a bot nudge asking the claimant whether they are still active, does **not** clear a claim that is otherwise live (an assignee or an open PR). In live mode, apply any house rule in `scope.md` that changes whose claim comments count. | required |

Three decisions inside that wording:

*It is `required`, not `preferred`.* The shipped template weights it `preferred`, which
is the single highest-cost default in the starter: four of the twenty scored issues
(03, 08, 13, 18) are rejected by gold *only* because someone is already working on them.
A preferred check never changes a verdict, so leaving the default in place accepts all
four, caps agreement at 16/20, and leaves the `claimed` category at 0 — failing the
category floor as well as the bar.

*It reads three signals independently.* Issue-18 has `assignees: none` and would pass an
assignee-only check, but carries open linked PRs and a stack of "can I pick this up?"
comments. Issue-08 has both an assignee and an open PR. Requiring all three to clear
catches both shapes.

*Open-state and the 365-day window are the load-bearing numbers.* Without them the check
is too blunt and eats good issues. Issue-09 is the case: opened 2018, a 2022 comment
reading "I'd like to take a swing at this as my first open-source contribution", a
linked PR, and a stale-bot nudge. Gold says accept. It passes because the linked PR
`conda/conda#11627` is **closed** rather than open (an abandoned attempt is evidence
the issue is free, not that it is taken), the claim is ~1,660 days old against a
365-day window, and the maintainer `jakirkham` replied "Think you can just give it a
try if you are interested".

**Trade-offs**

The 365-day claim window is what this check gives up, and issue-09 is the case that
shows both sides of it. The window is tuned to an eval set where live claims are days
or weeks old and dead ones are years old — nothing sits near the boundary, so any
threshold from roughly six months to three years scores identically here. That is a
warning rather than a comfort: the number is unfalsified, not validated. On a
fast-moving repo a silent 90-day-old claim is already abandoned and this check would
have me skip an issue I could take; on a slow repo a 400-day-old claim might still be
live and it would send me into a collision. I accept that it will misread exactly those
cases, because the eval set contains none of them and I have no evidence on which to
tune it further.

I also accept a second cost: the check cannot see claims that leave no trace in the
fields it reads — a classmate who has started work but not commented, opened a PR, or
taken an assignment is invisible to it. In Path Review the house rule makes that
harmless, since shared issues cost nobody anything there. On a real repo it is a real
gap, and the mitigation is a claim comment of my own rather than a better check.

Nothing changed elsewhere when I fixed `Scope fit` after run 3: the `Unclaimed`,
`Maintainer alive`, `Repo in use` and `AI policy` checks agreed with gold on all twenty
issues in run 3, and run 5 re-graded the full set to confirm the scope rewrite did not
regress any of them.

---

## Selection rationale

**Selection rationale**

*1. Fit to my interests and to the time available.* Issue #66 is a Python
test-configuration fix — `structlog` records not reaching pytest's `caplog` — in
`tests/conftest.py`. Python is the language I write every day, and my fit profile in
`scope.md` says I am fastest there and that I prefer issues with a clear reproduction.
#66 ships an exact reproduction command and a named cause, so the diagnosis time is
close to zero and the work is one file. That matters because my time budget for Unit 2
is evenings, not full days. It also teaches me something I actually want: how pytest
captures log records, which is closer to the testing and system-design skills I am
building than a docs correction would be.

*2. What the verdict identified correctly, and what I weighed that the rubric could not.*
The rubric got the mechanical part right and honestly: all five required checks passed
on all three candidates, so the verdict correctly told me that nothing disqualifies any
of them — no dead repo, no claim, no AI-policy problem, no scope trap. What it could not
weigh is which of three equally-valid issues is the right one for *me*. Both preferred
checks passed on all three as well, so they did not separate the candidates either. The
fit profile broke the tie toward #66, and I agreed with it for a reason the rubric does
not encode: #73 carries the only explicit `good first issue` label, and I deliberately
did not let that decide. A label is a maintainer's claim that an issue is *friendly*,
not a claim that it is the right issue for my goals. I also weighed learning value
over safety — #73 is the lower-risk task, and I chose the one I would learn more from.

*3. Anticipated difficulty in claiming it.* Low. #66 has no assignee, no comments, and
no linked PRs, so there is nobody to negotiate with, and the Path Review house rule
means a classmate's claim would not block me even if one appeared before Unit 2. The
real difficulty is not claiming but reproducing: the fix depends on how `structlog` is
configured in this project's test setup, and a logging-propagation bug can look fixed
locally while depending on test ordering or on handlers another test installed. I have
not commented on the issue — choosing is not claiming, and the claim comment belongs to
Unit 2.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
