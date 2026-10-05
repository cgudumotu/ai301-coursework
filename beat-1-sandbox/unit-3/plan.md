# Plan: issue #66 — structlog output is not captured by pytest caplog

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66
My reproduction: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/66#issuecomment-5864209554

## Repro evidence this plan relies on

From my posted reproduction on a clean clone of `main` at `f89c06f`
(Python 3.11.16, pytest 9.1.1, structlog 26.1.0):

- Run 1, the issue's command, reports `1 xfailed`: the test carries
  `@pytest.mark.xfail(strict=True, reason="issue #66: ...")`.
- Run 2, the same test with `--runxfail`, fails:

```
E       AssertionError: assert ('Empty chunks list' in '' or False)
E        +  where '' = <_pytest.logging.LogCaptureFixture object at 0x7464adf54f10>.text
----------------------------- Captured stdout call -----------------------------
2026-09-28 05:38:04 [warning  ] Empty chunks list provided to BatchEmbeddingProcessor
```

- `caplog.text` is the empty string, while the same warning appears
  under "Captured stdout call". The code under test does emit the
  event; it never reaches the stdlib `logging` system that `caplog`
  hooks into.

## Diagnosis

`BatchEmbeddingProcessor` logs through `structlog.get_logger()`
(`ingestion/embeddings/batch_processor.py`, line 6). Nothing in the
test run configures structlog: `tests/conftest.py` has no structlog
setup, and `core.logging.configure_logging()` is only called from
`scripts/seed_db.py`. With no configuration, structlog uses its
default `PrintLogger`, which writes the rendered line straight to
stdout. That matches the artifact above exactly: the line shows up in
captured stdout and `caplog.records` stays empty. `caplog` only sees
records that pass through stdlib `logging`, so the fix is to route
structlog into stdlib logging during tests, as the issue suggests.

## Scope

