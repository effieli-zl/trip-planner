# QA — Test Strategy

Internal quality doc. Defines how we verify the build before it reaches user
acceptance ([UAT](../uat/uat-plan.md)).

## Scope

QA covers correctness of the build against the design in each
[dev step](../development/README.md). Each step's own **Verification** section is
the starting point; QA turns those into repeatable checks here.

## Levels

- **Unit / script checks** — individual scripts and functions behave per spec
  (e.g. schema validation accepts valid trips and rejects malformed ones).
- **Integration** — a trip flows through the whole pipeline (build → import →
  export) and produces valid, consistent output.
- **Regression** — a fixed sample trip round-trips cleanly after every change.

## Test data

- Maintain at least one realistic sample trip (multi-day, multi-timezone,
  multi-contributor) used across checks. Location: _tbd (e.g. `trips/…-demo/`)_.

## Checklists

Per-phase QA checklists are added here as steps land.

### Phase 0

- [ ] _tbd — filled in as itinerary / IDP / export steps are built._

## Defect tracking

Record notable bugs found in QA, their fix, and the regression check added to
prevent recurrence.
