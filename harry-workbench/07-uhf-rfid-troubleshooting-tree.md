---
tags: [troubleshooting, uhf-rfid, field, training]
created: 2026-11-02
---

# UHF RFID Troubleshooting Decision Tree (vendor-neutral)

> [!info] Evidence level
> These are **general UHF RFID (EPC Gen2 / ISO 18000-63) principles**, not Chainway-specific settings. Setting names and ranges differ by device. Check them in the device manual or SDK. Where a value is regulatory (frequency band, max power), **confirm it against the current Vietnam regulation and the device's region setting** ❓. I am not stating the band from memory.

Use it as a field card, and as the base for a training slide set (prompt #4 in [[03-prompt-library-fae]]).

## Step 0: Classify the symptom first
| Symptom | Go to |
|---|---|
| Reader not reachable / SDK can't connect | **A** |
| Connected, but reads **nothing** | **B** |
| Reads, but **misses** some tags | **C** |
| Reads tags it **shouldn't** (stray/cross reads) | **D** |
| Tag read **once, then disappears** during a continuous inventory | **E** |
| Works, then fails **intermittently** | **F** |

## A. Not reachable
1. Power LED / PoE budget on the switch port → physical.
2. Link light on the port? Swap the cable and port.
3. `ping` the reader IP. If that fails, check the IP/subnet, an **IP conflict** (`arp -a`: two MACs for one IP?) and the VLAN.
4. Ping OK but the SDK fails → is the port blocked by a firewall? **Is another host already holding the session?** (See F5 in [[02-capstone-spec-ura4-store-monitor]].)
5. Recently changed firmware or config? Roll back or factory-reset per the manual.

## B. Connected, zero reads
1. Is the **antenna port enabled** in config for the port that is physically connected?
2. Antenna cable seated and undamaged? Test with a known-good antenna and cable.
3. Is the region/frequency setting correct for the country? (A wrong region can make it illegal and/or poor-performing.)
4. TX power set to a reasonable value (not minimum)?
5. Test a **known-good reference tag** at 30–50 cm, face-on to the antenna. If that fails, the problem is hardware/config. If it works, go to **C**.
6. Inventory actually started? (App, SDK or trigger mode: some fixed readers wait for a GPI trigger.)

## C. Missed tags
| Cause | Check | Typical fix |
|---|---|---|
| Tag on metal / liquid (detuning) | Same tag reads fine in free air? | On-metal tags, spacers, change tag position |
| Tag orientation vs antenna polarization | Rotate the tag 90°: reads improve? | Circular-polarized antenna, or add a 2nd antenna at a different angle |
| Range / power | Reads near, fails far? | Raise power (within legal limit), reposition the antenna, shorter or lower-loss cable |
| Dense tag population | Count stalls below the expected number? | Adjust Gen2 **Q** / session / search mode per SDK; longer dwell time |
| Speed (dock door, conveyor) | Fails only when moving fast? | Longer read window, more antennas, slow-down zone |
| Shadowing (tags stacked behind others, inside cartons) | Fails only for inner items? | Antennas from several sides, change pallet orientation |
| Bad / damaged tags | Fails at a fixed position even near the antenna? | Replace; sample-test the tag batch |

## D. Stray / cross reads
1. Lower the TX power step by step until the wanted zone still reads 100% and the unwanted one doesn't.
2. Aim/tilt the antenna away from neighbouring zones; check for reflections off metal doors.
3. Use RSSI filtering or a **direction/sequence logic** (two antennas: which fired first) in software.
4. Physical shielding (RF-absorbing or metal panels) between doors as a last resort.

## E. Tag read once, then "disappears"
- Gen2 **sessions** (S0–S3) control how long a tag stays "already inventoried" (flag A→B) after being read. In S1/S2/S3 the tag can stay silent for a while, which is intended for counting large populations once.
- For **continuous presence monitoring**, use the session/target mode the SDK documents for repeated reads (often S0 or dual-target A↔B) ❓ per device.

## F. Intermittent
1. Correlate with time: shift change (people/forklifts), a conveyor starting, Wi-Fi peaks, power events.
2. Check the reader temperature / ventilation if the SDK exposes it ❓.
3. Network: DHCP lease expiry (use a reservation), switch power-saving (EEE) on the port, duplex mismatch.
4. Interference from another reader nearby on overlapping channels: coordinate the channels or use dense-reader mode if supported ❓.
5. Put the site on the monitor and look at the pattern over days, not minutes.

## Escalation pack (collect before calling vendor/HQ)
- [ ] Model, serial, firmware, SDK/app version
- [ ] Config export or screenshots (region, power, ports, session, mode)
- [ ] Photo of the antenna placement plus the tag on the item
- [ ] Steps to reproduce + what you already ruled out (from this tree)
- [ ] Logs / packet capture if it's network-related
