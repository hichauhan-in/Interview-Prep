# Part L — Interview Question Bank (100+ Q&A)

> **Goal of this Part:** A drilling resource of 100+ questions across every topic, split **Basic / Intermediate / Advanced** plus **Behavioral** and **Closing**. Each has a concise model answer/hint and a cross-reference to the Part with full detail. Use the tracker at the end to self-test.
>
> **How to use:** Cover the answer, say yours **aloud**, then check. Mark ✅/🔁/❌ in the tracker. Re-drill anything not ✅.

---

## 🟢 BASIC (Q1–Q22) — fundamentals you must never miss

1. **What is a network?** Two+ devices connected to share data using protocols. *(Part A)*
2. **Bandwidth vs latency?** Bandwidth = capacity (lanes); latency = delay (travel time). *(Part A)*
3. **What is a packet?** A small chunk of data with addressing info; enables packet switching. *(Part A)*
4. **LAN vs WAN?** LAN = local (home/office); WAN = wide (the Internet is the largest). *(Part A)*
5. **Client vs server?** Client requests; server provides. *(Part A)*
6. **Name the 7 OSI layers.** Application, Presentation, Session, Transport, Network, Data Link, Physical. *(Part B)*
7. **What's at the Transport layer?** TCP/UDP, ports, end-to-end delivery. *(Part B/E)*
8. **What PDU is at the Network layer?** Packet. (Transport=segment, Data Link=frame, Physical=bits.) *(Part B)*
9. **Which device works at Layer 2 vs Layer 3?** Switch = L2 (MAC); Router = L3 (IP). *(Part B/F/G)*
10. **What are the TCP/IP model layers?** Application, Transport, Internet, Network Access. *(Part C)*
11. **What does DNS do?** Translates domain names to IP addresses. *(Part C)*
12. **What does DHCP do?** Automatically assigns IP config (DORA). *(Part C/D)*
13. **IPv4 address size and format?** 32 bits, four octets 0–255. *(Part D)*
14. **What are the private IP ranges?** 10/8, 172.16–31/12, 192.168/16. *(Part D)*
15. **What is a subnet mask for?** Marks network vs host bits. *(Part D)*
16. **TCP vs UDP in one line?** TCP = reliable/ordered; UDP = fast/best-effort. *(Part E)*
17. **What port is HTTPS / SSH / DNS?** 443 / 22 / 53. *(Part E)*
18. **What is a MAC address?** 48-bit physical address burned into the NIC, used locally. *(Part F)*
19. **Hub vs switch?** Hub repeats to all ports; switch forwards by MAC. *(Part F)*
20. **What is a VLAN?** A logical split of a switch into separate networks. *(Part F)*
21. **What does a router do?** Connects different networks and forwards by IP via a routing table. *(Part G)*
22. **What is a default route?** 0.0.0.0/0 — used when no specific route matches. *(Part G)*

---

## 🟡 INTERMEDIATE (Q23–Q44) — connecting the concepts

