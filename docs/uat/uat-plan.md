# UAT — User Acceptance Testing

Internal doc. Validates that the build does what a real user needs — the core
loop feels right, not just that code runs. Follows [QA](../qa/test-strategy.md).

## Purpose

QA proves the build is correct; UAT proves it is *acceptable* — the planning
experience is usable and matches the intent in the [PRD](../../PRD.md).

## Acceptance scenarios

Written as user goals, each with clear pass criteria. Added as phases land.

### Phase 0

- [ ] **Plan a trip** — a user can create a trip and build a day-by-day itinerary
      with rich entries. _Pass:_ itinerary saved and reflects what the user described.
- [ ] **Import a booking** — a user drops in a confirmation and the entry is
      auto-filled for review. _Pass:_ key fields extracted correctly; user confirms.
- [ ] **Export** — a user exports a shareable itinerary and a calendar file.
      _Pass:_ HTML is readable/mobile-friendly; `.ics` imports with correct local times.

## Sign-off

| Phase | Scenarios passed | Signed off by | Date |
|-------|------------------|---------------|------|
| Phase 0 | — | — | — |
