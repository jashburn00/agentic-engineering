# agentic-engineering

A suite of AI skills for agentic software engineering — a repeatable, comprehensive workflow that gets the most out of coding agents: high-quality, maintainable, human-reviewable output with minimal human time and effort.

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

The repo is the source of truth. Installing it means symlinking two things into your **user-level** `~/.claude/` — the global system prompt and every skill — so they are active in **every** session on the machine, in any directory. You never launch work from inside this repo; the toolbox travels to each project you open.

**Claude Code:**

```sh
# 1. Global system prompt. Claude Code auto-loads ~/.claude/CLAUDE.md in every
#    session; symlinking it to AGENTS.md makes the agent-agnostic system prompt
#    active machine-wide. (Non-Claude agents read AGENTS.md directly.)
mkdir -p ~/.claude
ln -sfn "$PWD/AGENTS.md" ~/.claude/CLAUDE.md

# 2. All skills. Link every skill directory into ~/.claude/skills. Re-run this
#    after adding a new skill — it is idempotent.
mkdir -p ~/.claude/skills
for skill in "$PWD"/skills/*/; do
  ln -sfn "$skill" ~/.claude/skills/"$(basename "$skill")"
done
```

**Other agents:** point that agent's instruction entrypoint at `AGENTS.md` (a symlink, or the agent's native `AGENTS.md` discovery), and link the `skills/` directories into wherever that agent loads skills from.

Clone this repo and re-run the commands on any machine to carry the whole workflow with you.
