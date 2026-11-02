---
tags: [spec, hikvision, dock-door, rfid, scaffold]
created: 2026-11-02
status: scaffold (questions first, no design commitments)
---

# Dock-Door RFID + Hikvision Alarm: Spec Scaffold

> [!important] Status
> Your note says this project is at **spec stage only**. So this file is a **question-led scaffold**: it lists what must be decided and verified before a real spec can be written. No Hikvision model, API endpoint or alarm behaviour is asserted as fact.
> 🟡 General background, to verify per model: many Hikvision NVRs/cameras offer an HTTP-based integration API (commonly called **ISAPI**) and physical **alarm inputs/outputs**. Whether *your* models support what's needed is ❓.

## 1. Business intent (fill in with the customer)
- [ ] What exactly is "the alarm"? Unauthorized item leaving · wrong dock door · shipment mismatch vs order · tag seen with no open shipment
- [ ] Who must react, and how fast? (Guard, dock supervisor, ops manager)
- [ ] Desired output: siren/strobe · popup on the guard's VMS · video bookmark/clip linked to the event · record in the WMS
- [ ] Is video evidence the main value (proof of who/what), or the real-time stop?

## 2. Integration patterns (choose after the facts are in)
| Pattern | Flow | Pros | Cons / unknowns |
|---|---|---|---|
| P1 Hardwired | Reader GPO → Hikvision alarm **input** | No software; very robust | Needs GPO on the reader ❓; no tag data in the event |
| P2 Middleware → API | Reader → middleware decides → calls Hikvision API to trigger an event/output/bookmark | Rich logic (check against WMS order) | API support per model ❓; credentials and security |
| P3 Middleware → relay | Middleware → network relay/IO module → siren + Hikvision alarm input | Vendor-independent | Extra hardware |
| P4 Camera-first | Camera motion/line-crossing → triggers reader inventory | Reader only on when needed | Timing; camera event API ❓ |

## 3. Facts to verify
| # | Question | Status |
|---|---|---|
| H1 | Exact Hikvision models (NVR, cameras) and firmware on site | ❓ |
| H2 | Free alarm inputs/outputs on the NVR or camera? | ❓ |
| H3 | Does the model's API support triggering an alarm output / custom event / recording tag? Test on the bench | ❓ |
| H4 | Which VMS do the guards use (iVMS-4200, HikCentral, NVR monitor)? Can it pop up an alarm? | ❓ |
| H5 | Reader GPI/GPO availability and electrical spec (voltage, dry contact?) | ❓ from the reader datasheet |
| H6 | Direction detection needed (in vs out)? Two antennas or two readers per door? | ❓ |
| H7 | Where is the "allowed to leave" truth: WMS order, shipment list, gate pass? | ❓ |
| H8 | Network path between the reader, middleware and NVR (same VLAN? firewall?) | ❓ |

## 4. Acceptance criteria (draft, measurable)
- Detect ≥ __% of tagged items passing at ≤ __ m/s (define the test: N passes, pallet type)
- False alarm rate ≤ __ per day/door
- Alarm latency from tag read to siren/VMS popup ≤ __ s
- Each alarm event has: door, time, EPC list, linked video timestamp

## 5. Bench test plan (before any quote)
1. One reader + one antenna + one Hikvision device on the bench.
2. Verify H3 (or H2+H5 for the hardwired route) with the simplest possible trigger.
3. Measure latency 20 times and record min/avg/max.
4. Then write spec v0.1 using the same structure as [[02-capstone-spec-ura4-store-monitor]].

## 6. Reuse from other files
- Read-zone problems → [[07-uhf-rfid-troubleshooting-tree]] (sections C, D)
- Site data → [[06-field-survey-template]]
- Network/credentials → [[05-iot-security-checklist]] (B, D)
