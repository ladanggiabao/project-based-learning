---
tags: [template, field-survey, rfid, deployment]
created: 2026-11-02
---

# RFID Site Survey Template (one copy per site)

> [!tip] How to use
> Duplicate this note as `survey-{{customer}}-{{site}}-{{YYYY-MM-DD}}`. Fill it in on site on your phone (Obsidian mobile). Afterwards, paste it into prompt #3 in [[03-prompt-library-fae]] to get the deployment report.
> Mark each field ✅ observed · 🗣 told by customer (not verified) · ❓ unknown.

## 1. Site basics
| Field | Value | Source |
|---|---|---|
| Customer / site name | | |
| Address / GPS | | |
| Site contact (name, role, phone, Zalo) | | |
| Customer IT contact | | |
| Opening hours / shifts | | |
| Survey date, surveyor | | |

## 2. Use case
- [ ] Inventory / stocktake (handheld) · [ ] Dock door in/out (fixed) · [ ] EAS / anti-theft · [ ] Shelf / zone tracking · [ ] Other: ___
- Item types and **materials** (metal, liquid, foil packaging, garments): ___
- Tag type in use or planned (inlay model, size, on-metal?): ___
- Expected volume (tags per pass, items per day): ___
- Required read rate / accuracy (customer's words): ___
- WMS/ERP system + version, and integration method (API, DB, file, middleware): ___

## 3. RF environment
| Check | Finding |
|---|---|
| Other UHF readers on site (brand, location, channel)? | |
| Metal structures near read zones (racks, roller doors, conveyors)? | |
| Liquids / cold storage near read zones? | |
| Distance from read zone to areas that must NOT be read (stray reads)? | |
| Wi-Fi / other RF sources (informational) | |
| Power outlets / PoE available at each mount point? | |
| Mounting surface + height available | |

## 4. Network
| Check | Finding |
|---|---|
| Wired drop at each reader location? Cable run length (m) | |
| Switch model / PoE budget | |
| VLAN for devices? Guest/POS separation? | |
| DHCP or static? IP range allocated | |
| Wi-Fi coverage at handheld work areas (signal measured where?) | |
| Internet / VPN access for remote support | |
| Firewall rules needed (host ↔ reader ports: ❓ from SDK docs) | |

## 5. Photos checklist
- [ ] Each read zone, wide shot · [ ] mount point close-up · [ ] network cabinet / switch · [ ] sample items with tags · [ ] problem areas (metal, liquids)

## 6. Risks and open questions
| # | Risk / question | Impact | Owner | Due |
|---|---|---|---|---|
| 1 | | | | |

## 7. Preliminary BOM (draft)
| Item | Qty | Note |
|---|---|---|
| Fixed reader | | |
| Antenna | | |
| RF cable (length) | | |
| Handheld | | |
| Tags (type) | | |
| Network (switch/PoE injector/AP) | | |
| Mounting hardware | | |

## 8. Next actions
- [ ] ___ (owner, date)
- [ ] Run [[05-iot-security-checklist]] sections A–B with customer IT
