---
name: validate
description: Run a change through an ordered validation pipeline and report findings — format, lint/vet, build/type-check, tests, and review. Standalone and reusable — it reports what passed, what failed, and what needs a human, without deciding how much rigor a task deserves. Use to confirm a change is correct and complete before it lands.
---

# Validate

Run a change through the pipeline below, in order. Each gate passes or produces findings. Apply safe mechanical fixes automatically; flag judgment findings for the caller. The change is validated only when every gate is green.

This skill does not decide *whether* a change deserves validation or *how much* — the caller decides that and invokes it. Validate just validates.

## Pipeline

Run the gates in order. When a gate fails it raises a finding:
- a **blocker** stops the pipeline until it is resolved — fix a mechanical blocker and re-run; halt a judgment blocker for the caller;
- a **warning** is recorded and the pipeline continues.

1. **Format** — apply the language's canonical formatter.
2. **Lint / vet** — run the project's linters and static analysis.
3. **Build / type-check** — compile or type-check; it must succeed.
4. **Test** — run the existing suite; all green. Changed behavior that lacks coverage (happy or failure paths) is a finding — validate checks coverage; development writes the tests, not this step.
5. **Review** — read the diff for correctness, clarity, and edge cases, against the `engineering-standards` constitution when present.

## Findings

Every failure or concern becomes a finding with these fields:

- `gate` — which step raised it.
- `severity` — `blocker` or `warning`.
- `kind` — `mechanical` (safe, deterministic fix: formatting, import order, an obvious lint fix) or `judgment` (touches behavior or intent; needs a human call).
- `location` — file:line or scope.
- `summary` — what is wrong, briefly.
- `fix` — the applied fix (mechanical) or the proposed options (judgment).

Handle by kind:

- **mechanical** → apply the fix, then re-run the affected gate and any later gate the change could affect (in practice, from that gate onward); record it resolved.
- **judgment** → do not guess. Record it with options and leave it for the caller.

## Verdict

Report one verdict plus the findings record:

- **pass** — no unresolved blockers (warnings may remain, and are listed). Include evidence: the commands run and their results.
- **blocked** — any unresolved blocker: a red gate or a judgment finding awaiting a human. List them.

Never report pass on a red gate or an unverified claim. Report evidence a reviewer can trust without re-running it.
