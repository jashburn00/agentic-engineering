---
name: orient
description: Bring a human up to speed on an unfamiliar or half-remembered codebase — produce a progressive, human-facing orientation briefing (what it is, how it's built, where things live, how to run it), led by a high-level overview you then steer into. Read-only; reads the codebase map if present and offers to create or refresh it. Use when entering a project cold ("orient me", "get me up to speed", "what is this repo").
---

# Orient

Bring a human up to speed on a project — a cold clone, or one they haven't touched in months. The outcome is a human-facing briefing delivered now, not a file: it leaves the human oriented and the repo untouched unless they opt in.

This is the human-facing companion to `codebase-map`. The map is the agent's durable, coarse structural doc; orient reads it when present and turns understanding into a briefing for a person — richer, narrative, and steerable.

## Outcome

A progressive briefing:

- **Lead with a tight overview** — a screenful, no more: what the project is and does, its stack, the main components and how they fit (architecture and data flow at a glance), the entry points, how to build/run/test, where things live (placement and naming conventions), and notable risks or gotchas.
- **Then let the human steer.** Stop, and let them drill into whatever matters next ("walk me through the request flow", "where would I add X", "how do I run it"). Expand on demand rather than front-loading everything.

By default, orient for general understanding. If the human names a goal ("I need to add auth"), anchor the tour toward the relevant modules and where they'd make the change.

## How it reads

- **Delegate the sweep.** Run the broad initial exploration — reading across the tree — in a subagent, or a few parallel explorers for a large repo, and return a distilled overview; keep the lead context lean. Handle the interactive drill-downs inline, with targeted reads.
- **Read-only.** Never run the build, the tests, or any code to orient unless the human asks. Infer from reading config and source.
- **Flag the unknowable.** State intent or behavior you can't confirm from reading as brief flags ("appears to…", "unverified:") rather than asserting it. No need to cite files as evidence — keep the briefing readable.

## The map

Read `docs/codebase-map.md` (or the project's architecture doc) if it exists, and use it to orient faster — applying fix-on-read if you find it stale. If no map exists, or it is stale, offer to create or refresh it per `codebase-map` (structured parts in `toon`) — but never write it without the human's go-ahead; orienting must leave an unfamiliar repo untouched by default.

## Keep it honest

Orientation is a hint that speeds navigation, not a substitute for reading the code. Pitch it high-level and send the human to the code for specifics.