23. **Explain encapsulation.** Each layer adds a header: Data→Segment→Packet→Frame→Bits. *(Part B)*
24. **Why did TCP/IP win over OSI?** Shipped first, free, simpler, gov/university backing. *(Part C)*
25. **Walk through typing a URL and pressing Enter.** DNS → TCP handshake → encapsulate → route → server replies → render. *(Part C)*
26. **What is ARP?** Resolves a known IP to a MAC on the local network. *(Part C/D)*
27. **Usable hosts in a /26?** 62 (2⁶−2). *(Part D)*
28. **Subnet 192.168.1.0/24 into 4.** /26, blocks of 64: .0/.64/.128/.192. *(Part D)*
29. **Why subtract 2 for usable hosts?** Network and broadcast addresses are reserved. *(Part D)*
30. **What is NAT/PAT?** Many private IPs share a public IP, distinguished by ports. *(Part D)*
31. **Explain the TCP 3-way handshake.** SYN → SYN-ACK → ACK. *(Part E)*
32. **What is a socket?** IP address + port number. *(Part E)*
33. **When choose UDP?** Voice/video/gaming/DNS — speed over reliability. *(Part E)*
34. **Collision vs broadcast domain?** Switch port = own collision domain; router/VLAN splits broadcast domains. *(Part F)*
35. **What is 802.1Q trunking?** Tags frames with VLAN IDs to carry multiple VLANs on one link. *(Part F)*
36. **What is inter-VLAN routing?** Router/L3 switch routes between VLANs (router-on-a-stick). *(Part F)*
37. **What problem does STP solve?** Layer-2 loops/broadcast storms; blocks redundant paths. *(Part F)*
38. **Static vs dynamic routing?** Static = manual/precise; dynamic = auto-adapting via protocols. *(Part G)*
39. **What is administrative distance?** Trust rank between route sources; lower wins. *(Part G)*
40. **AD vs metric?** AD picks between protocols; metric picks paths within a protocol. *(Part G)*
41. **What metric does each protocol use?** RIP=hops, OSPF=cost, EIGRP=bw+delay, BGP=attributes. *(Part G)*
42. **IGP vs EGP?** IGP inside an AS (OSPF/EIGRP); EGP between ASes (BGP). *(Part G/J)*
43. **What is split horizon?** Don't advertise a route back out the interface you learned it on. *(Part H)*
44. **RIP max hop count and why?** 15 (16=unreachable) — loop safety valve. *(Part H)*

---

## 🔴 ADVANCED (Q45–Q104) — depth that impresses

