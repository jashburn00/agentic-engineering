---
name: verify
description: Confirm a change actually does what it should by running it and observing real behavior — not by reasoning that it should work. Run the app, feature, or fix; watch what happens; compare against expected; report with evidence. Use to validate a fix, feature, or local change before it lands.
---

# Verify

Prove a change works by running it and observing, not by reasoning. This is Gate B's "observe, don't assume": success requires evidence you actually saw.

Verify is runtime observation — it complements the automated tests `validate` runs by exercising the real app or feature, catching what a suite can miss: integration, I/O, UX, actual output.

## Procedure

1. **State expected behavior first.** Write down what "working" looks like for this change *before* running — so you judge against a fixed bar, not rationalize whatever happens.
2. **Find how to run it.** Prefer a project-specific launch method (a project skill or documented command). Otherwise fall back to the project type: CLI (invoke it), server (start it, hit an endpoint), TUI (drive it), library (call it from a small harness), browser app (load and interact).
3. **Exercise the change.** Run the specific behavior — the happy path, plus the previously failing case for a bug fix, and edge cases the change touches.
4. **Observe real evidence.** Capture what actually happened: output, logs, exit code, HTTP response, on-screen state, DB rows, a screenshot. Observation over inference.
5. **Compare** observed against the expected from step 1.

## Report

Give a verdict plus evidence:
- **verified** — observed behavior matches expected. Include what you ran and what you saw.
- **not verified** — observed behavior diverges. Show the gap: expected vs actual.
- **could not run** — state why (missing command, environment, credentials) rather than guessing at success.

Never report verified without an observation. Report evidence a reviewer can trust without re-running.
