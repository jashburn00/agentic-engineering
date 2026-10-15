# Per-feature plan

Status: approved

## Goal
Give every task a plan file that records its scope, decisions, and definition of done, so high-scrutiny projects can review why each change was made, not only the code.

## Scope
- In scope: a plan for every task, written after triage; a seven-section template; who writes and runs the plan; the plan's status lifecycle; approval rules plus a project opt-in; vertical slices in the Implement step; matching README text.
- Non-goals: first-mate's parallel-task content (being redesigned separately); workflow run logs; changes to `engineering-standards`.

## Acceptance criteria
- `workflow` steps run triage → plan → implement → validate → lenses → land; "define done" is folded into the plan, and every step-number cross-reference (including Done) is correct.
- `skills/workflow/plan-template.md` lists the seven sections, a status line, and how size scales with the task; `workflow`, `planning`, and `planning-lite` point to it, and it loads only when writing a plan.
- Escalated tasks stop for plan sign-off; non-escalated tasks don't, unless the project's `AGENTS.md` carries the opt-in line.
- The land step fills Outcome (with the PR link) and sets status `pr-opened`; no post-merge status exists; no `docs/plans/completed/` rule remains.
- The Implement step requires vertical slices; `engineering-standards` is unchanged.
- The README diagram and text match.
- The captain's blind runs on cloned repos produce a plan per task that follows the template.

## Decisions
- **The plan file becomes the spec.** Options: a separate spec artifact, or the existing plan file. Choice: the plan file. Reason: one artifact per task holds scope and decisions without duplication. Who decided: human.
- **Every task writes one.** Options: escalated tasks only, scaled by tier, or every task. Choice: every task, with size scaled to the task. Reason: high-scrutiny projects need a record of every change; the short version keeps light tasks cheap. Who decided: human.
- **Seven sections.** Options: free-form, or fixed sections. Choice: Goal, Scope, Acceptance criteria, Decisions, Approach, Risks / open questions, Outcome. Reason: a fixed shape makes plans reviewable side by side, and acceptance criteria absorb the old "define done" step. Who decided: human.
- **Vertical slices in workflow's Implement step.** Options: `engineering-standards`, or `workflow`'s Implement step. Choice: the Implement step. Reason: slicing is process, not code quality; the constitution stays about code. Who decided: human.
- **Template at `skills/workflow/plan-template.md`, keeping the "plan" name.** Options: inline in `workflow`, inside a planning skill, or a separate file under `workflow`; "plan" or "spec". Choice: a separate file under `workflow`, named "plan". Reason: `workflow` owns the plan step and the three skills share one template; loading it only when writing a plan keeps the hot path small; "plan" is already the term across skills. Who decided: human.
- **A status line instead of `completed/`.** Options: move landed plans to `docs/plans/completed/`, or a status line in a flat folder. Choice: `Status: draft | approved | pr-opened`. Reason: paths and links stay stable, and the state is visible in the file. Who decided: human.
- **`pr-opened` as the final status.** Options: `landed` set when the PR opens, or `pr-opened`. Choice: `pr-opened`; the agent fills Outcome with the PR link, and that completes the task. Reason: a plan must never claim landed or merged before its PR merges, and merge is the human's act, so the agent never sets a post-merge status. Who decided: human.
- **The planning lens writes the plan; the lead runs it.** Options: a subagent runs planning, or the lead does. Choice: a planning lens always writes it (`planning-lite` by default, `planning` for high-stakes or ambiguous work and always under first-mate), run by the lead, since only the lead talks to the human; under first-mate the lead hands the file write to a subagent. Reason: questions reach the human directly, and the captain's screen stays free of diffs. Who decided: human.
- **Approval opt-in wording.** Options: always approve, never approve non-escalated plans, or a project opt-in. Choice: escalated plans stop for sign-off; others don't unless the project's `AGENTS.md` contains "Human approval required for all plans, including non-escalated tasks." Reason: the default stays low-touch, and high-scrutiny projects can require review of every plan. Who decided: human.
- **Validation.** Options: synthetic checks in this repo, or real runs. Choice: the captain validates with his own blind runs on cloned repos. Reason: real runs show whether agents follow the new step unprompted. Who decided: human.

## Approach
1. Template plus the `workflow` plan step: a task now produces a plan file end to end.
2. `planning` and `planning-lite` point to the template and state who runs them; first-mate notes that a subagent writes the file.
3. Lifecycle: the land step fills Outcome and sets `pr-opened`; drop the `completed/` rule; fix the Done cross-reference.
4. Vertical slices in the Implement step.
5. README diagram and text.
6. This plan, written from the template.

## Risks / open questions
- A plan for every task adds overhead to trivial ones; the short version is meant to keep it small.

## Outcome