45. **At which OSI layer is TLS/encryption?** Presentation (6), conceptually. *(Part B)*
46. **How does longest-prefix match work?** Most specific route (longest prefix) wins, regardless of metric/AD. *(Part G)*
47. **What happens if a router finds no matching route?** Uses default route, else drops the packet. *(Part G)*
48. **Explain TCP flow control vs congestion control.** Flow = don't flood receiver (window); congestion = don't flood network (slow start). *(Part E)*
49. **What is the TCP sliding window?** Receiver-advertised amount of unACKed data the sender may have in flight. *(Part E)*
50. **How does TCP guarantee reliability?** Sequence numbers, ACKs, retransmission, checksums, flow/congestion control. *(Part E)*
51. **Describe the TCP 4-way close.** FIN → ACK → FIN → ACK (each side closes independently). *(Part E)*
52. **What is VLSM?** Variable-length masks — subnetting subnets to fit different sizes; allocate largest first. *(Part D)*
53. **IPv4 vs IPv6 key differences.** 32 vs 128-bit; decimal vs hex; IPv6 has no broadcast, no NAT needed. *(Part D)*
54. **How does a switch build its MAC table?** Learns source MAC + port; forwards known, floods unknown. *(Part F)*
55. **How is the STP root bridge elected?** Lowest Bridge ID (priority, then MAC). *(Part F)*
56. **STP vs RSTP?** RSTP (802.1w) converges in seconds vs STP's 30–50s. *(Part F)*
57. **Name STP port states.** Blocking, Listening, Learning, Forwarding (Disabled). *(Part F)*
58. **What is EtherChannel and LACP?** Bundles links into one logical link; LACP is the open negotiation protocol. *(Part F)*
59. **What is a native VLAN?** The untagged VLAN on an 802.1Q trunk (default 1). *(Part F)*
60. **Distance-vector vs link-state — core difference?** DV trusts neighbor summaries; LS builds a full map and runs SPF. *(Part H/I)*
61. **What is EIGRP DUAL?** The algorithm guaranteeing loop-free paths via successors/feasible successors. *(Part H)*
62. **Successor vs feasible successor?** Successor = best route; FS = pre-computed loop-free backup for instant failover. *(Part H)*
63. **What three tables does EIGRP keep?** Neighbor, topology, routing. *(Part H)*
64. **Why is EIGRP called hybrid?** Distance-vector base + link-state features (neighbors, triggered updates). *(Part H)*
65. **List distance-vector loop-prevention methods.** Split horizon, route poisoning, poison reverse, hold-down, triggered updates, max hop. *(Part H)*
66. **What algorithm does OSPF use?** Dijkstra's SPF. *(Part I)*
67. **How is OSPF cost calculated?** Reference bandwidth ÷ link bandwidth. *(Part I)*
68. **Why does OSPF use areas?** Scalability — localize detail, summarize across borders; all connect to Area 0. *(Part I)*
69. **What are ABR and ASBR?** ABR connects an area to the backbone; ASBR connects OSPF to external networks. *(Part I)*
70. **What is an LSA and LSDB?** LSA = link description; LSDB = full map (identical per area). *(Part I)*
71. **Why DR/BDR?** Reduce flooding on multi-access networks; central + backup update points. *(Part I)*
72. **How is the DR elected?** Highest OSPF priority, then highest Router ID. *(Part I)*
73. **How is OSPF Router ID chosen?** Highest loopback IP, else highest active interface IP, else manual. *(Part I)*
74. **Name OSPF neighbor states.** Down, Init, 2-Way, ExStart, Exchange, Loading, Full. *(Part I)*
75. **Why might OSPF neighbors stick at 2-Way?** DROthers only form full adjacency with DR/BDR, not each other. *(Part I)*
76. **What is BGP and why is it path-vector?** Internet EGP that tracks the full AS-Path, not just hops. *(Part J)*
77. **eBGP vs iBGP and their ADs?** eBGP between ASes (20); iBGP within an AS (200). *(Part J)*
78. **What transport/port does BGP use?** TCP port 179 (reliable). *(Part J)*
79. **How does BGP prevent loops?** Rejects routes containing its own ASN in the AS-Path. *(Part J)*
80. **Key BGP path attributes in order?** Weight, Local Preference, AS-Path, Origin, MED... *(Part J)*
81. **Local Preference vs Weight?** Weight is Cisco-local to one router; Local Pref is shared across the AS (both: higher wins). *(Part J)*
82. **What is MED?** A hint to a neighbor AS for the preferred entry point (lower wins). *(Part J)*
83. **Name BGP neighbor states.** Idle, Connect, Active, OpenSent, OpenConfirm, Established. *(Part J)*
84. **Why is BGP policy-based, not shortest-path?** Internet routing reflects business/peering agreements, not just speed. *(Part J)*
85. **Standard vs extended ACL?** Standard = source IP only; extended = source/dest/protocol/port. *(Part K)*
86. **Stateless vs stateful firewall?** Stateless = per-packet; stateful = tracks connections. *(Part K)*
87. **IPsec vs SSL VPN?** IPsec = site-to-site/secure tunnels; SSL/TLS = browser-based remote access. *(Part K)*
88. **What is QoS and its mechanisms?** Prioritize traffic via classification, queuing, policing, shaping. *(Part K)*
89. **Unicast vs multicast vs anycast?** One-to-one / one-to-group / one-to-nearest. *(Part K)*
90. **What is SDN; control vs data plane?** Centralizes control plane (decisions) apart from data plane (forwarding). *(Part K)*
91. **What is Zero Trust?** Never trust, always verify — authenticate every request. *(Part K)*
92. **How do you troubleshoot "no Internet"?** Ping up the stack; 8.8.8.8 works but names fail = DNS. *(Part K)*
93. **What does traceroute reveal?** Each hop and per-hop latency to locate breaks. *(Part K)*
94. **IDS vs IPS?** IDS detects/alerts; IPS detects and blocks inline. *(Part K)*
95. **What is port security?** Limits MACs per switch port to stop rogue devices. *(Part K)*
96. **Why does TCP need a handshake but UDP doesn't?** TCP establishes state (seq numbers, reliability); UDP is stateless. *(Part E)*
97. **What is a broadcast storm?** Endless looping Layer-2 broadcasts (no TTL) — prevented by STP. *(Part F)*
98. **Compare OSPF vs EIGRP.** OSPF = open link-state, areas, cost; EIGRP = Cisco hybrid, DUAL, bw+delay. *(Part I)*
99. **Why is there no TTL at Layer 2?** Ethernet frames have no hop limit — why loops are catastrophic and STP exists. *(Part F)*
100. **What is convergence?** The time for all routers to agree on the topology after a change; faster = better. *(Part G/H/I)*
101. **What is route summarization/aggregation?** Combining multiple routes into one advertisement to shrink tables. *(Part I/J)*
102. **Why run an IGP and BGP together?** IGP handles internal best paths; BGP handles inter-AS policy/reachability. *(Part J)*
103. **What is asymmetric routing?** Traffic takes a different path back than it came — can break stateful firewalls. *(Part K)*
104. **What is MTU and fragmentation?** MTU = max frame size; oversized packets are fragmented or dropped (PMTUD). *(Part D/E)*

