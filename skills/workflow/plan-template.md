# Plan template

Copy into `docs/plans/<task-slug>.md` (the folder stays flat). Size scales with the task: a non-escalated task writes a short version — Goal, Acceptance criteria, a one-slice Approach, Outcome, plus any Decisions the agent made alone; an escalated task fills every section.

Status moves `draft` → `approved` (human sign-off) → `pr-opened` (set at the land step, with Outcome and the PR link filled; that completes the task); non-escalated tasks skip `approved`. Never set a post-merge status.

---

# <Task title>

Status: draft | approved | pr-opened

## Goal
What is being built and why, in 1–2 sentences.

## Scope
In scope, plus non-goals.

## Acceptance criteria
Define done: the tests, checks, and observed behaviors that must pass.

## Decisions
Each with the options considered, the choice, the reason, and who decided (human or agent).

## Approach
An ordered list of vertical slices, the first being the thinnest path that works end to end.

## Risks / open questions
What could go wrong, and what is still unresolved.

## Outcome
Filled at land, once the PR is open: what differed from the plan, plus links to the PR and evidence.
