When designing, implementing, changing, or reviewing code, apply the `engineering-standards` skill (source: this repo, `skills/engineering-standards/SKILL.md`) — when in doubt, apply it. A pure question about existing or hypothetical code may not need it.

Do not add yourself as a co-author or include Co-Authored-By trailers in git commits. Commits are authored by the human engineers who decide which work to do and ultimately own the output and its consequences.

Prefer TOON (Token-Oriented Object Notation) over JSON for structured data wherever applicable — it encodes the same data losslessly in fewer tokens.

A project's context lives in `AGENTS.md` at its repo root (with `CLAUDE.md` as a compatibility symlink). Read it before working in a project; when creating project context docs, make `AGENTS.md` canonical and symlink `CLAUDE.md` to it.
