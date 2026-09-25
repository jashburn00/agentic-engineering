---
name: workflow
description: The end-to-end process for taking a coding task from request to a landed PR — triage, define done, plan, implement, validate, review, and land, scaled to the task's stakes. Use at the start of any non-trivial coding task; it sequences the other skills rather than replacing them.
---

# Workflow

The repeatable process for a coding task: request in → PR + evidence out. It routes — each step invokes a dedicated skill; this skill decides *what* runs and *how much*, scaled to the task's stakes. It does not restate what those skills do.

Process is proportional to stakes: a Minimal task takes a light path; a Full task runs everything. Never spend more process than the change warrants, or less.

Use subagents where sensible; the same agent running different tasks and having contextual knowledge of them can break the intended atomicity of steps.

## Steps

1. **Triage** — classify with `triage`: the lane (autonomous vs escalate) and the rigor tier (Minimal or Complete). Everything below scales to this.

2. **Define done** — before implementing, state the task's acceptance criteria: which tests, checks, and observed behaviors must pass (`engineering-standards` Gate B rule 1 — define done, don't guess it). For escalated tasks this lives in the plan; otherwise state it briefly up front.

3. **Plan** — *only when triage escalates.* Write a plan to `docs/plans/<task-slug>.md`, resolve open decisions with the human (`planning-lite` by default, or `planning` for high-stakes or ambiguous work), and get sign-off before coding. Non-escalated tasks skip this step and proceed.

4. **Implement** — make the change under `engineering-standards` (and, for a frontend/UI change, `frontend-practices`). Small, coherent diffs; write the tests for changed behavior here (Gate B); follow the plan when one exists.

5. **Validate** — run `validate` at the tier from step 1. Apply mechanical fixes; escalate judgment findings. `validate`'s review gate is the `review` skill — do not run `review` again separately.

6. **Conditional lenses** — invoke based on the change, not by default:
   - `security-review` when it touches a security surface (input, auth, data, secrets, external calls, new dependencies).
   - `verify` when there is runtime behavior worth observing (a fix, a feature, UI, integration); a visible UI change defaults to rendering and screenshotting the page.

7. **Land** — with `git-safety`: commit (repo conventions and any authorship rules), push a feature branch, and open a PR whose body carries the summary and evidence (validation results, findings, what was verified). Opening a PR is the normal reversible landing, not an escalation. Publish screenshots and other visual evidence to an orphan `evidence` branch — one folder per PR, disconnected from code history and never merged — and link them from the PR body; preserve them before any worktree cleanup. Then, by tier and lane:
   - **Minimal** — PR opened autonomously; report "done, PR #N."
   - **Complete, autonomous** — PR with summary; await the merge decision.
   - **Escalated** — request review before merge, surfacing the key decisions and risks.
   Merge is the gated act — leave it to the human or parent agent unless the project has opted into auto-merge for green Minimal PRs. On merge, move any plan to `docs/plans/completed/`.

## When blocked

If `validate` returns blocked or a lens finds a blocker, loop back to Implement, fix, and re-validate. If it cannot be resolved with reasonable effort, or a judgment call needs the human, escalate — do not force a pass.

If part of the pipeline could not run (a tool or permission is unavailable — build, tests, `verify`, or opening a PR), do not report done and do not fabricate results: complete every step you can, then report exactly what passed, what could not run and why, and escalate the blocked part.

## Done

A task is done when the acceptance criteria from step 2 are met: the tier's validation passed with no unresolved blockers, the required lenses are clear, escalations are resolved, and a PR is open with evidence. Never report done on a red gate, a gate that could not run, an unmet criterion, or an unverified claim.
