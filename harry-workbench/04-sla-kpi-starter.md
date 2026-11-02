---
tags: [itil, sla, kpi, career]
created: 2026-11-02
---

# SLA and KPI Starter Kit (ITIL-lite for a 30-store RFID fleet)

> [!info] Why this file exists
> SLAs/KPIs and ITIL are on your gap list for the Area IT lead / IT ops track. The fastest way to learn them is to **run them on your own fleet**. The numbers below are 🟡 **starting proposals**, not industry standards. Tune them to the customer contract.

## 1. Vocabulary (ITIL 4 terms you'll be asked about in interviews)
| Term | Plain meaning | Your example |
|---|---|---|
| Incident | Unplanned interruption or quality drop | URA4 at store 12 offline |
| Problem | Underlying cause of one or more incidents | Bad PoE switch model at 4 stores |
| Change | Planned modification | Reader firmware upgrade |
| SLA | Agreement with the customer | "P2 restored within 8 business hours" |
| OLA | Agreement between internal teams | "Warehouse ships a spare within 1 day" |
| UC (Underpinning Contract) | Agreement with a third party | Courier / ISP / Chainway HQ RMA |
| KPI | Metric that shows if you meet targets | MTTR, uptime % |

## 2. Priority matrix (Impact × Urgency → Priority)
| Impact ↓ / Urgency → | High (business stopped now) | Medium | Low |
|---|---|---|---|
| **High** (multiple stores / fleet) | **P1** | P2 | P3 |
| **Medium** (one store, partly working) | P2 | **P3** | P4 |
| **Low** (one reader, workaround exists) | P3 | P4 | **P4** |

## 3. Draft SLA targets (🟡 proposal)
| Priority | Respond (acknowledge) | Restore / workaround | Update cadence |
|---|---|---|---|
| P1 | 30 min | 4 h | every 1 h |
| P2 | 2 h | 8 business h | every 4 h |
| P3 | 1 business day | 3 business days | daily |
| P4 | 2 business days | next planned visit | weekly |

Define "business hours" explicitly (e.g. Mon–Sat 08:00–18:00 ICT) and list Vietnamese public holidays in the contract annex.

## 4. KPIs: formulas you can compute from the `incidents` table in the capstone spec
| KPI | Formula | Starter target 🟡 |
|---|---|---|
| Reader availability % | (total minutes − minutes down) ÷ total minutes, during opening hours only | ≥ 99.0% |
| MTTA (mean time to acknowledge) | avg(acked_at − opened_at) | P2 < 2 h |
| MTTR (mean time to restore) | avg(resolved_at − opened_at) | P2 < 8 h |
| SLA compliance % | incidents within target ÷ all incidents | ≥ 95% |
| Proactive detection % | incidents opened by the monitor ÷ all incidents | grow monthly (this is your monitor's ROI) |
| Repeat incident rate | readers with ≥ 2 incidents in 30 days ÷ all readers | < 5%. Above that, open a Problem record |
| First-time fix rate | resolved with no second visit ÷ on-site visits | ≥ 80% |

> [!tip] The KPI that sells the monitor
> **Proactive detection %** shows management, in one number, why the monitor exists: "Last month, 70% of faults were found by us before the store noticed."

## 5. Monthly report skeleton (1 page)
```
# RFID Fleet Service Report: {{Month YYYY}} ({{Customer}})
## Headline
- Availability: __% (target 99.0%) · SLA compliance: __% · Proactive detection: __%
## Incidents
| P | Count | Met SLA | MTTR |
## Top 3 problems and actions
## Changes made (firmware, config, hardware swaps)
## Risks / requests for customer
```

## 6. Practice exercises (career gap → evidence for your CV)
- [ ] Back-fill the last 3 months of tickets into the incident table (even roughly) and compute MTTR.
- [ ] Write one Problem record for your most frequent repeat issue (symptom, root cause, workaround, permanent fix).
- [ ] Present the 1-page report to your manager once. That is evidence of "SLA/KPI ownership" for your CV.
- [ ] Later: ITIL 4 Foundation certification (official exam through PeopleCert). This kit covers the core concepts in practice.
