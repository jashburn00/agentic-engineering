---
name: triage
description: Classify a task or decision by reversibility and blast radius before acting — decide whether to proceed autonomously or escalate to the human, and set the validation rigor tier. Use at the start of a coding task and before any consequential mid-task decision.
---

# Triage

Before acting on a task or a consequential decision, classify it on two axes, then pick a lane and a rigor tier.

## Two axes

- **Reversibility** — how cheaply can this be undone? Cheap: a change under version control. Costly or impossible: deleted data, force-push, a released artifact, spent money, a sent message.
- **Blast radius** — how far do consequences reach? Small: one local function. Large: a public API, a shared schema or data model, auth/security, prod config, many callers, an added dependency.

## Lanes

**Proceed autonomously** when the change is reversible *and* local — e.g. an internal refactor of one function, adding a test, renaming a local symbol, fixing a typo or comment. Version control covers you.

**Escalate to the human** when the action is hard to reverse, wide-reaching, or ambiguous:
- **Irreversible / costly:** data deletion or migration, force-push, dropping schema, deleting resources, spending money, publishing or releasing, sending external communication.
- **Wide blast radius:** public API or interface changes, shared data model or schema, auth or security boundaries, dependency add/upgrade, prod config, breaking changes across many callers.
- **Ambiguous:** unclear requirements, multiple valid interpretations with divergent outcomes, or architectural choices with long-term lock-in.
- Anything outward-facing or leaving the machine.

When unsure which lane applies, escalate.

## On escalate — don't just stop

Present the decision crisply: what is being decided, the viable options, your recommendation with reasoning, and the risk or cost. For design-level decisions, resolve each open branch with the human (`planning-lite` by default, or `planning` for high-stakes or ambiguous work). Batch related decisions into one ask rather than interrupting repeatedly.

## Set the rigor tier (how much of the `validate` pipeline to run)

Two tiers:
- **Minimal** — reversible + local (typo, comment, one-line internal fix): format, lint/vet, and a targeted test.
- **Complete** — anything wider-reaching or irreversible: run the complete `validate` pipeline with evidence, producing well-tested and documented code. This is the default — agents are fast, so thorough testing and documentation cost little. For the highest-stakes changes, add extra scrutiny (adversarial edge cases, a second review pass) on top of the complete pipeline.

Rigor scales with blast radius and never drops below the tier the change warrants; when unsure, choose Complete.
