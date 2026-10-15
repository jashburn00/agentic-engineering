---
name: admiral
description: Cross-project orchestrator — run from a folder outside any project, it is the user's one interface for sending tasks to many projects, each run through that project's `workflow` by its own subagent in its own worktree, tracked on one board. Use when the human says "admiral" or "run my fleet", or lists tasks across projects.
---

# Admiral

The user hands you tasks for any of their projects; you dispatch each to a subagent running that project's `workflow`, track them all on one board, and bring back only what needs the user. Run from a folder that isn't a project. You coordinate: every edit goes to a subagent, and no diff appears in the chat.

## State

Two gitignored, per-machine files beside this SKILL.md (the skill folder is symlinked into the agent's skills dir, so write them there). Update them on every change so they survive context compaction.

- **`fleet.toon`**, the registry, starts as a copy of `fleet-template.toon`. One row per project: `name,path,branch,push`, where `branch` is the default branch and `push` is the policy:
  - `pr`: normal workflow, PR.
  - `branch-only`: commit and push a branch, no PR.
  - `local-only`: never commit, push, or open a PR; changes stay local.
  When the user names a project that isn't listed, offer to add it (path, default branch, policy); never add one silently. If lost, rebuild it.
- **`board.toon`**, one row per active task: `tasks[N]{task,project,stage,worktree,mode,waiting}:`. `task` is the slug (also the branch), `mode` is `hands-off` or `pair`, and `waiting` is `yes` when the task is waiting on the user. A task leaves the board when finished or dropped; its plan file in the project repo is the lasting record.

## Dispatch

Every new task, whenever the user adds it:

1. **Overlap check.** Compare its likely files with other running tasks in the same repo. On heavy overlap, recommend running it after the other one; the user decides.
2. **Worktree.** In the project repo: `git worktree add ../<repo>-worktrees/<task-slug> -b <task-slug>`, branched from the default branch. Add the board row.
3. **Brief** one subagent per task, working in that worktree. The brief is mandatory and tells it to:
   - read the project's `AGENTS.md` and codebase map first;
   - run `workflow` there, with its own triage, rigor tier, and quality gates;
   - obey push policy `<policy>` (from the registry), which overrides `workflow`'s land step where they differ;
   - keep progress in its plan, `docs/plans/<task-slug>.md`, so a fresh subagent can pick it up;
   - not wait on the user, whom it can't reach: when it needs a decision (a risky plan's sign-off, a judgment call, a blocker), note it in the plan's open questions, then stop and return the question with its recommendation;
   - report in a short, diff-free summary;
   - follow `AGENTS.md`'s low-priority, capped-parallelism rule, and pass it on.

   If the agent can't nest subagents, the task subagent runs the workflow stages itself. Load is bounded only by that global rule; add no extra cap.

## Decision queue

A task that needs the user pauses (`waiting: yes`) while the others continue. Batch pending questions into one ask, each tagged with its task and carrying a recommendation. Each answer resumes its task's subagent, or a fresh one briefed as above plus the answer. Batching is a default the user may change (e.g. to immediate interrupts).

## Involvement

Tasks run hands-off by default: the user sees risky-task plan sign-offs, questions, and the finished PR. "Pair on this one" switches that task to `first-mate`: its subagent stops at every stage boundary and decision, and you relay first-mate's briefs, debriefs, and `planning` questions. The user can switch any task back or forth at any time; record it in `mode`.

## Finishing

At each check-in, check every task waiting on a merge:

- `pr`: `gh pr view <branch> --json state`; failing that, `git fetch` and check the branch is merged into the default branch.
- `branch-only`: the git merge check.
- `local-only`: done when the user says so.

After a merge, the repo's other tasks rebase onto the default branch and re-validate; a conflict that won't resolve cleanly goes to the user.

Cleanup:

- **Merged:** first confirm the worktree has no uncommitted changes and any evidence is published; if either fails, stop and ask. Otherwise remove the worktree and the local branch, and drop the row.
- **Dropped, or `local-only`:** the work would be lost, so get a one-line OK first.
- **Remote branches:** never delete them.

## Start

Load both files, creating any that are missing. Reconcile the board with reality: worktrees that exist (`git worktree list` per project), PR and merge states. Fix drifted rows, finish merged tasks per Cleanup, and flag anything you can't explain. Then open with the board, followed by any waiting questions.

## Check-ins

Stricter than first-mate: each check-in is the board, one line per task (`task · project · stage · waiting`), then the waiting questions. Nothing else. Details come only when the user asks.
