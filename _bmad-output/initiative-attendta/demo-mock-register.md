# AttendTa demo data register

Decision: 2026-11-06 — demo rescope, approved by the project owner. Every piece of demo data
that stands in for a missing backend is listed here with the ticket that removes it. The
initiative's Delivery section and `delivery-queue.md` give the phase order.

This file is the planning record. The code keeps the same ids in a comment on each demo
class, so a search for the id finds every line to delete.

## Rules every demo-phase ticket follows

1. **Real first.** A screen calls an existing service method wherever one returns the data or
   runs the command. Demo data covers only what no existing method provides. A demo provider
   may compose existing reads (for example the published feed plus the per-event registration
   detail) so real events, registrations and members appear.
2. **One switch, any environment.** The configuration key `DemoData:Enabled` turns demo data on.
   It is off in every committed `appsettings*.json`; the demo machine sets the environment
   variable `DemoData__Enabled=true`. It does not depend on `ASPNETCORE_ENVIRONMENT`, because the
   repository sets no environment and an unset environment runs as Production. With the switch
   off, the same screens show their normal empty state and an unavailable command returns the
   standard refusal outcome; nothing breaks the existing test suites.
3. **No visible sign.** Nothing rendered to a browser tells a viewer the data is not real: no
   banner, badge, ribbon, watermark, toast, footer line, tooltip, alt text, aria label, page
   title, file name, console message or CSS class/id containing "demo", "mock", "sample",
   "fixture", "prototype", "simulated" or "test". Demo records use realistic names and values.
   Generated PDFs use the real snapshot template (`RenderSnapshotAsync`), never the watermarked
   sample render. Server-side logs may note at startup which register ids are active.
4. **No real writes.** A simulated command changes only the in-memory demo store, keyed by real
   record ids. It never inserts or updates SQL rows, never writes under `Common:SetupRoot`, and
   never appends to the Activity Log. Restarting the app resets the demo store.
5. **Coherent flow.** One demo store serves every screen, so an action on one screen shows on
   the others for the rest of the session: check in a member, see the attendance in their
   history, complete the event, approve and issue the certificate, see it in the member's list
   and in the report. Start-up seeds enough scenarios on real records that every demo screen
   opens populated.
6. **Never hides a real backend.** When a real implementation exists for a contract, its demo
   provider is not registered. A failing real service shows its real error; it never falls back
   to demo data.
7. **Same contract, one place.** A demo provider implements the same service interface the real
   ticket will implement. All demo code lives in one WebApp folder and is registered from one
   extension method behind the switch, so removing a row means deleting its classes and its
   registration line.
8. **Record it.** A ticket that adds demo data adds or fills its row below in the same pull
   request (interface, classes, seeded scenarios). A ticket that replaces it deletes the classes
   and sets the row to `removed` with the commit. 8.11 proves no row is open.

## Register

| Id | Surface | Missing backend today | Added by | Removed by | State |
|---|---|---|---|---|---|
| M-01 | Demo store (shared state: event check-in and completed state, pass tokens, attendance, certificate reviews and issues, account and membership status), `DemoData:Enabled` switch, provider registration, start-up seed | — (infrastructure) | 1.13 | 8.11 | open |
| M-02 | My Events: all of a member's registrations across events, with status and cutoff labels | Cross-event owner query (only per-event and upcoming-only reads exist) | 3.17 | 3.13 | open |
| M-03 | Pass card QR image and PNG download (token from the demo store, QR drawn by a locally served browser library) | QR library, token read, authorized PNG endpoint | 4.10 | 4.1 | open |
| M-04 | Check-in window open/close command (state in the M-01 store); controls on the dossier (3.16) and scanner (4.11) | Open/close command | 1.13 | 4.2 | open |
| M-05 | Check-in by scan: outcome (success, repeat, invalid, closed) and the attendance record | Validation engine and attendance write | 4.11 | 4.3 (engine), 4.4 (camera wired to it) | open |
| M-06 | Manual lookup of confirmed participants by name or student number, and manual check-in | Registration-scoped search and manual check-in | 4.11 | 4.5 | open |
| M-07 | Attendance invalidation with reason | Invalidation command | 4.11 | 4.6 | open |
| M-08 | Member attendance history | Owner-scoped attendance query | 4.12 | 4.7 | open |
| M-09 | Event completion command (status Completed, check-in closed; state in the M-01 store); control on the dossier (3.16) | Completion command | 1.13 | 3.12 | open |
| M-10 | Certificate reviews (a pending review is derived for each checked-in registration of a completed event until decided); approve and decline decisions | Review creation hook, review queue and decisions | 6.12 | 6.4 (creation), 6.6 (queue and decisions); 6.5 adds the invalidation guard | open |
| M-11 | Certificate issuance and the officer's PDF view and download (rendered in memory with the real template; nothing retained) | Issuance pipeline, retained file, authorized download | 6.12 | 6.7 (issuance), 6.8 (download) | open |
| M-12 | Member certificate history and download links | Owner-scoped certificate query | 6.13 | 6.9 | open |
| M-13 | Participation report: summary counts, rate, no-shows, course/year breakdowns, paged detail rows | Report service and queries (only unused `ParticipationFacts` helpers exist) | 7.4 | 7.1 (summary), 7.2 (no-shows, breakdowns), 7.3 (detail rows) | open |
| M-14 | Account deactivation and reactivation with reason (list shows the new state for the session) | Account access commands and cascade | 5.11 | 5.4 (deactivate), 5.5 (reactivate) | open |
| M-15 | Membership deactivation and reactivation with reason | Membership status commands and cascade | 5.11 | 5.6 (deactivate), 5.7 (reactivate) | open |

A demo-phase ticket that finds another missing contract adds the next id (M-16, …) with its
removing ticket; when no ticket owns the real work, the builder names that gap in the pull
request and the row's "Removed by" says `8.11`.

## Known limits of the demo

- Deactivating an account in the demo does not end that person's real session or block their
  login; the real behavior arrives with 5.4.
- Demo attendance and certificates exist only until the app restarts.
- A pass scanned in the demo is matched against demo-store tokens, not `registrations.qr_token`.
