---
name: engineering-standards
description: The code-quality constitution — load whenever designing, implementing, changing, or reviewing code in any language, and when unsure whether a coding task qualifies. Two co-equal gates: Gate A (simplicity/readability) and Gate B (validation/testing). A pure question about existing or hypothetical code may not need it.
---

# Engineering Standards

The standard every coding and review task obeys. Code is not "done" until it passes **both** gates.

**Gate precedence: when Gate A and Gate B conflict, Gate B wins — style serves correctness, never the reverse.**

## Gate A — Simplicity

Code is read by developers of any level and must pass human review. A junior should follow it without chasing definitions. This is a gate: code that fails it isn't done.

1. **Idiomatic first.** Use the language's idiomatic solution whenever one applies.
2. **Aim for both — don't trade one away.** Do not write slow code or plan to lower performance for the sake of readability — instead, aim to write performant code in a way that is readable and understandable. When density genuinely buys performance, repay it with smaller blocks and more comments; density is the dial.
3. **Names state what a thing is and does.** Readers should be able to discern behavior from names without having to look up definitions. Prefer descriptive over clever or abbreviated.
4. **Decompose complex work.** Function/method bodies ~40 lines — a norm, not a limit; overrun is a signal to reconsider.
5. **Structure as visible steps.** Group logic into small blocks (~4–15 lines), one step each, separated by a blank line that marks the next step.
6. **Comment every block; scale with complexity.** Default: one full-line comment above each block, narrating the step (e.g. *"trim spaces and test the query against the sanitization regex"*) — situational to adjust. Obvious block → that one line. Complex operation (external libraries, logic-heavy calls) → more, up to one comment per line. Narrate, don't lecture; cut any comment that only restates obvious code.
7. **Treat errors as expected, not exceptional.** Anticipate failure at every operation that can fail. Handle errors and edge cases gracefully, log them with enough context to diagnose, and never silently discard an error.

## Gate B — Validation

"Done" means simple *and* validated. Unvalidated code is not done, however clean it reads. Rigor scales with blast radius.

1. **Define "done" before starting; refuse to guess it.** State up front what validation this task requires — which tests, checks, and observed behaviors must pass.
2. **Scale rigor to blast radius.** Reversible, local change → minimal checks. Irreversible, architectural, or security-touching → full pipeline. Same axis as the escalation triage.
3. **Prefer durable automated tests as the default.** Write tests alongside any new feature unless told otherwise, and when scaffolding a new project, stand up its test infrastructure as part of that work. Tests are cheap for agents and pay back as regression safety and cheaper verification; a throwaway spike or pure config rarely needs them. **Exception** — a small change to an existing project with no test infrastructure: don't stand up a framework for it; note the coverage gap, rely on `verify`, and suggest setting up test infrastructure at most once. Honor a project's stated decision to omit automated tests, and simply note the gap.
4. **Test the failure paths, not just the happy path.** Cover edge cases and the error handling from Gate A rule 7. A feature untested for how it fails is not validated.
5. **Run gates in order; a red gate stops progress.** Format → lint/vet → compile/type-check → tests → review. Do not proceed past a failing gate.
6. **Observe, don't assume.** Confirm the change works by running it, not by reasoning that it should. Success requires observed evidence.
7. **Leave evidence a reviewer can trust; never mark done on a red gate or an unverified claim.** Record what was run and its result so a human needn't rerun it to believe it.
