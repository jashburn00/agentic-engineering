---
name: codebase-map
description: A persistent, high-level map of a project's structure so agents orient in seconds instead of re-deriving the layout every session. Defines what the map holds, where it lives, when to read it, and how to keep it from going stale. Load when starting work in a project you don't already hold in context, or when a change alters its structure.
---

# Codebase Map

A codebase map is a durable, high-level document of a project's structure, checked into the repo. Its purpose is orientation: an agent reads it to learn where things live and how the project fits together, instead of re-exploring the tree and re-deriving conventions every session — saving time and tokens. It is a hint that speeds navigation, not a substitute for reading the code.

## Where it lives

- Default path: `docs/codebase-map.md`. If the project already keeps an architecture doc (e.g. `ARCHITECTURE.md`), use that instead of adding a second one.
- The project's `AGENTS.md` points to it in one line, so every agent discovers it. The map itself is loaded on demand, keeping the always-loaded `AGENTS.md` small.

## What it contains — high-level and stable only

Record only what changes rarely, so the map rarely goes stale:

- **Module/directory map** — the top-level layout and what each directory is responsible for.
- **Entry points** — where execution starts (main, server bootstrap, CLI, request handlers, jobs).
- **Where things live** — where to add a feature, a test, a config, a migration; the project's conventions for placement and naming.
- **Architecture and data flow** — the major components, how they talk, and the key boundaries and invariants.
- **Build / test / run** — the canonical commands, if they aren't already obvious from `AGENTS.md`.

Deliberately leave out volatile detail — function signatures, line numbers, exhaustive file listings. That detail belongs in the code; putting it in the map guarantees drift. When in doubt, keep it coarse.

## When to read it

Read the map to orient before planning or implementing in a project you don't already hold in context. Trust it for navigation, but verify any specific claim against the code before you depend on it — treat it as a map, not the territory.

## When to update it

- **At land, when the change altered structure** — a new module, a moved boundary, a changed entry point, a new placement convention. Update the map as part of the change so it lands in the same PR. A change that doesn't touch structure (a bug fix, a typo, an internal tweak) needs no map update.
- **Fix-on-read** — whenever you read the map and find it wrong or stale, correct it then; if the fix is out of scope, flag the drift. A map that lies is worse than none, so keeping it honest is part of every task that touches it.

## Bootstrapping

If no map exists and you're doing structural work, create one at the default path and point `AGENTS.md` at it — scaled to stakes: don't block a one-line fix to document the whole system. If a full map is warranted but out of scope, note the gap rather than writing it half-heartedly.

## Keep it lean

The map earns its keep by being fast to read. Keep it concise, and write any structured data in TOON (`toon`). If it grows into a re-explanation of the code, it's too detailed — cut it back to structure.
