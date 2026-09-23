# agentic-engineering

A suite of AI skills for agentic software engineering — a repeatable, high-quality workflow that gets the most out of coding agents while producing senior-grade, maintainable, human-reviewable output.

## Design principles

- **Skills-first, orchestration-later.** Build portable quality skills on native Claude Code primitives (skills, subagents, hooks, `CLAUDE.md`) first; add a multi-tier orchestrator only when parallel cross-repo work justifies it.
- **Minimal hot path.** Anything auto-loaded into every agent (`CLAUDE.md`) stays tiny. Heavy content lives in skills, which load on demand (progressive disclosure).
- **Token-lean authoring.** Skill text is written in the fewest tokens that preserve all intent.
- **Reversibility-based autonomy.** Agents act autonomously on reversible/local changes; irreversible, architectural, or security decisions escalate to the human.

## Layout

```
skills/
└── engineering-standards/   The code-quality constitution (Gate A + Gate B).
```

More skills (triage, plan, implement, land, per-stack styles) land here next.

## Installation

The repo is the source of truth. Activate a skill by symlinking it into `~/.claude/skills/`:

```sh
ln -s "$PWD/skills/engineering-standards" ~/.claude/skills/engineering-standards
```

Clone this repo and repeat the symlink on any machine to carry the workflow with you.
