---
tags: [index, harry-workbench]
created: 2026-11-02
---

# Harry's Workbench (made during your flight)

Your fork of *project-based-learning* is a general tutorial list. I turned it into a **personal toolkit** aimed at your Chainway FAE work and your career targets. Everything is Markdown with `[[wikilinks]]`, so this folder works as an Obsidian vault as-is.

## Start here: 15 minutes when you land
1. Read the **F1–F8 fact table** in [[02-capstone-spec-ura4-store-monitor]]. **F5** (can the URA4 take a second host connection alongside the WMS?) decides the whole design. Test it on the bench this week.
2. Quick win, zero code: install **Uptime Kuma** (self-hosted ping/TCP monitor) and add 3 pilot store readers. You get L0 monitoring this week.
3. Send prompt template #6 (spec review) from [[03-prompt-library-fae]] with the capstone spec pasted in, and let it attack the design.

## Files
| File | What it is | Feeds which goal |
|---|---|---|
| [[01-learning-path-12-weeks]] | 12-week plan built from this repo's tutorials, filtered by the repo bot's link-health data, with work-linked deliverables | Linux, networking, Node.js, self-hosting |
| [[02-capstone-spec-ura4-store-monitor]] | **Spec only** (your rule) for monitoring about 30 stores of URA4 readers. Unknowns marked, no invented device facts | URA4 project |
| [[03-prompt-library-fae]] | 7 copy-paste prompt templates + a daily drill | Goal #1: prompt engineering |
| [[04-sla-kpi-starter]] | ITIL-lite priority matrix, SLA targets, KPI formulas, monthly report skeleton | Career gaps: ITIL, SLA/KPI |
| [[05-iot-security-checklist]] | Site hardening checklist for readers, handhelds and MQTT | IoT cybersecurity track |

## What I found in the repo (verified from repo files)
- The bot's link sweep (`.github/link-rot-state.json`, last update 2026-07-20) tracks **456 URLs**: 374 OK, 69 blocked to bots (usually fine in a browser), **11 hard-dead**, 2 suspect.
- The repo has **no Bash, no current Android, and no IoT-security tutorials**. The learning path fills those gaps with labelled outside sources.
- The strongest part of the repo for you is *C/C++ → Network programming* (TCP/IP stack, concurrent servers, MQTT broker).
- I did not touch `README.md`. Your fork stays in sync with upstream, and the upstream lint/link bots keep working.

## Assumptions I made (instead of asking)
- About 5 hours/week of study time.
- URA4 readers sit on customer store LANs behind NAT.
- You want work-first value, so every learning week ends with something usable at work.
