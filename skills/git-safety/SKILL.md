---
name: git-safety
description: Operational safety for git — prevent leaking secrets, dangerous or destructive git operations, and pushes to the wrong target. Check before you commit, push, or rewrite history. Standalone and agent-agnostic; use whenever staging, committing, pushing, or changing git state.
---

# Git Safety

Operational safety around git: what to check before committing, pushing, or rewriting history. This guards the *act of using git*, distinct from code review (`review`) and application-security review (`security-review`).

Destructive or outward-facing git actions are `triage` escalations — this skill is the procedure for the ones you're cleared to run.

## Secret & sensitive-data hygiene

Before staging or committing, scan the diff for anything that must not enter history:
- Credentials, API keys, tokens, private keys, passwords, connection strings.
- Personal or machine-specific data: home paths (`/home/<user>`, `/Users/<user>`), internal hostnames, real user data.
- `.env` files, credential files, or large binaries not meant for the repo.

If found: remove it and don't commit until it's gone. A secret already committed must be treated as compromised — rotate it, don't just delete the line. Never write secrets into logs, error messages, or commit/PR text either.

## Safe git operations

- **Never rewrite shared history.** No force-push, rebase, reset, or amend on a branch others may have pulled, or on a protected/default branch. Escalate if a rewrite seems needed.
- **Prefer additive, reversible operations.** New commits over history edits; `--force-with-lease` over `--force` when a force is genuinely required and cleared.
- **Destructive commands are escalations**, not defaults: `push --force`, `reset --hard`, `clean -fd`, branch/tag deletion, `filter-branch`/`filter-repo`.
- **Confirm working state before switching or resetting** — don't discard uncommitted work.

## Correct routing

- **Push to the intended remote and branch.** Verify `remote -v` and the target before pushing; don't assume `origin`/`main`.
- **Respect the branch model** — branch off the project's documented working branch (check the project's `AGENTS.md` or git config); when undocumented, use a feature branch off the default. Never push directly to the default/protected branch unless explicitly told.
- **Match fork vs upstream** — push to your fork, open PRs against upstream; never push to a repo you don't own.

## Before you push — quick checklist

1. Diff is free of secrets and machine-specific data.
2. No unintended history rewrite.
3. Correct remote and branch.
4. Commit follows repo conventions (and any system prompt authorship rules).
