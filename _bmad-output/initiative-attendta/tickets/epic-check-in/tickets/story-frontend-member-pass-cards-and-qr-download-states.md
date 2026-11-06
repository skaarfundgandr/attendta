---
id: 10
type: story
title: "Frontend member pass cards and QR download states"
parent: epic-check-in
covers: [R1, R2]
after: [3.17]
hitl: false
risk: low
tracker_id: "144"
remote: "https://github.com/rainyDayssss/AttendTa/issues/144"
tracker_status: backlog
---

# Frontend member pass cards and QR download states

## Description

Fill 3.17's _PassPartial with the DESIGN screen 13 pass cards: active and past sections, event and holder identity, a QR with an intact quiet zone, and a PNG download. Until 4.1 exists the QR encodes the demo-store token (M-03) and is drawn by a locally served browser library, and the download saves that image. Pending, waitlisted, cancelled or unavailable registrations show no usable pass. Own the pass partial and its scoped assets, not MyEventsController or the host card. The 4.11 scanner matches these demo-store tokens.

## Requirements

Verify: A Member opens My Passes, sees a card per confirmed registration with a QR that encodes that registration's demo-store token, downloads it as PNG, and sees no usable pass for a pending, waitlisted or cancelled registration; cards read well in one and two columns on phone and desktop; nothing on screen says the data is demo data.

## References

- parent — _bmad-output/initiative-attendta/tickets/epic-check-in/epic-check-in.md
- docs/DESIGN.md
- _bmad-output/initiative-attendta/delivery-queue.md
- _bmad-output/initiative-attendta/demo-mock-register.md

## Notes

- Demo phase (2026-11-06 demo rescope): frontend only. Use the real backend where a service method exists and demo data per `demo-mock-register.md` (rows M-03) where it does not; nothing on screen may show that data is demo data. Add or fill this ticket's register rows in the same pull request.
- Done check: `dotnet build`, `npm run css:build` when views or styles change, and one look at the changed screen with `DemoData__Enabled=true`. No new automated tests, no per-ticket Impeccable critique or polish and no separate code-review pass; the pull request keeps green existing CI and one peer approval. Critique and polish run in 8.15.
