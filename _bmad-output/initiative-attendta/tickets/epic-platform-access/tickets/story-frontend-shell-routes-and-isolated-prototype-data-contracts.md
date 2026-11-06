---
id: 13
type: story
title: "Frontend shell routes and isolated prototype data contracts"
parent: epic-platform-access
after: [9]
hitl: false
risk: medium
tracker_id: "136"
remote: "https://github.com/rainyDayssss/AttendTa/issues/136"
tracker_status: backlog
---

# Frontend shell routes and isolated prototype data contracts

## Description

After a short inventory of the current routes and screens against docs/DESIGN.md, prepare the shared role-appropriate navigation, a route for every screen the demo needs (My Events with passes, attendance and certificates; the officer event dossier; the door scanner; the certificate ledger and viewer; the participation report; the account and membership access dialogs) and typed screen contracts. Build the demo data seam from demo-mock-register.md row M-01: the DemoData:Enabled switch read in any environment and off in committed settings; one in-memory demo store keyed by real record ids that holds the shared demo state several surfaces read (event check-in open and completed state, pass tokens, attendance records, certificate reviews and issues, account and membership status); one WebApp folder for demo providers with a registration file per surface, all called from one method, so slices do not edit the same lines; and a start-up seed on real records so every demo screen opens populated: a published event with check-in open and confirmed registrations, a started event ready to complete, and a completed event with check-ins (one invalidated), a pending, a declined and an issued certificate. It also provides the two event-state demo commands several screens call, open/close check-in (M-04) and complete event (M-09), plus two shared partials: the event selector the scanner, ledger and report use, and the open/close check-in control the dossier and scanner use. Surface tickets add their own provider methods over this store; they do not change its shape without a note in the register. Write the route, partial, contract, real-versus-demo source and file-boundary map into the register before the five surface tickets start. Prepare feature-scoped asset seams so later UI tickets do not all edit the shell, Startup.DI.cs or global JavaScript. Preserve Razor/Tailwind, the current shell and docs/DESIGN.md.

## Requirements

Verify: Every new navigation link opens an authorized screen shell for the right role, not a dead link. With DemoData__Enabled=true the seeded screens open populated and nothing rendered says the data is demo data; with it unset the same screens show their normal empty state and the existing test suites still pass. No demo command writes SQL rows, stored files or Activity Log entries. The register lists each screen's real and demo sources with its removing ticket, and the five demo-critical surface tickets can start.

## References

- parent — _bmad-output/initiative-attendta/tickets/epic-platform-access/epic-platform-access.md
- docs/DESIGN.md
- _bmad-output/initiative-attendta/delivery-queue.md
- _bmad-output/initiative-attendta/demo-mock-register.md

## Notes

- Demo phase (2026-11-06 demo rescope): frontend only. Use the real backend where a service method exists and demo data per demo-mock-register.md (rows M-01, M-04, M-09) where it does not; nothing on screen may show that data is demo data. Add or fill this ticket's register rows in the same pull request.
- Done check: dotnet build, npm run css:build when views or styles change, and one look at the changed screen with DemoData__Enabled=true. No new automated tests, no per-ticket Impeccable critique or polish and no separate code-review pass; the pull request keeps green existing CI and one peer approval. Critique and polish run in 8.15.
- Decision: 2026-11-06 — demo rescope: no longer waits on the old 1.12 walkthrough (now the 8.15 demo rehearsal) and absorbs its route inventory; the Development-only, visibly labelled fixture mechanism is replaced by the demo data rules in demo-mock-register.md. The title is kept to match issue #136.
- The final real-data integration gate is 8.11; demo-phase delivery never closes the existing functional integration tickets.
