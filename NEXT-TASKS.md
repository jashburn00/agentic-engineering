# Next tasks

- ~~**Per-feature spec from planning.**~~ Done 2026-10-14 (`5179176`): every task writes a plan from `skills/workflow/plan-template.md`.
- **Workflow run logs.** Log each agentic workflow run so it can be reviewed afterwards, in a subfolder of `docs/` (e.g. `docs/logs/`). Include tool calls, subagent spawns, timestamps, time spent per task and subtask, and more. Open questions: agent-agnostic capture (hooks vs agent self-reporting), format (TOON?), size and retention, and whether logs are committed.
  - Pinned 2026-10-14, to revisit if a strategy emerges. Draft idea: hooks capture raw metadata events (time, agent, tool, short target, duration) to a gitignored `docs/logs/<slug>/events.jsonl`, with no outputs or contents; at the land step a `run-log` skill writes a committed `docs/logs/<slug>.md` summary (stage timeline, subagents, counts, validation, links). Agents without hooks write stage markers themselves. Note: Claude Code already keeps full JSONL transcripts in ~/.claude/projects/.
- ~~**Review hoopfeed-map v2.**~~ Done 2026-10-14: v2 promoted to `bet365interview/notes/hoopfeed-map.html`.
- **Design `admiral`.** Built 2026-10-14 (uncommitted): `skills/admiral/` (SKILL.md, fleet-template.toon; fleet.toon and board.toon gitignored); trial still pending. A cross-project orchestrator run from outside any repo: one interface to all of the user's projects, sending tasks to each project's workflow. Move the parallel material (worktrees, decision queue, task board, overlap check) out of first-mate into it, and return first-mate to a pairing mode for one task, which admiral can use per task. Then trial it on real tasks (one cross-project pair, one same-repo pair).
  - Captain's decisions carried over from first-mate (2026-10-13), verbatim from the removed section:
    The captain may run several tasks at once, across projects or in one repo, and may add one at any time; each new task goes through triage. You stay the single coordinator across all of them.

    - **One subagent per task**, each in its own worktree on its own branch named after the task: `git worktree add ../<repo>-worktrees/<task-slug> -b <task-slug>` (a sibling of the repo). Each task keeps its own triage, rigor tier, and quality gates, and lands as its own PR. Load stays bounded by the global low-priority, capped-parallelism rule in `AGENTS.md`; add no extra cap.
    - **Overlap.** Before starting a task, check its likely files against tasks already running in the same repo. If they overlap heavily, recommend running it after the other finishes; the captain decides. After a merge, the repo's other tasks rebase and re-validate; a conflict that won't resolve cleanly goes to the captain.
    - **Decision queue.** A task that needs the captain pauses; the others continue. Batch pending decisions into one ask, each tagged with its task and carrying your recommendation; each answer unblocks its own task. This is a default the captain may change (e.g. to immediate interrupts).
    - **Board.** With 2+ tasks active, open each check-in with one line per task: `task · project · stage · needs captain?`. Persist it to a small file outside the repos (your scratch/state dir) so it survives context compaction.
    - **Cleanup.** After the captain merges or drops a task, remove its worktree and branch only after a one-line OK in the decision queue.
- **Revisit if needed:**
  - Decision-queue batching vs immediate interrupts.
  - A soft line cap on first-mate updates (deferred).
  - A concurrency cap beyond the global rule (deferred).
