---
name: systematic-debugging
description: Use when something behaves differently than expected — a failing test, an error or stack trace, a crash, a flaky build, "it works locally but not in CI". Finds the root cause before changing code. Not for adding new features.
---

# Systematic debugging

No fix without a root cause. A change that makes the symptom disappear without an explanation is a guess, and guesses come back.

## 1. Build a failing loop first

Before forming theories, get a command that reproduces the problem and fails, and that you can run in seconds: a unit test, a script, a curl, a single Gradle task. If you cannot reproduce it, say so and collect more evidence (logs, inputs, versions, environment) instead of changing code.

## 2. Read the evidence

- Read the full error and the first stack frame that belongs to this codebase.
- Check what changed recently: `git log -p` on the affected files, dependency bumps, config, environment.
- Compare a working case with the failing one and list every difference.

## 3. Hypothesize and test one thing at a time

- Write down the hypothesis in one sentence, and the observation that would confirm or kill it.
- Change one variable per experiment. Add temporary instrumentation (logs, assertions) where the hypothesis predicts something visible.
- Remove the instrumentation when you are done.

## 4. Fix at the source

- Fix where the wrong value is produced, not where it is noticed.
- Turn the failing loop into a permanent regression test if it is not one already.
- Run the regression test (must pass now, must have failed before) and the surrounding suite.

## Stop rules

- **Three failed fix attempts means stop.** Do not try a fourth variation. Report what you tried, what each attempt showed, and what you now suspect (often the problem is in the design or the assumption, not the line you keep editing).
- Do not disable, skip or loosen tests, linters or type checks to make the problem go away.
- Do not "fix" by catching and ignoring the exception.

## Secrets

Logs, environment dumps and HTTP traces often contain tokens, passwords and personal data. Redact them before printing anything to the conversation. Never print the contents of `.env` files, keystores or credential stores.

## Output

Root cause (one or two sentences) → evidence → fix → how it was verified (command and result) → anything still unexplained.

## Known failures

_Record real mistakes here with a date and the rule that prevents them._
