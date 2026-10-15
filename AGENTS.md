When designing, implementing, changing, or reviewing code, apply the `engineering-standards` skill (source: this repo, `skills/engineering-standards/SKILL.md`) — when in doubt, apply it. A pure question about existing or hypothetical code may not need it.

Do not add yourself as a co-author or include Co-Authored-By trailers in git commits. Commits are authored by the human engineers who decide which work to do and ultimately own the output and its consequences.

Prefer TOON (Token-Oriented Object Notation) over JSON for structured data wherever applicable — it encodes the same data losslessly in fewer tokens.

Give the shortest reply that does the job: lead with the answer or decision, prefer short bullets over paragraphs, and don't recap what the human already knows or your own process.

A project's context lives in `AGENTS.md` at its repo root (with `CLAUDE.md` as a compatibility symlink). Read it before working in a project; when creating project context docs, make `AGENTS.md` canonical and symlink `CLAUDE.md` to it.

Keep a high-level codebase map (the `codebase-map` skill) that the project's `AGENTS.md` points to; read it to orient before working, and update it when a change alters the project's structure.

When entering an unfamiliar codebase or asked to explore one, use the `orient` skill.

Never let agent work hog resources or make the human's computer unresponsive — they may be screen-sharing or performing other work. This applies to every agent and subagent, in every project; pass it on when delegating.
- Run every build, test, lint, compile, or install command at low priority with capped parallelism.
- Ask before heavy work: race detectors or sanitizers, repeated runs (`-count`, `--repeat`, shell loops), full-repo builds or suites, benchmarks, container/image builds, or several of these back to back. Always wait for an OK in these scenarios. If you were instructed to work without supervision and shouldn't wait for approval, then perform the work in a way that best adheres to the above instructions.
