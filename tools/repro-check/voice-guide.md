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
