# Networking Fundamentals & Routing — Study Guide

> **Topic:** Networking Fundamentals (OSI, TCP/IP, UDP), Routing Protocols, and Switching Concepts
> **Your background:** Complete beginner — every term is explained from zero, with plain-English analogies.
> **Goal:** Interview prep — never go blank. Know the fundamentals + the concept behind every answer.
> **Style:** Deep dives with config examples (Cisco IOS style), Mermaid diagrams, memory hooks, and an interview question bank.

---

## How to use this guide

1. Read the Parts **in order** — each one builds on the last. Foundations first, then core protocols, then applied/switching, then interview drilling.
2. Each Part is its own file in the `prep/` folder. Don't rush — say **"next"** to unlock the next Part.
3. After reading, **say the answers aloud**. Reading builds knowledge; speaking builds readiness.
4. Use the **Question Bank** (Part L) to self-test, and the **Behavioral & Closing** Part (Part M) to prep the human side.

---

## 📚 Master Index

### Group 1 — Foundations (how networks even work)
- **Part A — Networking from Zero: What Is a Network?**
  1. Hosts, links, nodes, packets, bandwidth, latency
  2. LAN vs WAN vs MAN, the Internet vs an internet
  3. Clients, servers, peers; circuit vs packet switching
  4. Why we need "layers" (the big idea behind everything)

- **Part B — The OSI Model (the 7-layer mental map)**
  1. All 7 layers explained with analogies
  2. What lives at each layer (protocols, devices, data units)
  3. Encapsulation & de-encapsulation (headers as envelopes)
  4. PDUs: bits → frames → packets → segments → data

- **Part C — The TCP/IP Model (what the real Internet uses)**
  1. The 4 (or 5) layers and how they map to OSI
  2. Why TCP/IP won over OSI
  3. End-to-end data journey across the stack

### Group 2 — Core Protocols (the engines)
- **Part D — IP Addressing & Subnetting**
  1. IPv4 anatomy, classes, private vs public, CIDR
  2. Subnet masks, subnetting & VLSM (step-by-step math)
  3. IPv6 essentials (why it exists, address format)
  4. NAT, DHCP, ARP — how a host actually gets on the network

- **Part E — TCP vs UDP (the transport layer in depth)**
  1. Ports & sockets; multiplexing
  2. TCP: 3-way handshake, sequence/ACK, flow & congestion control
  3. UDP: connectionless speed; when to use which
  4. Common ports & protocols (HTTP, DNS, DHCP, etc.)

### Group 3 — Switching (Layer 2: the local neighborhood)
- **Part F — Switching Concepts**
  1. MAC addresses, frames, switch MAC-address tables
  2. Collision vs broadcast domains; hubs vs switches vs bridges
  3. VLANs, trunking (802.1Q), inter-VLAN routing
  4. Spanning Tree Protocol (STP/RSTP), EtherChannel — with configs

### Group 4 — Routing (Layer 3: connecting neighborhoods)
- **Part G — Routing Fundamentals**
  1. What a router does; the routing table & longest-prefix match
  2. Static vs dynamic routing; default routes
  3. Administrative distance & metrics
  4. Routed vs routing protocols

- **Part H — Distance-Vector Protocols: RIP & EIGRP**
  1. How distance-vector thinking works ("routing by rumor")
  2. RIP / RIPv2 — config, timers, limitations
  3. EIGRP — DUAL, feasible successors, metrics — with configs
  4. Loop-prevention: split horizon, poison reverse, hold-down

- **Part I — Link-State Protocols: OSPF**
  1. Link-state vs distance-vector thinking
  2. OSPF areas, LSAs, DR/BDR, the SPF (Dijkstra) algorithm
  3. OSPF neighbor states & configuration
  4. Single-area & multi-area OSPF — with configs

- **Part J — Path-Vector & The Internet's Glue: BGP**
  1. Autonomous Systems; IGP vs EGP
  2. eBGP vs iBGP; path attributes & best-path selection
  3. BGP neighbor states & basic config
  4. Why BGP runs the Internet

### Group 5 — Edge, Trends & Interview Drilling
- **Part K — Miscellaneous & Deeper Topics**
  1. Network security basics (ACLs, firewalls, VPNs, zero trust)
  2. QoS, NAT/PAT deeper, IPv6 routing, multicast
  3. SDN, network automation, cloud networking, current trends
  4. Troubleshooting methodology & key CLI tools (ping, traceroute, etc.)

- **Part L — Interview Question Bank (100+ Q&A)**
  1. Basic (~20%), Intermediate (~20%), Advanced (~60%)
  2. Behavioral (STAR) & Closing/"why" questions
  3. Self-quiz tracker

- **Part M — Behavioral & Closing Prep**
  1. STAR method + your-background-to-competency map
  2. Ready-to-adapt stories & "why" answers
  3. Smart questions to ask + night-before cheat sheet

---

## 🗂️ Parts Tracker

| Part | Title | File | Status |
|------|-------|------|--------|
| A | Networking from Zero | prep/Part-A-Networking-from-Zero.md | ✅ Done |
| B | The OSI Model | prep/Part-B-OSI-Model.md | ✅ Done |
| C | The TCP/IP Model | prep/Part-C-TCPIP-Model.md | ✅ Done |
| D | IP Addressing & Subnetting | prep/Part-D-IP-Addressing-Subnetting.md | ✅ Done |
| E | TCP vs UDP | prep/Part-E-TCP-vs-UDP.md | ✅ Done |
| F | Switching Concepts | prep/Part-F-Switching-Concepts.md | ✅ Done |
| G | Routing Fundamentals | prep/Part-G-Routing-Fundamentals.md | ✅ Done |
| H | Distance-Vector: RIP & EIGRP | prep/Part-H-Distance-Vector-RIP-EIGRP.md | ✅ Done |
| I | Link-State: OSPF | prep/Part-I-Link-State-OSPF.md | ✅ Done |
| J | Path-Vector: BGP | prep/Part-J-Path-Vector-BGP.md | ✅ Done |
| K | Miscellaneous & Deeper Topics | prep/Part-K-Misc-Deeper-Topics.md | ✅ Done |
| L | Interview Question Bank | prep/Part-L-Interview-Question-Bank.md | ✅ Done |
| M | Behavioral & Closing Prep | prep/Part-M-Behavioral-Closing.md | ✅ Done |

---

*Confirm or tweak this index, then say **"next"** (or name a Part) to build the first section.*
