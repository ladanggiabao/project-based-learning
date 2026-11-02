---
tags: [spec, rfid, ura4, monitoring, iiot]
created: 2026-11-02
status: draft-v0.1 (spec only, not approved for build)
---

# Spec v0.1: URA4 Fleet Health Monitor (~30 stores)

> [!important] Scope rule
> This is a **spec and design draft only**. Nothing gets implemented until you approve it. Every device fact is marked ✅ Verified, 🟡 Assumption or ❓ Unknown. **No URA4 interface, port, protocol or SDK method is claimed as fact here.** Fill those in from the Chainway datasheet, SDK and demo source.

## 1. Problem
About 30 stores each run Chainway URA4 UHF RFID readers. Today, a reader going offline or reading poorly is probably found late, through store complaints (🟡 assumption, confirm). Goal: **find faults before the store calls**, and give you SLA numbers you can report (see [[04-sla-kpi-starter]]).

## 2. Goals / non-goals
| Goals (v1) | Non-goals (v1) |
|---|---|
| Know within N minutes that a reader is unreachable | Changing reader config remotely |
| Know when a reachable reader has stopped reading tags ("silent reader") | Inventory accuracy analytics |
| Per-store and fleet uptime % for monthly SLA reports | Replacing the customer's WMS/ERP |
| Alerts to you (FAE) and, optionally, store IT | Hikvision dock-door alarms (separate project, still at spec stage) |

## 3. Facts to collect before design freeze (your homework)

| # | Question | Status | Where to find it |
|---|---|---|---|
| F1 | URA4 network interfaces (Ethernet? Wi-Fi? PoE?) | ❓ | Datasheet |
| F2 | Host protocol: Chainway proprietary SDK, LLRP, HTTP, MQTT, TCP server/client mode? | ❓ | SDK docs, demo source |
| F3 | Does the reader *push* to a server (client mode) or wait for a host to connect (server mode)? | ❓ | SDK docs |
| F4 | Is there a heartbeat or a "get status / temperature / antenna" call? | ❓ | SDK API list. **Do not assume.** |
| F5 | Can more than one host connect at once? (Decides whether a monitor can sit next to the WMS connection.) | ❓ | Test it on the bench. **Critical.** |
| F6 | Who owns the store network: customer IT or a partner? Is there VPN access? | ❓ | Customer |
| F7 | Does an edge box exist in each store (PC, Android tablet, gateway)? | ❓ | Field survey |
| F8 | What does the WMS/ERP see when a reader stops: an error, or just no data? | ❓ | Customer IT |

> [!warning] F5 is the design-breaker
> If the reader accepts only **one** host session and the WMS already holds it, the monitor **cannot** connect directly. It must then use an **indirect** signal: ping/ARP, switch-port status, or "time since last read" seen in WMS data. Test this before anything else.

## 4. Health signals (ranked by how little they need from the device)

| Level | Signal | Needs | Detects |
|---|---|---|---|
| L0 | ICMP ping / TCP port open | IP only (works if F5 = single session) | Power, cable, switch, IP conflict |
| L1 | Last-read timestamp per reader (from WMS DB/API or our own collector) | WMS access **or** an SDK session | "Silent reader": antenna, RF, config, app crash |
| L2 | Device status (temperature, antenna VSWR, firmware version) | Only if F4 = yes | Hardware degrading before failure |
| L3 | Read-rate baseline per hour/weekday | L1 history ≥ 2 weeks | Gradual degradation, environment change |

v1 = L0 + L1. L2 only if the SDK supports it (❓ F4).

## 5. Proposed architecture (🟡 draft, adjust after F1–F8)

```
[Store x30]                         [Central: VPS or office Pi/NUC]
 URA4 ──LAN── Edge probe ──MQTT/TLS──▶ Mosquitto ──▶ Collector (Node.js)
             (Pi or existing PC)                      │
             - ping/TCP check (L0)                    ├─▶ DB (SQLite → Postgres)
             - SDK read stats (L1/L2, if F5 allows)   ├─▶ Alerting (Telegram/Zalo/email)
                                                      └─▶ Dashboard (React/Next.js)
```

- **Why edge probes:** store LANs are usually NATed and behind customer firewalls (🟡). Outbound MQTT/TLS from the store avoids opening inbound ports.
- **Option without edge hardware:** if a VPN to all stores exists (F6), central polling with L0 only. Cheaper, but blind to "silent reader".
- **Off-the-shelf shortcut for L0:** Uptime Kuma (self-hosted, open source) can do ping/TCP checks plus notifications. You could have L0 running this week with zero code. Then build only L1, which no generic tool does.

### MQTT topic tree (draft; refine in learning-path Week 5)
```
chainway/{customer}/{store_id}/{reader_id}/status    # online|offline + ts (retained)
chainway/{customer}/{store_id}/{reader_id}/metrics   # reads_5min, last_read_ts
chainway/{customer}/{store_id}/{reader_id}/alarm     # raised/cleared events
```
Use an MQTT Last Will on `.../probe/status` so a dead **probe** is not mistaken for a dead **reader**.

## 6. Alert rules (draft)
| Rule | Condition | Priority (see SLA doc) |
|---|---|---|
| R1 Reader unreachable | L0 fails 3 checks in a row (e.g. 3 × 60 s) | P2 (P1 if > 30% of the store's readers) |
| R2 Silent reader | Reachable, but 0 reads for X min **during store opening hours** | P3 |
| R3 Probe offline | Probe LWT fired | P3 (this is our own infrastructure) |
| R4 Fleet event | ≥ 5 stores down at once | P1: likely a central/network/WMS issue, not readers |

Opening hours per store go in config. Without them, R2 will fire every night. ❓ Get the store hours list.

## 7. Data model (minimum)
- `stores(id, name, address, open_time, close_time, contact)`
- `readers(id, store_id, ip, model, fw_version?, install_date, location_note)`
- `checks(reader_id, ts, l0_ok, last_read_ts?, reads_5min?)` (retention 90 days 🟡)
- `incidents(id, reader_id, rule, priority, opened_at, acked_at, resolved_at, root_cause, action)`, which feeds the MTTA/MTTR KPIs

## 8. Security requirements (from [[05-iot-security-checklist]])
- Probes connect outbound only, with MQTT over TLS and a separate credential per store.
- The monitor never stores WMS credentials in plain text. Read-only DB user if L1 comes from the WMS.
- The dashboard sits behind auth. No public exposure of reader IPs.

## 9. Phased rollout
| Phase | Scope | Exit criteria |
|---|---|---|
| P0 Bench | 1 URA4 on your desk; answer F2–F5 | Fact table filled in |
| P1 L0 only | Uptime Kuma on 3 pilot stores | 2 weeks with no false alarms |
| P2 L1 | Collector + "silent reader" on pilot stores | Catches ≥ 1 real issue before the store calls |
| P3 Fleet | All 30 stores + monthly SLA report | Report delivered to customer/manager |

## 10. Open decisions for you
1. Central host: VPS vs office machine? (Assumption: VPS, because it is reachable from all stores.)
2. Edge probe hardware: Pi per store vs an existing store PC? (Cost vs control.)
3. Alert channel: Telegram, Zalo or email? (Zalo is common in VN; check its API terms ❓.)
4. Is this an internal FAE tool, or a sellable service for the customer? It changes the SLA wording.