In scope (one bounded change, following the issue's own suggestion):

1. `tests/conftest.py`: add an autouse fixture that configures
   structlog to hand every event to stdlib `logging`, and resets
   structlog's defaults after each test.
2. `tests/unit/test_batch_processor.py`: remove the
   `@pytest.mark.xfail(strict=True, ...)` marker from
   `test_empty_chunks_list_returns_empty`. It has to go: with
   `strict=True`, a test that now passes is reported as a failure
   (XPASS(strict)).

Not in scope:

- `core/logging.py` and how the application configures logging in
  production or development.
- Any change to `batch_processor.py` or other application code. The
  code already logs the right event.
- The other `xfail` markers in the suite (`test_bias_detector.py`,
  `test_pii_scrubber.py`, and others). They point at other issues.

## Files

- `tests/conftest.py`
- `tests/unit/test_batch_processor.py`

## Approach

1. In `tests/conftest.py`, add `import structlog` and an
   `@pytest.fixture(autouse=True)` named `structlog_to_stdlib` that
   calls `structlog.configure(...)` with:
   - `logger_factory=structlog.stdlib.LoggerFactory()`, so loggers are
     real stdlib `logging.Logger` objects;
   - `wrapper_class=structlog.stdlib.BoundLogger`;
   - `processors=[structlog.dev.ConsoleRenderer(colors=False)]`, so
     each event is rendered to one plain string (the event text plus
     any `key=value` pairs) and handed to stdlib as the log message;
   - `cache_logger_on_first_use=False`, so the module-level
     `logger = structlog.get_logger()` picks up this configuration.
   The fixture then yields, and calls `structlog.reset_defaults()` on
   teardown so no configuration leaks between tests.
2. Remove the `xfail` marker from the one test.
3. Re-run the repro (test plan below), then the whole
   `tests/unit/test_batch_processor.py` file, then the full unit suite.

Choices made, and why:

- **Route into stdlib, not `structlog.testing.capture_logs`.** The
  issue offers both. `capture_logs` is a context manager each test
  would have to use *instead of* `caplog`, so it means rewriting test
  assertions. Routing structlog into stdlib keeps the existing
  `caplog` test unchanged and makes every future `caplog` assertion
  work, which is what the issue title asks ("fail suite-wide").
- **Render to a string, not `render_to_log_kwargs`.** I first tried
  `render_to_log_kwargs`, which passes key-value pairs as stdlib
  `extra`. A scratch test showed it raises
  `KeyError: "Attempt to overwrite 'name' in LogRecord"` for a call
  like `logger.info("...", name=x)` once INFO is captured, and
  `rag/generator/prompt_templates.py:132` logs `name=name`. Rendering
  to a single string passes no `extra`, so no key can clash.
- **A test fixture, not `configure_logging()`.** That function reads
  app settings and adds timestamps and color codes. A minimal
  test-only configuration keeps the message close to the event text
  the test checks, and leaves app logging untouched.

## Test plan

Re-run my Unit 2 repro steps against the change:

```
.venv/bin/python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q
.venv/bin/python -m pytest tests/unit/test_batch_processor.py::TestBatchEmbeddingProcessor::test_empty_chunks_list_returns_empty -q --runxfail
```

- Before (posted in my repro): `1 xfailed`, and with `--runxfail`
  `AssertionError: assert ('Empty chunks list' in '' or False)`.
- Expected after: both commands print `1 passed`. The passing test is
  itself the proof that `caplog.text` now contains
  `Empty chunks list provided to BatchEmbeddingProcessor`, because that
  is the assertion that failed before.
- Key-clash check: `pytest tests/unit -q --log-level=INFO` runs with no
  `KeyError: "Attempt to overwrite ... in LogRecord"`.
- Regression check: `pytest tests/unit/test_batch_processor.py -q`
  passes all tests in the file, and `pytest tests/unit -q` shows no new
  failures compared with `main` (same pass/xfail counts apart from
  this test moving from xfailed to passed).

## Risks and unknowns

- I prototyped this fixture in a scratch copy: the target test passed
  with and without `--runxfail`, and all 11 tests in
  `test_batch_processor.py` passed, also with `--log-level=INFO`. I
  have not yet run the full unit suite with it on my machine.
- Log messages captured in tests now include `key=value` pairs after
  the event text (for example `... chunk_count=3`). A future test that
  compares `record.message` with exact equality would need to account
  for that; the current test uses `in`, so it is unaffected.
- Because the fixture is autouse, every test now logs through stdlib.
  Any test that relied on structlog printing to stdout (via `capsys`)
  would change. A search of `tests/` found no `capsys` or `capfd`
  use, so I expect none.

## Deviations

Nothing changed between the plan I posted and the change I built; the plan held.
The commit (`f91e4b5` on `fix/66-structlog-caplog`) touches exactly the two
planned files: the autouse fixture in `tests/conftest.py` using
`ConsoleRenderer(colors=False)`, and the removed `xfail` marker in
`tests/unit/test_batch_processor.py` (21 insertions, 3 deletions).

One change did happen, but before I posted, so the posted plan already
reflects it: my first draft used `structlog.stdlib.render_to_log_kwargs`. My
own plan-check run flagged the key-clash risk, a scratch test confirmed
`KeyError: "Attempt to overwrite 'name' in LogRecord"`, and I switched to
rendering a plain string before posting.

One part of the test plan hit something outside this change. On my Windows
machine, `pytest tests/unit -q --log-level=INFO` stopped while collecting
`tests/unit/test_review_service.py` with
`sqlalchemy.exc.NoSuchModuleError: Can't load plugin: sqlalchemy.dialects:postgres`.
The traceback is entirely in `core/database.py` and SQLAlchemy, at import time,
before any test or fixture runs. A `DATABASE_URL` in my shell environment uses
the old `postgres://` scheme and overrides the app's `postgresql+asyncpg://`
default. It is not caused by this change. I'll re-run the full unit suite with
that variable cleared before opening the PR in Unit 4.
