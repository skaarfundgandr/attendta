---
id: 11
type: story
title: "Remove prototype data and prove complete real integrations"
parent: epic-release-acceptance
covers: [R1, R2, R3]
after: [13, 2.10, 2.11]
hitl: false
risk: high
tracker_id: "152"
remote: "https://github.com/rainyDayssss/AttendTa/issues/152"
tracker_status: backlog
---

# Remove prototype data and prove complete real integrations

## Description

Use `demo-mock-register.md`: confirm every row M-02 to M-15 is `removed` by its integration ticket and finish any that is not, then delete the demo store, the demo data folder, the `DemoData:Enabled` switch and its registration (M-01) and set M-01 removed. Verify My Events, passes, attendance, scanner, completion, certificate review, issue and download, access changes and reports against one coherent database. Keep only isolated test fixtures in the test projects. This is a functional integration gate, not a frontend redesign or a claim that the final test suite already passed.

## Requirements

Verify: Every register row shows `removed` with its commit; a search of the code finds no register id and no demo data code; starting the app with `DemoData__Enabled=true` changes nothing; a real member goes from approved account through registration, pass, check-in and completion to a retained certificate and matching report and audit, with no manual SQL state workaround.

## References

- parent — _bmad-output/initiative-attendta/tickets/epic-release-acceptance/epic-release-acceptance.md
- _bmad-output/initiative-attendta/delivery-queue.md
- _bmad-output/initiative-attendta/finale-test-cases.md
- _bmad-output/initiative-attendta/demo-mock-register.md

## Notes

- Removes demo data M-01 (2026-11-06 demo rescope): delete the demo store, the DemoData:Enabled switch, the demo data folder and their registration; set M-01 to removed with the commit in demo-mock-register.md.
