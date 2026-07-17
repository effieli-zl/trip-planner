# Roadmap & Project Management

Internal planning doc. Tracks phases, milestones, and the ordered dev steps for
each phase. The [PRD](../../PRD.md) is the source of truth for *what* we build;
this doc tracks *when* and *in what order*.

## Phases

| Phase | Goal | Status |
|-------|------|--------|
| **Phase 0 — AI Skill** | Validate the core loop (plan → structured itinerary → export) as a lightweight, file-backed AI skill. No backend, no app. | Planning |
| **MVP** | Full web + mobile product per PRD §7 (itinerary, map, AI agent, collaboration, sharing, journaling, blog generation, offline). | Not started |
| **Phase 2** | Booking assist, real-time collab, social content, community, budget tracking (PRD §7). | Not started |

## Phase 0 — dev steps

Each step gets its own detail doc under [`../development/`](../development/)
following [`TEMPLATE.md`](../development/TEMPLATE.md). Steps are added here as they
are scoped, and linked from the [development index](../development/README.md).

_(Scope agreed so far: itinerary builder + IDP auto-fill + HTML/ICS export.
Detailed step docs to be written as each step is picked up.)_

| # | Step | Detail doc | Status |
|---|------|-----------|--------|
| _tbd_ | Itinerary data model & builder | _tbd_ | Not started |
| _tbd_ | IDP auto-fill (booking confirmations) | _tbd_ | Not started |
| _tbd_ | Export (HTML / ICS) | _tbd_ | Not started |

## Milestones

- **M0** — Documentation framework in place _(this commit)_.
- **M1** — Phase 0 scope locked, dev steps written.
- **M2** — Phase 0 skill usable end-to-end (plan a trip, import a booking, export).

## Decision log

Record notable product/technical decisions here (date, decision, rationale).

- _2026-07-17_ — Build a Phase 0 "AI skill" version before the full app, to
  de-risk the itinerary data model.
- _2026-07-17_ — Keep [PRD.md](../../PRD.md) as-is for now; do not restructure it.
