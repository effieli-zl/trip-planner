# CLAUDE.md — Trip Planner

Central index and working guide for this repo. Read this first. It catalogs all
documentation, captures the high-level design, and lists the rules for working
here.

## What this is

Trip Planner is a personal and group travel tool. The **itinerary is the single
source of truth** — planning, collaboration, journaling, and content generation
all build on top of it. See [PRD.md](./PRD.md) for the full product spec.

**Current stage:** Greenfield. We are building a **Phase 0 "AI Skill"** version
first — a lightweight, file-backed planner that validates the core loop before
the full web + mobile product.

---

## Documentation catalog

Keep this catalog current. **When you add, move, or remove a `.md`, update this
list in the same change.**

### Customer-facing
- [README.md](./README.md) — public project summary. Marketing/user-facing tone only.

### Product
- [PRD.md](./PRD.md) — Product Requirements Document (v1). Source of truth for
  vision, users, feature modules F1–F9, and MVP vs. Phase 2 scope.

### Internal build docs (`docs/`)
- **Project management**
  - [docs/project-management/roadmap.md](./docs/project-management/roadmap.md) —
    phases, milestones, dev-step order, decision log.
- **Development**
  - [docs/development/README.md](./docs/development/README.md) — dev workflow +
    index of step docs.
  - [docs/development/TEMPLATE.md](./docs/development/TEMPLATE.md) — template every
    dev-step doc is copied from.
  - _Step docs (`docs/development/step-NN-*.md`) — one per dev step; listed here as
    they are created._
- **QA**
  - [docs/qa/test-strategy.md](./docs/qa/test-strategy.md) — test levels, test data,
    checklists, defect tracking.
- **UAT**
  - [docs/uat/uat-plan.md](./docs/uat/uat-plan.md) — acceptance scenarios and sign-off.

---

## High-level design & architecture

### Product architecture (target, per PRD)
- **Itinerary-centric data model** — one uniform entry record; UI reveals detail
  based on booking status (not a schema difference). Shared vs. personal entries.
- **Offline-first, CRDT-compatible** data layer from day one (PRD §6).
- **Clients:** web (planning/collaboration/content) + mobile (in-trip, offline).
  Cloud-first storage with local cache. Auth required from day one.
- **AI agent** with trip data in context: discovery, gap detection, practical flags,
  IDP auto-fill, content generation.

### Phase 0 architecture (what we build now)
- **File-backed, no backend.** A trip lives as structured JSON in the repo
  (e.g. `trips/<slug>/itinerary.json`), diff-able and schema-validatable. The JSON
  schema is the precursor to the real product data model.
- **Self-contained, stdlib-only scripts** for validation and export (HTML, `.ics`).
  No external dependencies, so it is portable and easy to run.
- **AI (this assistant) is the interface** — conversational itinerary building,
  IDP extraction (with user confirmation), and export.
- **MCP tools are optional enhancements**, not dependencies (e.g. Gmail to fetch a
  confirmation, Google Calendar to push events). The skill must work without them.
- **Deferred to later phases:** backend, mobile/offline, real-time collaboration/
  CRDT, sharing enforcement, community, blog/social content generation. The schema
  may *record* forward-looking fields (visibility, ownership) without implementing
  their behavior.

---

## Working guidelines (do / don't)

### Documentation
- **DO** keep [README.md](./README.md) strictly customer-facing (what the product
  is and does, in user language).
- **DON'T** put internal build details — tasks, test plans, architecture debates,
  TODOs — in README.md. Those go under `docs/`.
- **DO** keep [PRD.md](./PRD.md) as the product source of truth; change it
  deliberately and note the decision in the roadmap decision log.
- **DON'T** restructure the PRD without an explicit request.
- **DO** give every meaningful dev step its own doc in `docs/development/`, copied
  from [TEMPLATE.md](./docs/development/TEMPLATE.md).
- **DO** update the **catalog in this file** whenever docs are added/moved/removed,
  and keep the roadmap and development indexes in sync.
- **DON'T** duplicate content across docs — link instead. Each fact has one home.

### Building (Phase 0)
- **DO** treat the itinerary JSON schema as the single source of truth for the data
  model; validate trip files against it.
- **DO** keep Phase 0 scripts dependency-free (Python stdlib) and the skill runnable
  without any MCP connection.
- **DO** show IDP-extracted fields to the user for confirmation before writing them.
- **DON'T** pull Phase-0-out-of-scope work (backend, mobile, collaboration,
  community, content generation) into Phase 0.
- **DON'T** hardcode secrets, tokens, or personal data into the repo.

### Git
- **DO** develop on the designated feature branch; commit with clear messages;
  push when work is complete.
- **DON'T** open a pull request unless explicitly asked.
