---
tags: [learning-path, linux, networking, nodejs, iot]
created: 2026-11-02
status: ready-to-start
---

# 12-Week Learning Path: from this repo to your FAE toolkit

> [!info] How this was built
> - **Source:** tutorials picked from this repo's `README.md` (676 lines, ~456 links).
> - **Link health:** from `.github/link-rot-state.json`, the repo bot's own sweep. Last state commit: **2026-07-20**. It is not live, so open each link once before you plan around it.
> - **Selection rule:** each pick must feed at least one of your stated targets: Linux, Bash, networking, Node.js, Android, self-hosting, IoT security, or the URA4 store monitor ([[02-capstone-spec-ura4-store-monitor]]).
> - **Assumption:** about 5 h/week (2 weekday evenings + 1 weekend block). With less time, stretch each week to 2 weeks. Keep the order.

## Verified findings about the repo (what it covers and what it doesn't)

| Area you want | Repo coverage | Verdict |
|---|---|---|
| Networking (TCP/IP, servers, MQTT) | Strong: C/C++ → *Network programming* section | ✅ Use the repo |
| Node.js / React | Medium: many entries are 2015–2019 | ⚠️ Use selectively |
| Containers / self-hosting | Some: Liz Rice video, Go chat app "Dockerizing" step | ⚠️ Partial |
| Linux / Bash | **No Bash tutorials**. Only "Write a Shell in C" and Linux containers | ❌ Needs an outside source |
| Android (Kotlin) | **1 entry** (Keddit, ~2016, Medium; the bot gets a 403 there) | ❌ Outdated: use official Android docs |
| IoT security | **None** | ❌ Needs an outside source |

> [!warning] Dead links found by the bot (avoid these)
> `hackclub.com/workshops` (404), `sahatyalkabov.com` React voting app (404), all `ashwinhariharan.tech` "Uber-for-X" Node parts, `thinkster.io`, `hackhands.com`, `arjunsreedharan.org` (both posts). Each has 4 consecutive failures, so the bot marks them HARD_DEAD.

---

## Phase 1: Linux and networking foundations (Weeks 1–4)