---

## 💬 BEHAVIORAL (STAR) (Q105–Q112)

> Use **STAR**: Situation, Task, Action, Result. Full prep in [Part M](Part-M-Behavioral-Closing.md).

105. **Tell me about a time you solved a difficult technical problem.** *(STAR: pick a real debugging story — what broke, what you tried, the fix, the measurable outcome.)*
106. **Describe a time you had to learn something new quickly.** *(Show a learning method + result — perfect for a career-changer.)*
107. **Tell me about a time you worked in a team / handled conflict.** *(Focus on communication and a positive resolution.)*
108. **Describe a time you made a mistake. How did you handle it?** *(Honesty + what you changed afterward.)*
109. **How do you handle pressure or tight deadlines?** *(Concrete prioritization example.)*
110. **Give an example of explaining something technical to a non-technical person.** *(Great for showing communication skill.)*
111. **Tell me about a time you took initiative.** *(Ownership and impact.)*
112. **Describe a time you disagreed with a decision.** *(Respectful pushback + outcome.)*

---

## 🎯 CLOSING / "WHY" (Q113–Q118)

113. **Why do you want this role?** *(Tie your goals to the role's responsibilities.)*
114. **Why networking / why this company?** *(Show genuine research + fit.)*
115. **Where do you see yourself in 3–5 years?** *(Growth aligned to the team.)*
116. **What are your strengths and weaknesses?** *(Real weakness + active fix.)*
117. **Why should we hire you?** *(2–3 differentiators mapped to their needs.)*
118. **Do you have any questions for us?** *(Always yes — see Part M for a strong list.)*

---

## 🗂️ Self-Quiz Tracker

> Legend: ✅ confident · 🔁 review · ❌ relearn. Re-drill anything not ✅ until it is.

| Range | Topic focus | Round 1 | Round 2 | Round 3 |
|-------|-------------|---------|---------|---------|
| Q1–Q22 | Basics | ⬜ | ⬜ | ⬜ |
| Q23–Q44 | Intermediate | ⬜ | ⬜ | ⬜ |
| Q45–Q60 | Advanced: transport/switching | ⬜ | ⬜ | ⬜ |
| Q61–Q75 | Advanced: EIGRP/OSPF | ⬜ | ⬜ | ⬜ |
| Q76–Q84 | Advanced: BGP | ⬜ | ⬜ | ⬜ |
| Q85–Q104 | Advanced: security/QoS/misc | ⬜ | ⬜ | ⬜ |
| Q105–Q112 | Behavioral (STAR) | ⬜ | ⬜ | ⬜ |
| Q113–Q118 | Closing/"why" | ⬜ | ⬜ | ⬜ |

**Target before interview day:** every range at ✅ for two consecutive rounds, said **aloud**.

---

➡️ **Final Part:** [Part M — Behavioral & Closing Prep](Part-M-Behavioral-Closing.md) — STAR stories, "why" answers, smart questions, and a night-before cheat sheet.
