# Travel Planning & Sharing App — PRD v1
_Status: Draft · 2026-07-04_

---

## 1. Product Vision

A personal travel planning tool that grows with you from the first search to the final post. The itinerary is the single source of truth — everything else (collaboration, journaling, content generation) builds on top of it.

**One-liner:** Plan trips, travel together, tell the story — all in one place.

**Why this exists:** Existing tools are either planning-only (no sharing, no content layer) or social-only (no serious planning). Nothing does the full loop well.

---

## 2. Target User

**Primary:** Individual traveler who plans seriously — researches destinations, tracks logistics, wants to remember and share the trip.

**Secondary:** Small groups (couples, friends, family) who co-plan and travel together.

**Unifying trait:** Wants a tool that doesn't make them rebuild everything from scratch to write about the trip afterward.

---

## 3. User Journey

```
[ PLANNING ]  →  [ IN-TRIP ]  →  [ POST-TRIP ]
  Research         Record          Generate
  Organize         Journal         Share
  Collaborate      Capture         Publish
```

### Phase 1: Planning
- Create a trip, add destinations
- Build itinerary: stops, dates, transportation, bookings, links
- AI agent assists: destination suggestions, logistics, flags issues
- Collaborate with travel companions (equal permissions)
- Map view: all stops visualized, route drawn

### Phase 2: In-Trip
- Mobile app, optimized for on-the-go
- Works offline — data syncs when connection returns
- Add journal entries tied to itinerary stops
- Capture photos, quick notes, spontaneous changes
- Real-time (or near-real-time) sync with collaborators

### Phase 3: Post-Trip
- Itinerary + journal entries + photos = raw material
- AI generates draft content: long-form blog post OR social media post
- User selects platform (小红书, Instagram, X, etc.) — content adapts to format/tone
- Edit, finalize, publish or export

---

## 4. Feature Modules

### F1 — Trip & Itinerary (Core)
**Priority: MVP**

The foundation. Everything else depends on data richness here.

#### Trip container
- Create and name a trip (destination(s), dates, cover photo)
- Day-by-day structure, drag-and-drop reorder within and across days

#### Planning phases — UX concern, not data concern
The rough plan vs. actual booking distinction is a **UI/UX layer**, not a data schema difference. The underlying data model is uniform across all entries. What changes is how the UI presents fields based on booking status:

- When status = Considering / Selected → UI shows simplified view (location, time, type, basic notes). Confirmation #, payment fields, timed entry, attachments are hidden or collapsed.
- When status = Booked → UI expands to show full detail fields, payment section, attachment uploader.
- No data migration between "phases" — the same record, progressively revealed.

This means:
- Comparing options (Phase A): multiple entries of the same type in the same time slot, shown side-by-side in the UI. User picks one → others archived. No special data structure needed.
- Booking detail (Phase B): same entry, status updated to Booked, full fields now editable. IDP attachment triggers auto-fill on relevant fields.

#### Shared vs. personal itinerary entries
Trips support two layers, because contributors often travel from different origins before converging:

- **Shared entries** — visible and editable by all contributors. The main trip timeline.
- **Personal entries** — owned by one contributor (e.g., their individual flight to the first destination). Visible to all, attributed to that person, not editable by others.

Both appear in the trip view; personal entries are visually distinguished with a contributor avatar/name. Supports scenarios like "Day 0: Effie flies DEN→BA, 卓蓬蓬 flies ZHZ→BA — different flights, same destination, meet on Day 1." The handoff from personal → shared is natural, not a formal mode switch.

#### Entry types (user selects per entry)
Each type has a relevant field template; all types share: notes, photos, links, attachments, booking status, price.

| Type | Key fields |
|------|-----------|
| **Accommodation** | Hotel/property name, address, check-in time, check-out time, confirmation #, price |
| **Flight** | Airline, flight #, departure airport + time, arrival airport + time, confirmation #, seat, price |
| **Train / Ferry / Bus** | Carrier, route, departure + arrival, booking ref, price |
| **Car rental / Transfer** | Provider, pickup location + time, dropoff, confirmation #, price |
| **Activity / Attraction** | Name, address, timed entry slot, booking required (Y/N), confirmation #, duration, price |
| **Restaurant** | Name, address, time, reservation required (Y/N), confirmation #, estimated cost per person |
| **Free time** | Time block, optional notes — no structured fields |
| **Custom** | User-defined label + freeform fields |

#### Booking status (per entry)
- Considering
- Selected
- Booked
- No reservation needed
- Cancelled

#### Budget & payment tracking
Per entry:
- **Total Amount** — with a Confirmed / Estimate toggle (is the price locked in or still approximate?)
- **Paid Amount** — how much has actually been paid (handles deposits, partial payments, full payment)

Trip-level summary: total estimated spend, total confirmed spend, total paid — derived from entries.
Currency per entry (for multi-country trips); display in one base currency + local.

#### Timezone handling
- Each entry defaults to the local timezone of its destination
- Timezone shown alongside every time display
- User can manually override per entry
- Trip-level view can optionally show all times in home timezone (for reference)

#### Attachments & IDP
- Attach files to any entry (PDF, image, email screenshot)
- AI reads uploaded booking confirmation docs (IDP) and auto-populates: confirmation #, dates/times, amounts, names — user reviews and confirms
- Supported: airline e-tickets, hotel confirmations, activity vouchers