### Week 1: How a shell actually works
- 📘 [Write a Shell in C](https://brennan.io/2015/01/16/write-a-shell-in-c/) (bot: OK 200)
  - Goal: understand `fork/exec/wait`, PATH lookup and built-ins. You don't need to become a C dev.
- ➕ Outside the repo (Bash gap): *The Linux Command Line*, William Shotts, free at linuxcommand.org. Read the chapters on redirection, pipes, permissions and processes.
- 🛠 **Deliverable (work-linked):** in plain English, write the steps a Bash script would take to read `stores.csv` (store_id, reader_ip) and report which IPs answer. Spec only, no code yet (your "spec before build" rule). Save it in this folder.

### Week 2: Ethernet, ARP and ICMP (what you troubleshoot on site)
- 📘 Let's Code a TCP/IP Stack
  - [Part 1: Ethernet & ARP](http://www.saminiir.com/lets-code-tcp-ip-stack-1-ethernet-arp/) (OK)
  - [Part 2: IPv4 & ICMPv4](http://www.saminiir.com/lets-code-tcp-ip-stack-2-ipv4-icmpv4/) (OK)
- 🛠 **Deliverable:** take a Wireshark capture of a reader or handheld booting on your bench LAN. Find the DHCP, ARP and the first TCP connection. Write a 1-page "how to read a capture" for field techs (doubles as training material).

### Week 3: TCP behaviour (why readers "go offline")
- 📘 [Part 3: TCP Basics & Handshake](http://www.saminiir.com/lets-code-tcp-ip-stack-3-tcp-handshake/), [Part 4: Data Flow & Socket API](http://www.saminiir.com/lets-code-tcp-ip-stack-4-tcp-data-flow-socket-api/), [Part 5: Retransmission](http://www.saminiir.com/lets-code-tcp-ip-stack-5-tcp-retransmission/) (all OK)
- 📘 Unreviewed but on-topic: [Networking from Scratch](https://github.com/TanayK07/networking-from-scratch) (OK). Skim it and decide if it's worth more time.
- 🛠 **Deliverable:** add a "symptom → layer → check" table to your troubleshooting notes. Example: *reader pings but SDK can't connect* → L4 → port open? firewall? another client holding the session?

### Week 4: Server concurrency → the mental model behind Node.js
- 📘 Programming concurrent servers (eli.thegreenplace.net, all OK)
  - [Part 1 Intro](https://eli.thegreenplace.net/2017/concurrent-servers-part-1-introduction/), [Part 3 Event-driven](https://eli.thegreenplace.net/2017/concurrent-servers-part-3-event-driven/), [Part 4 libuv](https://eli.thegreenplace.net/2017/concurrent-servers-part-4-libuv/), [Part 6 Callbacks, Promises, async/await](https://eli.thegreenplace.net/2018/concurrent-servers-part-6-callbacks-promises-and-asyncawait/)
  - libuv is the event loop under Node.js. After Part 4, Node's "single thread but handles 30 readers" behaviour will make sense.
- 🛠 **Deliverable:** one diagram showing how one Node process could hold 30 store connections without 30 threads.

## Phase 2: IIoT plumbing (Weeks 5–8)

### Week 5: MQTT from the inside
- 📘 MQTT Broker from scratch (codepr.github.io, all OK)
  - [Part 1 The protocol](https://codepr.github.io/posts/sol-mqtt-broker) (must read), [Part 5 Topic abstraction](https://codepr.github.io/posts/sol-mqtt-broker-p5) (must read). Other parts are optional.
- 🛠 **Deliverable:** draft the topic tree for 30 stores, e.g. `chainway/{store_id}/{reader_id}/status|reads|alarm`. Put it into the capstone spec.

### Weeks 6–7: Test-driven Node API
- 📘 [Test-Driven Development with Node, Postgres and Knex](http://mherman.org/blog/2016/04/28/test-driven-development-with-node/) (OK; from 2016, so expect old package versions)
- ⚠️ Assumption: use current Node LTS and expect to fix version errors. Debugging those is part of the learning.
- 🛠 **Deliverable:** a working `/stores` and `/readers` CRUD API with tests. This is the first code you write, and it is fake-data only, so no device integration yet.

### Week 8: Containers and self-hosting
- 📘 [Building a container from scratch in Go – Liz Rice (video)](https://www.youtube.com/watch?v=8fi7uSYlOdc) (OK). Watch it for the namespaces/cgroups concepts; you don't need Go.
- 📘 Optional: [Linux Container in 500 Lines of Code](https://blog.lizzie.io/linux-containers-in-500-loc.html) (OK; advanced, skim it)
- 📘 [Chat app in Go + React – Part 6 Dockerizing your Backend](https://tutorialedge.net/projects/chat-system-in-go-and-react/part-6-dockerizing-your-backend/) (OK). Read it for the Dockerfile pattern.
- 🛠 **Deliverable:** run Mosquitto + your Week 6–7 API with `docker compose` on a Raspberry Pi or old laptop.

## Phase 3: Dashboard, mobile and security (Weeks 9–12)

### Week 9: React dashboard (builds on your ReactJS skill)
- 📘 [Next.js App Router – Dashboard course](https://nextjs.org/learn/dashboard-app) (OK, official, current)
- 🛠 **Deliverable:** a stores table with an online/offline badge, reading from your API.

### Week 10: Offline-first and push alerts (for store tablets)
- 📘 Build a PWA (bitsofco.de, all OK): [Part 1 Service worker](https://bitsofco.de/bitsofcode-pwa-part-1-offline-first-with-service-worker/), [Part 2 IndexedDB](https://bitsofco.de/bitsofcode-pwa-part-2-instant-loading-with-indexeddb/), [Part 3 Push notifications](https://bitsofco.de/bitsofcode-pwa-part-3-push-notifications/)
- 🛠 **Deliverable:** decide whether store staff alerts should be a PWA push, Zalo/Telegram, or email. Record the decision in the capstone spec.

### Week 11: Android (repo gap, so use official sources)
- ❌ Repo entry *Keddit* (2016, Kotlin + old Android APIs): skip it for an Android 13 target.
- ➕ Outside the repo: Google's official *Android Basics with Compose* course (developer.android.com).
- ⚠️ For your RFID tablet app, the Chainway SDK docs and demo app are the source of truth. **No SDK method is assumed here.**
- 🛠 **Deliverable:** an empty Android Studio project for Android 13 (API 33) that builds, plus a list of the SDK calls you need, copied from the official Chainway demo source.

### Week 12: IoT security pass + capstone review
- ➕ Outside the repo: OWASP IoT Top 10 (2018), and the OWASP IoT Security Verification Standard (ISVS).
- 🛠 **Deliverable:** run [[05-iot-security-checklist]] against one real store site, then update [[02-capstone-spec-ura4-store-monitor]].

---

## Progress tracker (copy into your daily note)

- [ ] W1 Shell + Bash spec
- [ ] W2 ARP/ICMP + Wireshark how-to
- [ ] W3 TCP + symptom table
- [ ] W4 Concurrency + Node diagram
- [ ] W5 MQTT topic tree
- [ ] W6 Node TDD API (part 1)
- [ ] W7 Node TDD API (part 2)
- [ ] W8 Docker compose on Pi
- [ ] W9 Next.js dashboard
- [ ] W10 PWA + alert-channel decision
- [ ] W11 Android 13 skeleton + SDK call list
- [ ] W12 Security pass + capstone review
