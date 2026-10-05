# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of my plan, the branch I built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

cgudumotu

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66#issuecomment-5988797542

**Plan for #66**, built from my reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66#issuecomment-5864209554

**Diagnosis.** Nothing configures structlog during tests (`tests/conftest.py` has none, and `core.logging.configure_logging()` is only called from `scripts/seed_db.py`), so structlog uses its default `PrintLogger` and writes to stdout. My `--runxfail` run shows that: the warning is printed under "Captured stdout call" while the assertion fails with

```
E       AssertionError: assert ('Empty chunks list' in '' or False)
```

**Change (following the issue's suggestion, one bounded change):**
- `tests/conftest.py`: an autouse fixture that calls `structlog.configure()` with `structlog.stdlib.LoggerFactory()`, `structlog.stdlib.BoundLogger`, and `structlog.dev.ConsoleRenderer(colors=False)` as the only processor, so each event becomes a plain-string stdlib log record that `caplog` sees. It resets structlog's defaults after each test.
- `tests/unit/test_batch_processor.py`: remove the `xfail(strict=True)` marker on `test_empty_chunks_list_returns_empty`, since with `strict=True` the passing test would otherwise be reported as a failure.
- Not touching `core/logging.py`, any application code, or the other `xfail` markers in the suite.

**Why this route.** Of the two options in the issue, I chose stdlib routing over `capture_logs` because it keeps the existing `caplog` assertion unchanged and makes `caplog` work for every test, not just ones rewritten to use a context manager. I render to a string rather than using `render_to_log_kwargs`: in a scratch test, `render_to_log_kwargs` raised `KeyError: "Attempt to overwrite 'name' in LogRecord"` for a call like `logger.info("...", name=x)`, and `rag/generator/prompt_templates.py` logs `name=`.

**Test plan.** Re-run my repro command with and without `--runxfail`; expected `1 passed` both ways. Then run all of `test_batch_processor.py`, plus `pytest tests/unit -q --log-level=INFO` to confirm no key-clash errors, and compare the full unit suite's counts with `main`. So far I've only run the fixture against `test_batch_processor.py` in a scratch copy (all 11 pass); I haven't run the full unit suite with it yet.

I'm using an AI assistant (Claude) in this work. It helped draft this plan and will make the edits on my branch; I review every diff and run every test myself.

---

## Your branch

**Branch**

fix/66-structlog-caplog

**Evidence**

Commands run from the top folder of my fork's clone. Before: `main`
at `f89c06f`, from my Unit 2 reproduction (Ubuntu 22.04 VM, Python
3.11.16, pytest 9.1.1, structlog 26.1.0). After: branch
`fix/66-structlog-caplog` at commit `f91e4b5`, run on my Windows 11
machine (Python 3.11, the fork's `.venv`).

**Before** (posted in my Unit 2 repro comment)

```
$ .venv/bin/python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q
x                                                                        [100%]
1 xfailed in 0.68s
```

```
$ .venv/bin/python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q --runxfail
```
```
F                                                                        [100%]
=================================== FAILURES ===================================
_______ TestBatchEmbeddingProcessor.test_empty_chunks_list_returns_empty _______

self = <tests.unit.test_batch_processor.TestBatchEmbeddingProcessor object at 0x7464adf0a610>
processor = <ingestion.embeddings.batch_processor.BatchEmbeddingProcessor object at 0x7464b311c790>
caplog = <_pytest.logging.LogCaptureFixture object at 0x7464adf54f10>

    @pytest.mark.xfail(
        strict=True, reason="issue #66: structlog output is not captured by pytest caplog"
    )
    def test_empty_chunks_list_returns_empty(self, processor, caplog):
        """Test that empty chunks list logs warning and returns empty list."""
        result = processor.process([])

        assert result == []
        # Should log a warning
>       assert "Empty chunks list" in caplog.text or any(
            "empty" in record.message.lower() for record in caplog.records
        )
E       AssertionError: assert ('Empty chunks list' in '' or False)
E        +  where '' = <_pytest.logging.LogCaptureFixture object at 0x7464adf54f10>.text
E        +  and   False = any(<generator object TestBatchEmbeddingProcessor.test_empty_chunks_list_returns_empty.<locals>.<genexpr> at 0x7464adf512a0>)

tests/unit/test_batch_processor.py:48: AssertionError
----------------------------- Captured stdout call -----------------------------
2026-09-28 05:38:04 [warning  ] Empty chunks list provided to BatchEmbeddingProcessor
=========================== short test summary info ============================
FAILED tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty
1 failed in 0.17s
```

**After** (the built change)

```
PS C:\dev\pathreview-ai301-fa26-s1> .venv\Scripts\python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q
.                                                                                                        [100%]
1 passed in 1.29s
PS C:\dev\pathreview-ai301-fa26-s1> .venv\Scripts\python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q --runxfail
.                                                                                                        [100%]
1 passed in 0.18s
```

The same assertion that failed before (`'Empty chunks list' in caplog.text`)
now passes, with and without `--runxfail`.

The commit touches only the two planned files:

```
$ git show --stat --format='%h %s' HEAD
f91e4b5 test: route structlog into stdlib logging so caplog captures it (#66)

 tests/conftest.py                  | 21 +++++++++++++++++++++
 tests/unit/test_batch_processor.py |  3 ---
 2 files changed, 21 insertions(+), 3 deletions(-)
```

## Eval iterations

**Run history**

Before any paid run, I did a blind dry run: two helper agents on Sonnet
graded all 24 packages (20 scored plus 4 calibration) with my files,
without seeing the gold labels. It agreed on 24 of 24. It also flagged
one ambiguity: the diagnosis check did not say what happens when a plan
names no cause at all. I added that sentence to the rubric before the
official run. The dry run was not a harness run, so it has no score in
the list below.

Official harness runs, in order:

1. 20/20 (`agreement: 20/20 scored items  (bar: 18/20: PASS)`;
   `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`).
   This is the run saved in `eval-run.txt`.

My first two attempts to start the harness crashed before grading any
package, so they produced no score. The Unit 3 `run_eval.py` called a
bare `"claude"` (WinError 2 on Windows, where the CLI is `claude.cmd`)
and piped text without `encoding="utf-8"` (UnicodeEncodeError on
cp1252). I applied the same two fixes the Unit 2 harness already had,
and then ran it once in full.

**Package analysis**

`pkg-16` (pandas-dev/pandas#57666, category wrong-cause). My rubric
decided **reject**, and the gold label is **reject**.

The plan's diagnosis says: "The defect is in the pandas-side cast that
runs after pyarrow returns its table ... The cast is where the data is
damaged, so the cast is what must change." The package's own repro
evidence rules that out. Step 4 says: "the table's column arrives as
`int64` with value `1`; the zeros are already gone in the parsed table,
before any cast to string could run." Step 3 shows pyarrow keeps `"01"`
when told to parse the column as a string.

My rubric reads it this way because of the order the procedure
imposes. The grader reads the repro evidence *before* the plan and
writes a "ruled out" list (here: "the post-read cast: zeros are already
gone before it runs"). Then "Diagnosis fits the repro evidence" asks
whether the plan blames anything on that list. It does, so the check
fails. The rubric's pass condition names this exact pattern ("shows
the failure happening **before or without** it"). This plan is long,
specific, and confident, so a grader that read the plan first could
easily be talked into it. Re-padding cannot recover widths that were
never stored, which is why the evidence has to outrank the plan's
story.

**Check rationale**

The check, quoted from `tools/plan-check/rubric.md`:

> | Comment meets the repo's AI and comment policy | The repo-facts block's contribution policy and AI-policy line (live: CONTRIBUTING and any AI policy file), read against the comment's text. Treat every package as AI-assisted work. | Whatever the repo's stated policy requires **of comments** is satisfied. A policy that requires disclosing **all** AI usage in any form, or AI use in issues and comments, requires the comment to state the tool and the extent of the help. A policy that requires disclosure only in pull requests does not bind a comment. A policy requiring comments in the contributor's own words is satisfied by a comment that reads that way. Silence passes: no stated policy is no requirement. | required |

Why it reads this way: before writing it I listed the contribution
policy line of all 24 packages. The repos ask for very different
things. ghostty (pkg-20) requires disclosing "all AI usage in any form".
fd (pkg-09) requires disclosure only in the pull request and states "no
disclosure ask for issue comments". ripgrep (pkg-03, calib-04) wants
comments "written by humans in their own words". Most repos state
nothing.

I rejected a simpler version, "the comment must disclose AI use". It
would have wrongly rejected pkg-09 and every clear accept whose repo
says nothing. So the check scopes itself to what the policy requires
**of comments** and spells out the PR-only case. The line "Treat every
package as AI-assisted work" closes a loophole: without it, a grader
could pass pkg-20 by reasoning that maybe no AI was used, so nothing
needed disclosing. This is the same check I wrote for Unit 2, carried
forward, because the policies did not change between a repro comment
and a plan comment.

**Trade-offs**

This check accepts that it will miss a comment that is plainly
AI-written in a repo that requires comments "in your own words",
whenever the comment *reads* human. "Reads that way" is a judgment
call, so a polished AI-drafted comment can pass where a maintainer
might hide it. I accept that miss. Grading prose style for AI tells
would make graders disagree with themselves, and no scored package
depends on it.

I also checked that the PR-only carve-out changes nothing elsewhere.
The only packages whose result it could flip are the ones with an AI
policy: pkg-03, pkg-05, pkg-07, pkg-09, pkg-12, pkg-20 and calib-04.
In the official run all of them agree with gold. pkg-09 (the PR-only
case) is accepted, and pkg-20 (the all-usage case) is rejected, so the
carve-out loosened the check exactly where intended without flipping
the thread-convention package it has to catch.