#### Export
- Export full itinerary as **PDF** (print/archive format)
- Export as **HTML** (shareable static page, mobile-friendly)
- Export as **calendar file** (.ics) — each entry as a calendar event with local timezone (Phase 2)

---

### F2 — Map View
**Priority: MVP**

- Interactive map as visual backbone of the trip
- Every stop pins to map automatically
- Route drawn between stops in order
- Click pin → open stop detail
- Map and itinerary always in sync

---

### F3 — AI Agent (Trip Companion)
**Priority: MVP (core features) / Phase 2 (booking assist)**

Personality: dual-mode — playful travel companion + knowledgeable guide. Not a dry assistant. Proactive, context-aware.

**MVP capabilities:**
- Destination discovery and suggestions
- Itinerary gap detection ("you have 6 hours in Ushuaia with nothing planned")
- Practical flags: visa requirements, weather, seasonal notes, local tips
- Search assistance: surface options for hotels, activities, restaurants

**Phase 2:**
- Booking assist: pre-fill form data, user reviews and submits
- Budget tracking suggestions
- Dynamic rerouting if plans change

---

### F4 — Collaboration
**Priority: MVP**

- Invite collaborators to a trip via link or email
- All collaborators have equal edit permissions
- See who else is viewing / editing (presence indicators — Phase 2 real-time)
- Comment on specific stops or itinerary entries
- Change history / version log (Phase 2)

**Architecture note:** Data model must use CRDT or equivalent from day one to support offline-first and eventual merge. Phase 1 UI does not need real-time cursors but must not block the path to it.

---

### F5 — Sharing & Visibility
**Priority: MVP**

Per-trip visibility setting:
- **Private** — only the creator (+ invited collaborators)
- **Friends** — visible to confirmed connections
- **Public** — anyone with the link, or discoverable

Individual share: generate a read-only link for any trip regardless of visibility setting.

---

### F6 — In-Trip Journaling
**Priority: MVP**

- Add journal entries from mobile, tied to a stop or freeform
- Support: text, photos, voice memo (Phase 2)
- Offline-first: saves locally, syncs on reconnect
- Private by default; can be included in shared/public view

---

### F7 — Content Generation
**Priority: MVP (blog) / Phase 2 (social + platform integrations)**

Input: itinerary structure + journal entries + photos + trip metadata

**MVP output:**
- Long-form blog post draft — narrative structure, highlights, practical info
- AI adapts tone based on trip type and user writing style (learned over time)

**Phase 2 output:**
- Platform-specific social post: 小红书 (long image-text + tags), Instagram (caption + hashtags), X (thread or single post)
- User selects platform → content reformatted and retoned
- One-tap copy or open-in-app (no native publish in Phase 1)

---

### F8 — Content Community
**Priority: Phase 2 (basic infrastructure)**

- Public profile per user
- Browse/discover public trips
- Like / save trips
- No algorithmic feed in MVP — basic browse only
- Not a social platform; a discovery layer for the planning tool

---

### F9 — Offline Mode (Mobile)
**Priority: MVP**

- Full itinerary readable offline
- Journal entries and photo capture work offline
- Changes queue locally and sync on reconnect
- Merge conflicts: not Phase 1 UI, but data layer must support

---

## 5. Platform

| Platform | Purpose | Priority |
|----------|---------|---------|
| Web app | Planning, collaboration, content generation | MVP |
| Mobile app (iOS + Android) | In-trip recording, offline journaling, quick edits | MVP |
| Desktop | Not in scope | — |

Mobile app is not a companion to the web — it's a first-class experience for the in-trip phase.

---

## 6. Key Technical Decisions

| Decision | Direction |
|----------|-----------|
| Data architecture | Offline-first, CRDT-compatible from day one |
| Real-time collaboration | Phase 1: collaborative-editable (not real-time cursors). Phase 2: live presence + conflict resolution UI |
| Offline sync + merge | Phase 1: local queue + sync on reconnect. Merge UI = Phase 2 |
| Map | Mapbox or Google Maps (TBD) |
| Social publishing | Generate + copy/open-in-app. No native API publish in Phase 1 |
| AI agent | LLM-powered, context-aware (trip data in context window). Booking assist = Phase 2 |
| Auth | Required from day one (collaboration + sharing need identity) |
| Storage | Cloud-first with local cache on mobile |

---

## 7. MVP Scope Summary

**In MVP:**
- Trip + itinerary builder (rich entries, map-linked)
- Map view
- AI agent (discovery, suggestions, flags)
- Collaboration (equal permissions, invite by link/email)
- Sharing tiers (private / friends / public)
- In-trip journaling (mobile, offline)
- Blog post generation (long-form draft)
- Offline mode (mobile)

**Phase 2:**
- AI booking assist (form pre-fill)
- Real-time collab cursors + conflict resolution UI
- Social media post generation (platform-specific)
- Content community (browse public trips)
- Voice memo in journaling
- Auto travel time calculation
- Budget tracking

---

## 8. Open / Deferred

- Product name — TBD
- AI agent name / persona — TBD
- Map provider — Mapbox vs Google Maps (evaluate cost + API limits)
- Tech stack — TBD (to be discussed)
- Monetization — usage-based, timing TBD

---

_PRD is a living document. Update as decisions are made._
