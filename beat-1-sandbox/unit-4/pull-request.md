# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request I opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s1/pull/151

**Branch**

fix/66-structlog-caplog

**My own pr-precheck verdict on the draft** (live mode, run before opening the PR)

I ran `pr-precheck` in live mode on my PR title and description (`pr-description.md`) before
opening the PR, twice.

1. **First run: reject.** Two required checks failed, "Evidence exercises the plan's repro" and
   "Template and stated file asks met". My draft still had two unfilled result
   placeholders (for the mypy and the unit-test results) in its Testing section, and it never showed the
   `--log-level=INFO` run that my plan's test plan names. I ran mypy and the unit suite, pasted
   the real output into the description, and added the `--log-level=INFO` run.
2. **Second run: accept**, with all seven required checks passing and the JSON
   `"verdict": "accept"`. The closest call was "Repo's own checks run", because I ran the repo's
   commands directly instead of through `make` (not available on my Windows machine), and the
   description says so. One preferred check still failed, "Title and commits follow the repo's
   form": the PR title follows `<type>(<scope>): <description>`, but my one commit subject is
   `test: route structlog into stdlib logging so caplog captures it (#66)` with no scope. I did
   not amend it, because my Unit 3 files cite that commit's hash (`f91e4b5`) and a preferred
   check never changes the verdict.

I opened the PR after the second run.

## Eval iterations

**Run history**

Before spending course credit, I did one blind dry run: two helper agents on Sonnet
graded all 24 packages (20 scored plus 4 calibration) with my four files, without seeing the
gold labels. It matched 24 of 24, but it flagged one close call: in `pkg-02`, an accepted
package, the diff re-prints unchanged lines as `-`/`+` pairs, which my "Diff is reviewable"
check names as a debris tell. I added one sentence to that check to separate lines re-printed
by diff rendering from real churn, then ran the harness. The dry run is not a harness run, so
it has no score below.

Official harness runs, in order:

1. 19/20 (`agreement: 19/20 scored items  (bar: 18/20: PASS)`;
   `categories: clear-accept 6/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`).
   This is the run saved in `eval-run.txt`, and it is my only harness run. I did not re-run, so
   the last score matches the file.

(Before this run, starting the harness on Windows also failed twice before grading anything,
so those attempts have no score: it needed `shutil.which("claude")` and `encoding="utf-8"`,
which I added to my local copy of `run_eval.py`.)

**Package analysis**

`pkg-02` (Textualize/rich, category clear-accept). My rubric decided **reject**; the gold
label is **accept**. This is the one disagreement in the run, and the `note` column in
`eval-run.txt` names the checks it failed: `failed: Diff is reviewable, Description states its
limits`.

What the package contains: a fix that makes `save_text`, `save_html`, and `save_svg` export
with `clear=False`, write the file, and clear the record only after the write succeeds. The
diff matches the plan's shape, the description claims only what the diff does, a CHANGELOG
entry is in the diff, and the before/after output shows the record surviving a failed save.

Why my rubric read it as a reject: my "Diff is reviewable" check fails when "any debris tell
above sits in the diff", and one of the tells is identical lines re-printed. In the
`save_html` and `save_svg` hunks, the diff shows `html = self.export_html(` and `theme=theme,`
(and `title=title,` in `save_svg`) as both `-` and `+` lines, with only `clear=clear` changing
to `clear=False` beside them. I read that as the grader applying the tell literally. The gold
label treats it as diff rendering around a one-word change, not churn. My dry run had flagged
exactly this as the closest call among the accepts, and the sentence I added ("one unchanged
line ... beside a line the fix changes") was too narrow: here two or three lines are repeated
beside the changed one. I did not re-run to confirm the grader's exact reasoning; the
`note` column is the only record of which checks failed.

**Check rationale**

The check, quoted from `tools/pr-precheck/rubric.md`:

> | Diff is reviewable | Every added and removed line of the diff, and the commit list. Look for the debris tells: debug prints or logging left in, commented-out code, dead or unused functions, TODO or WIP markers, re-printed identical lines, import or whitespace re-ordering, and hunks unrelated to the fix. | Every added or removed line is part of the fix or its tests. Comments that explain the fix are not debris. A `-`/`+` pair that re-prints one unchanged line only because it sits beside a line the fix changes is diff rendering, not churn; a whole block re-printed with no changed line in it is churn. **Fails** when any debris tell above sits in the diff, **even mechanical churn around a correct fix**: the reviewer must dig the fix out of it. | required |

Why it reads this way: the evidence column lists the debris tells that the gold notes for the
unreviewable packages name (`pkg-12`: commented-out debug line, dead `_unused_buffer_probe`
fn, TODO; `pkg-15`: DEBUG `eprintln`, dead `count_open_fds`; `pkg-18`: import-restructure hunk,
re-printed identical blocks, commented-out `eprintln`). The pass condition asks whether every
changed line is the fix or its tests, so it judges the diff itself. I rejected two ways of
reading it: grading the commit subjects ("wip", "fmt + cleanup"), because that is the write-up's
shape and not the diff, and treating mechanical churn around a correct fix as acceptable,
because `pkg-18`'s gold label rejects exactly that. The sentence about diff rendering is what
I added after the dry run, to protect `pkg-02`.

**Trade-offs**

This check gives up `pkg-02`: it changed that package's result to a reject where the gold label
is accept, and it is the reason my score is 19 and not 20. I accept that a diff where
unchanged lines sit beside a changed one can look like churn to the grader; telling diff
rendering from churn reliably is a judgment my wording only half captures, so I count it as a
case my check will sometimes miss.

I did not re-run with `--only` to test a looser version, because a full re-run costs about $5
of course credit and the bar was already met. So I do not know the effect of loosening it on
the three other unreviewable packages. From their gold notes, `pkg-15` and `pkg-18` carry
other debris tells that would still fail them (a DEBUG `eprintln` and dead function in
`pkg-15`; a commented-out `eprintln` in `pkg-18`), but that is my reading of the notes, not a
measured result.
