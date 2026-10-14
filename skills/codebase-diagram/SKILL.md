---
name: codebase-diagram
description: Produce a standalone, static HTML page that visually represents a project's architecture, zooming in C4-style — a summary, a context diagram, a container diagram, a component diagram, then optional views such as data shape, key flows, and a module map — saved beside the codebase map. Use when a human asks to visualize, diagram, or "draw" a codebase's architecture or flow, or accepts orient's offer of a visual page. Also use to update the page when a change alters project structure, but only if the page (e.g. `docs/codebase-map.html`) already exists; if it doesn't, don't load this skill for code changes.
---

# Codebase Diagram

A single self-contained HTML file that shows a project's architecture at a glance, zooming from the system's surroundings down to its main components. It is the visual companion to the codebase map (`codebase-map`) and the orient briefing (`orient`). It is a picture of the structure, not a replacement for reading the code.

## Where it lives

- Beside the codebase map: `docs/codebase-map.html` next to `docs/codebase-map.md`. If the project keeps its own architecture doc instead (e.g. `ARCHITECTURE.md`), put the page beside that, using the same base name.
- Create it only when a human asks for it. Never create it on your own initiative.

## Source material

Build it from what is already distilled: the codebase map if one exists, otherwise the current orient overview. Sweep the tree yourself, in a subagent, only when neither exists. If the map is stale, fix the map first (per `codebase-map`'s fix-on-read rule) and draw from the corrected version, so the two never disagree.

## What it shows — coarse to fine

The page follows the C4 model loosely: each diagram zooms one level further in and adds one level of detail. Use C4's levels, not its full notation.

1. **Summary** — the project name, two or three sentences on what it is and who or what uses it, and the commit (short hash) and date the page reflects.
2. **Context** — the system as one highest-contrast box (solid dark in light mode, inverted in dark mode), with the people and external systems around it. Names only. Each line gets a short verb phrase ("request search results").
3. **Container** — the system boundary opened up: the deployable pieces inside it (services, workers, UIs, databases, caches, queues). Each box gets a name and a description of one or two lines. For a single-process project, merge this into the context diagram rather than drawing one box twice.
4. **Component** — zoom into the container that does the core work. Draw its components *inside* its box, stacked in layers by call direction. Use mostly names, with the tech in the name ("Router layer (Gin)"). Add a second component diagram only when another container is equally central.
5. **Optional views** — add each one only when the project has the thing it shows:
   - **Data shape** — when there is a real datastore: cylinders that show the tables with their key fields, or the cache key patterns.
   - **Blueprint** — when there is a real API surface: one column per container, listing its endpoints, controllers or handlers, and services as short bullets. It is denser, so it goes after the diagrams.
   - **Key flows** — one or two flows a newcomer most needs. Draw each flow as numbered markers (①②③) on a copy of the component diagram. Use a sequence diagram only when timing or async hand-off is the point of the flow.
   - **Module map** — a table of the top-level directories, each with its responsibility and where to add things.

Keep the same altitude as the codebase map: leave out function signatures, line numbers, and exhaustive file lists. Mark anything inferred rather than confirmed as `unverified` on the page itself.

## Visual style

The goal is a diagram the reader understands from its shape before reading a word. `reference.svg` in this skill's folder shows every rule below. Match its look.

- **Layout shows structure.** Callers sit above what they call, so calls run top to bottom. Callers of the system go on the left or top, and datastores on the right or bottom. Never arrange boxes in an even grid, and never let lines cross.
- **Containment instead of arrows.** Draw components inside their container's shaded box, and containers inside the system boundary. External systems sit outside the boundary. Use a dotted box for a logical group (e.g. "Service layer").
- **Shapes carry meaning.** Draw people as a person figure and datastores, caches, and queues as cylinders. Code is a plain rectangle. At the context level, the system is the highest-contrast box: solid dark in light mode, light in dark mode.
- **Few words.** Label every line that crosses a container or system boundary, or whose meaning isn't obvious. A plain call down a layered stack inside a container may go unlabelled, since the layering already says "calls". Write each label on a single line along the line, in about 3–5 words. Leave out `[type: tech]` tags. Put the tech in the name or the description instead.
- **One zoom level per figure.** Give each figure a large uppercase title (CONTEXT, CONTAINER, COMPONENT), plenty of white space, and a size that fits one screen. Use grayscale only. Keep the text beside each figure to a caption, since the diagrams are the content.
- **Legend: at most two items**, and only for something the code really has. The usual case is interface vs implementation, drawn as one small notch at the callee end of each internal dependency line: hollow if the caller depends on an interface, filled if on a concrete type. Without a real reason for one, leave the legend out.

Before finishing, check each figure: no overlapping labels, no crossing lines, each figure fits one screen, every box is labelled, and so is every line that crosses a boundary or isn't obvious.

## How it's built

- **One static file.** All CSS goes inline. Diagrams are inline SVG that you write by hand. Use no JavaScript, no CDN or other network dependency, and no external fonts or images, so the page opens offline, renders the same way for everyone, and diffs readably.
- **Readable in both themes** — define colors as CSS custom properties and switch them with `prefers-color-scheme`. Keep the grayscale contrast high in both themes.
- **Agent-agnostic** — the output is a plain local file. Don't depend on any agent-specific publishing or rendering tool.
- **Interactive only on request.** Add interactivity (click-to-expand modules, flow switching, JS) only when a human explicitly asks for it.

## Keeping it current

- **Once the page exists, it has the same update obligation as the map.** When a change alters structure (a new module, a moved boundary, a changed entry point or key flow), update the page in the same change and refresh its commit stamp. A change that doesn't touch structure doesn't need an update.
- **If the page doesn't exist, skip this step.** Don't create it as part of a code change, and don't ask the human about it.
- **Fix-on-read** — if you open the page and it is wrong, correct it, or flag the drift if fixing it is out of scope.

## Keep it honest

Its value is being quick to take in. Every shape, line, and label should explain itself. If a diagram needs more than the two-item legend, or more than a few words on a line, it's too detailed: cut it back to structure and send the reader to the code for specifics.
