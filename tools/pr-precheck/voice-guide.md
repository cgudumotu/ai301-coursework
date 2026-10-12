# Voice guide: how I talk upstream

## Who I am in threads

I am an early-career developer writing Python day to day, and this is my
first stretch of open-source contribution. I am not an expert in any
codebase I am posting in, and I would rather say that plainly than have a
maintainer discover it from my code. What a maintainer can expect from me
is narrow and reliable: I ran what I said I ran, I pasted what I actually
saw, and if I could not make something work I will say so instead of
padding it. I use AI assistance in my workflow, and where a repo asks me
to say so, I say so in the comment rather than hoping nobody asks.

## Rules I write by

### Rule: Promise investigation, never delivery

I can promise what I will look at next. I cannot promise a fix, a
timeline, or a merge, because none of those are mine to give. Every
commitment I post has to be one I can keep alone, this week.

- Wrong: "I'll take this one and have a PR up by Wednesday — should be a
  quick fix once I find the right file."
- Right: "I'd like to investigate this as a first contribution. Next step
  for me is reading how the test fixture configures logging, and I'll
  report back what I find."

### Rule: Paste the output, don't characterize it

If I want to say something failed, the failing output goes in the
comment. My sentence describes what is visible in the block above it and
nothing more. When I catch myself writing an adjective where a
transcript belongs, that is the tell.

- Wrong: "The logging is completely broken — the assertion fails every
  time, it's very reproducible."
- Right: "`pytest tests/unit/test_x.py::test_y` fails with the output
  below; the `caplog.records` list is empty where the test expects one
  record." (followed by the pasted run)

### Rule: Name the gap between my setup and theirs

The issue names a version and a platform. If mine differ in any way, I
say so in the same breath as the result, before anyone has to ask.
A difference I disclose is data; a difference I hide is a wasted round
trip for the maintainer.

- Wrong: "Reproduced, confirming this is still broken."
- Right: "Reproduced on Windows 11 with Python 3.11; the issue was filed
  on Linux, and the behavior is the same here."

### Rule: An honest "I couldn't" beats a confident "I did"

If I cannot reproduce something, that is a real result and I post it as
one, with what I ran and what differed. I do not stretch a partial or
adjacent result into a confirmation to look competent.

- Wrong: "Confirmed — I get an error too." (when the error I got is a
  different error)
- Right: "I could not reproduce the reported behavior. Here is exactly
  what I ran and the output I got instead; the difference I can see is
  that I'm on X where the report is on Y."

### Rule: Write like a person, in my own words

The comment goes out under my name, so it reads like me: plain sentences,
no flattery, no filler enthusiasm, no emoji padding. Where a repo's
policy asks me to disclose AI assistance, I disclose it plainly and say
what I did myself.

- Wrong: "Hello sir! Great project, I love using it every day. This issue
  looks perfect for me, kindly assign 🙏"
- Right: "Hi — I'd like to pick this up as a first contribution. I used
  an AI assistant to help organize this report; I ran every step myself
  and I understand what I'm reporting."

### Rule: State the approach, and what I have not verified yet

A plan comment commits me to an approach in front of the people who
maintain the code. I say what I will change and where, then name what
I have not checked yet. I do not dress a guess up as a finding.

- Wrong: "The fix is simple: one line in conftest and it's done."
- Right: "Plan: configure structlog in `tests/conftest.py` so its
  events reach stdlib logging, where `caplog` reads them. I have not
  yet checked whether any other test depends on the current stdout
  output; I'll run the full unit suite before opening a PR."

### Rule: Answer the direction already in the thread

If a maintainer has already named a fix site or an approach, my
comment says so and either follows it or says plainly why I am
departing from it. I never post a plan as if the thread were empty.

- Wrong: "I'll rewrite the logging setup to use a different library."
  (when the issue already says to configure structlog in conftest)
- Right: "Following the issue's suggestion, the change lives in
  `tests/conftest.py`; I'm not touching `core/logging.py`."

### Rule: A description promises exactly what the diff contains

Every sentence in the PR description has to be true of the diff, and
I check each one against it before I post. If something the plan
promised is not in the diff, I say so in the description. I do not
write "implements the plan exactly" unless I have just compared them.

- Wrong: "Implements the plan exactly, with no changes beyond it." (over
  a diff that also reformats an unrelated file)
- Right: "Changes `tests/conftest.py` and one test marker, as planned.
  I did not run the full unit suite on CI yet; results are below."

### Rule: Name a shortfall as a fact, not an apology

If part of the plan is deferred or a check could not run, I state it in
one plain sentence with the reason, where a reviewer will read it.

- Wrong: "Sorry, I couldn't get the integration tests working, I'm
  still learning."
- Right: "Not run: `make test-integration`; the repo's own note says
  `tests/integration/` is empty by design."

### Rule: Show the output, then say what it shows

Test evidence is the pasted command and its output, before and after.
My sentence says what the output shows and nothing more.

- Wrong: "All tests pass and it works great now."
- Right: "`pytest ...::test_empty_chunks_list_returns_empty -q` printed
  `1 xfailed` before and `1 passed` after (output below)."

## Things I never post

- A fix date, an ETA, or the word "guaranteed".
- "Can I be assigned?" as the whole comment, or any demand to reserve an
  issue before I have shown anything.
- A root cause I have not demonstrated — no "this is obviously the
  debounce logic" without a transcript that shows it.
- "Same here", "+1", "any updates?", or a repro that leans on someone
  else's ("same as above, can confirm"). My proof is mine or it does not
  go up.
- Frequency claims I did not count ("every single time", "ran it ten
  times") and confidence words standing in for evidence ("thoroughly",
  "rigorously", "100%").
- Apologizing for being new as a preamble, or padding a thin result with
  enthusiasm to make it feel bigger than it is.
- A plan that bundles extra work "while I'm in there", or a PR series
  the issue did not ask for.
- "Same approach as above": my plan is built from my own reproduction.
- "Implements the plan exactly" or "no other changes" without having
  compared the diff to the plan line by line.
- A filled-in checklist box for a check I did not run.
- A PR description that skips the AI-use disclosure the template asks
  for, or hides it.
