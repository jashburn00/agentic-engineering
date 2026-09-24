# agentic-engineering

A suite of AI skills for agentic software engineering — a repeatable, comprehensive workflow that gets the most out of coding agents: high-quality, maintainable, human-reviewable output with minimal human time and effort.

## Design principles

- **Agent-agnostic.** The system prompt lives in `AGENTS.md` (the cross-agent convention) and every skill is plain markdown, so the suite works with any coding agent, not just Claude Code.
- **Quality is a gate, not a hope.** Every change must clear two co-equal gates — simplicity/readability and validation/testing — with correctness winning ties. "Done" means simple *and* validated.
- **Evidence over claims.** Nothing is reported done on an unverified claim; gates and runtime checks produce evidence a reviewer can trust without re-running it.
- **Process proportional to stakes.** `triage` scales rigor to a change's reversibility and blast radius — a light path for trivial changes, the full pipeline for consequential ones.
- **Human-in-the-loop where it matters.** Agents run autonomously through the middle; the human is concentrated at planning (only when a change escalates) and the merge decision.
- **Minimal hot path.** Anything auto-loaded into every agent (`CLAUDE.md`/`AGENTS.md`) stays tiny; heavy content lives in skills that load on demand (progressive disclosure).
- **Token-lean authoring.** Skill text is written in the fewest tokens that preserve all intent.

## The workflow

One task, from request to a landed PR — each step invokes a dedicated skill, scaled to the change's stakes:

```
  task
  │
  ▼
  triage      ──▶  lane: autonomous | escalate · tier: Minimal | Complete
  │
  ▼
  define done ──▶  acceptance criteria, stated up front
  │
  ▼
  plan        ──▶  escalated tasks only · human sign-off · docs/plans/<task>.md
  │
  ▼
  implement   ──▶  under engineering-standards (Gate A)
  │
  ▼
  validate    ──▶  format → lint → build → test → review
  │
  ├──▶ security-review   when the change touches a security surface
  ├──▶ verify            when there is runtime behavior to observe
  │
  ▼
  land        ──▶  git-safety · feature branch · PR + evidence
  │
  ▼
  merge       ──▶  the human's call
                   Minimal → autonomous PR + notify
                   Complete / Escalated → review, then merge
```

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
# Symlink Claude Code's system-prompt location to this repo's AGENTS.md.
ln -sfn "$PWD/AGENTS.md" ~/.claude/CLAUDE.md

# Link every skill into ~/.claude/skills (idempotent — re-run after adding a skill).
mkdir -p ~/.claude/skills
for skill in "$PWD"/skills/*/; do
  ln -sfn "$skill" ~/.claude/skills/"$(basename "$skill")"
done
```

**Other agents:** point that agent's instruction entrypoint at `AGENTS.md` (a symlink, or the agent's native `AGENTS.md` discovery), and link the `skills/` directories into wherever that agent loads skills from.

Clone this repo and re-run the commands on any machine to carry the whole workflow with you.
