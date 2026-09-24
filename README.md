# agentic-engineering

A suite of AI skills for agentic software engineering — a repeatable, refined workflow that gets the most out of coding agents while producing high-quality, maintainable, human-reviewable output.

## Design principles

- **Skills-first, orchestration-later.** Build portable quality skills on native Claude Code primitives (skills, subagents, hooks, `CLAUDE.md`) first; add a multi-tier orchestrator only when parallel cross-repo work justifies it.
- **Minimal hot path.** Anything auto-loaded into every agent (`CLAUDE.md`) stays tiny. Heavy content lives in skills, which load on demand (progressive disclosure).
- **Token-lean authoring.** Skill text is written in the fewest tokens that preserve all intent.
- **Reversibility-based autonomy.** Agents act autonomously on reversible/local changes; irreversible, architectural, or security decisions escalate to the human.

## Layout

```
AGENTS.md                    Agent-agnostic system prompt (the entrypoint every agent reads).
skills/
├── engineering-standards/   The code-quality constitution (Gate A + Gate B).
├── workflow/                Conductor → sequences the skills below into one tier-proportional pipeline.
├── triage/                  Reversibility/blast-radius classifier → autonomy lane + rigor tier.
├── validate/                Standalone validation pipeline → findings + pass/blocked verdict.
├── review/                  Code-review judgment lens → correctness/quality findings + verdict.
├── security-review/         Application-security lens → vulnerability findings + severity + remediation.
├── verify/                  Runtime-observation lens → run the change, observe, report with evidence.
├── git-safety/              Operational git safety → secret hygiene, safe operations, correct routing.
└── toon/                    TOON syntax reference (cold-loaded) for the AGENTS.md TOON directive.
```

`AGENTS.md` follows the cross-agent convention, so the system prompt is not tied to any one tool. The constitution is plain markdown: Claude Code loads it as a skill (progressive disclosure); any other agent can read the file directly.

More skills (plan, implement, land, per-stack styles) land here next.

## Installation

The repo is the source of truth. Point each agent's instruction entrypoint at `AGENTS.md`, and activate skills into the agent's skill directory.

**Claude Code:**

```sh
ln -s "$PWD/AGENTS.md" ~/.claude/CLAUDE.md
ln -s "$PWD/skills/engineering-standards" ~/.claude/skills/engineering-standards
```

**Other agents:** either symlink that agent's instruction file to `AGENTS.md`, or rely on the agent's native `AGENTS.md` discovery.

Clone this repo and repeat the links on any machine to carry the workflow with you.
