---
tags: [iot-security, checklist, deployment, rfid]
created: 2026-11-02
---

# IoT Security Checklist for RFID Deployments (stores and warehouses)

> [!info] Scope and evidence
> These are **generic IoT/network hardening practices** (aligned in spirit with the OWASP IoT Top 10 themes: weak passwords, insecure network services, lack of updates, insecure defaults). Nothing here claims what a specific Chainway model supports. Mark each item **N/A** where the device has no such feature, and record which items you verified on the device.

How to use it: one copy per site survey or deployment. ✅ done · ❌ gap · N/A · ❓ unknown.

## A. Device identity and access
- [ ] Default admin/web/SSH passwords changed (if the device has any login)
- [ ] Credentials are unique per site, not one password for all 30 stores
- [ ] Credentials stored in a password manager, not in Excel or Zalo chats
- [ ] Unused services disabled (web UI, Telnet, FTP, debug ports) where configurable
- [ ] Physical: reader and cabling not reachable by the public; USB/serial ports not exposed

## B. Network
- [ ] RFID devices on a **separate VLAN/SSID** from the POS and guest Wi-Fi
- [ ] Firewall: readers can talk only to the hosts they need (WMS/middleware/monitor)
- [ ] No port-forwarding from the internet to readers
- [ ] Static IP or DHCP reservation documented (IP, MAC, location) in the site sheet
- [ ] Remote access goes through a VPN, never through an open RDP/AnyDesk with saved passwords

## C. Android handhelds/tablets (your Android 13 work)
- [ ] Enrolled in MDM where the customer has one (app allow-list, remote wipe)
- [ ] Developer options / USB debugging **off** in production
- [ ] Only signed release builds of your app, no debug APKs left on devices
- [ ] Screen lock policy set; kiosk/lock-task mode where staff don't need other apps
- [ ] Security patch level recorded at deployment (Settings → About)

## D. Data in transit and at rest
- [ ] Reader → server traffic encrypted where the protocol allows (TLS/VPN); if it can't be, isolate it at the network layer (section B)
- [ ] MQTT: TLS + per-client credentials + topic ACLs (each store can publish only to its own topics)
- [ ] API keys for WMS/ERP integration kept in config/env, not hard-coded in the APK

## E. Updates and lifecycle
- [ ] Firmware version recorded per device at install
- [ ] A process exists to check for vendor firmware updates (quarterly 🟡)
- [ ] Spare/RMA devices are wiped before they return to the pool
- [ ] Decommissioned devices: credentials revoked, removed from MDM/monitor

## F. Logging and monitoring
- [ ] Device online/offline is monitored (→ [[02-capstone-spec-ura4-store-monitor]])
- [ ] Clocks are synced (NTP), otherwise incident timelines can't be trusted
- [ ] Who changed what on reader config is logged, even if only by hand in a change log

## Site score
| Section | ✅ | ❌ | N/A | ❓ |
|---|---|---|---|---|
| A | | | | |
| B | | | | |
| C | | | | |
| D | | | | |
| E | | | | |
| F | | | | |

> [!tip] Career angle
> Run this at 3 sites and write up the findings anonymised. That gives you a real "IoT security assessment" sample for an IoT cybersecurity role.
