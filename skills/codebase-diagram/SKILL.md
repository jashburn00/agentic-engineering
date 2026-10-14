---
name: codebase-diagram
description: Produce a standalone, static HTML page that visually represents a project's architecture, zooming in C4-style — a summary, an architecture (context + container) diagram, a component diagram, then other details such as key flows and a module map — saved beside the codebase map. Use when a human asks to visualize, diagram, or "draw" a codebase's architecture or flow, or accepts orient's offer of a visual page. Also use to update the page when a change alters project structure, but only if the page (e.g. `docs/codebase-map.html`) already exists; if it doesn't, don't load this skill for code changes.
---

# Codebase Diagram

A single self-contained HTML file that shows a project's architecture at a glance, zooming from the system's surroundings down to its main components. It is the visual companion to the codebase map (`codebase-map`) and the orient briefing (`orient`). It is a picture of the structure, not a replacement for reading the code.

## Where it lives

- Beside the codebase map: `docs/codebase-map.html` next to `docs/codebase-map.md`. If the project keeps its own architecture doc instead (e.g. `ARCHITECTURE.md`), put the page beside that, using the same base name.
- Create it only when a human asks for it. Never create it on your own initiative.

## Source material

Build it from what is already distilled: the codebase map if one exists, otherwise the current orient overview. Sweep the tree yourself, in a subagent, only when neither exists. If the map is stale, fix the map first (per `codebase-map`'s fix-on-read rule) and draw from the corrected version, so the two never disagree.

## What it shows — coarse to fine

The page follows the C4 model loosely: each section zooms one level further in. Use C4's levels and labelling, not its full notation.

1. **Summary** — the project name, a brief description of what it is and who or what uses it, and the commit (short hash) and date the page reflects.
2. **Architecture diagram** — the C4 context and container levels in one picture. Draw the system boundary, the people and external systems around it, and the deployable pieces inside it (services, workers, CLIs, databases, queues). For a single-process project, this is the one box plus everything it talks to.
3. **Component diagram** — zoom into the container that does the core work and show its major internal components and how they connect. Add a second component diagram only when another container is equally central. Never draw one per container by default.
4. **Other details** — whatever else a newcomer needs, at your judgment. Prefer these when applicable:
   - **Key flows** — one or two flows that matter most (e.g. the request lifecycle, the data pipeline, a job run), drawn as sequence or flow diagrams. Pick the flows a newcomer most needs. Don't try to cover every path.
   - **Module map** — the top-level directories, each with its responsibility and entry points, and where to add things. It ties the diagrams to the folders in the repo.

Keep the same altitude as the codebase map: leave out function signatures, line numbers, and exhaustive file lists. Mark anything inferred rather than confirmed as `unverified` on the page itself.

## How it's built

- **One static file.** All CSS goes inline. Diagrams are inline SVG that you write by hand. Use no JavaScript, no CDN or other network dependency, and no external fonts or images, so the page opens offline, renders the same way for everyone, and diffs readably.
- **Readable in both themes** — define colors as CSS custom properties and switch them with `prefers-color-scheme`. Use high contrast and enough spacing that labels never overlap.
- **Agent-agnostic** — the output is a plain local file. Don't depend on any agent-specific publishing or rendering tool.
- **Interactive only on request.** Add interactivity (click-to-expand modules, flow switching, JS) only when a human explicitly asks for it.

## Keeping it current

- **Once the page exists, it has the same update obligation as the map.** When a change alters structure (a new module, a moved boundary, a changed entry point or key flow), update the page in the same change and refresh its commit stamp. A change that doesn't touch structure doesn't need an update.
- **If the page doesn't exist, skip this step.** Don't create it as part of a code change, and don't ask the human about it.
- **Fix-on-read** — if you open the page and it is wrong, correct it, or flag the drift if fixing it is out of scope.

## Keep it honest

Its value is being quick to take in. Every shape, line, and label should explain itself: name things directly on the diagram rather than encoding meaning in colors, line styles, or symbols. Label each box C4-style with its name, a `[type: tech]` tag (e.g. `[Container: Go]`, `[Database: Postgres]`), and a one-line responsibility. Label each arrow with what flows across it. If a diagram needs a legend at all, it's too detailed; cut it back to structure. Send the reader to the code for specifics.
