---
name: review
description: Review a code change (a diff or PR) for correctness and quality against the engineering-standards constitution, and report findings. The judgment lens — it targets bugs, edge cases, clarity, and design that formatters, linters, and tests cannot catch. Standalone and reusable; use to review a change before it lands, or as the review gate of a validation pipeline.
---

# Review

Read the change — the diff, plus enough surrounding code to understand intent — and judge it against the `engineering-standards` constitution (and, for UI changes, `frontend-practices`). Report findings; do not rewrite the code unless asked.

Review targets what mechanical gates cannot: logic, edge cases, design, and clarity of intent. Do not re-flag what a formatter or linter already enforces, and do not duplicate security review (a separate lens).

## What to look for

**Correctness** — first priority.
- Logic errors, off-by-one, wrong conditions, mistaken assumptions.
- Unhandled edge cases and failure paths; missing nil/empty/boundary handling.
- Error handling per Gate A rule 7 — anticipated, not swallowed; logged with context.
- Concurrency hazards, resource leaks, unclosed handles.
- Does the change actually do what the task intended?

**Simplicity & readability** (Gate A).
- Names a reader can follow without chasing definitions.
- Small, single-purpose blocks separated as visible steps; function bodies near the ~40-line norm.
- Comments that narrate each step, scaled to complexity — neither missing nor noise.
- Idiomatic for the language; performant without sacrificing clarity.

**Tests & maintainability** (Gate B, non-mechanical parts).
- Changed behavior has tests covering happy and failure paths — flag gaps, don't write them here.
- Fits existing patterns; no needless duplication, dead code, or leaked complexity.
- Designed in a way that allows future engineering work (scaling, extending, changing) to be idiomatic and sensible.

## Calibrate

- Prefer material, high-confidence findings over nitpicks. Silence is fine when the code is sound.
- State confidence when unsure; do not invent problems to look thorough.
- Honor Gate precedence: correctness (Gate B) outranks style (Gate A).

## Report

Emit findings, each with:
- `dimension` — correctness | readability | tests | maintainability.
- `severity` — blocker | warning.
- `location` — file:line or scope.
- `summary` — what is wrong, briefly.
- `suggestion` — how to address it (options if it is a judgment call).

Close with a one-line verdict: **approve** (no blockers) or **changes-requested** (list the blockers). Never approve past a real correctness blocker.

(As `validate`'s review gate, these findings are judgment-kind under gate `review`; `validate`'s own verdict subsumes the one above.)
