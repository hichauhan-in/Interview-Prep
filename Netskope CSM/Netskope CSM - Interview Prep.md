# Netskope CSM — Interview Preparation

> Goal: For any likely question, never go blank. Know the fundamentals and the concept behind it.
> Candidate background: Support Escalation Engineer (OneDrive/SharePoint, Microsoft). Strong networking fundamentals, working knowledge of Active Directory.

---

## Master Index (What We Need to Cover)

### Part A — Role & Company Context
1. About Netskope (what they do, why they exist, the "perimeter dissolving" story)
2. What a Customer Success Manager (CSM) actually does
3. CSM vs TAM vs Support vs Sales Engineer — how the roles differ
4. The Customer Success lifecycle (onboarding → adoption → value → renewal → expansion)

### Part B — Security Fundamentals
5. Core security concepts (CIA triad, AAA, defense in depth, least privilege)
6. The modern threat landscape (phishing, malware, ransomware, insider threat, data exfiltration)
7. Cloud computing & cloud security basics (SaaS / PaaS / IaaS, shared responsibility model)

### Part C — The Big One: SASE & SSE
8. What is SASE (Secure Access Service Edge)
9. What is SSE (Security Service Edge) — the security half of SASE
10. The SSE pillars: SWG, CASB, ZTNA, FWaaS, DLP
11. Why the network perimeter dissolved (the core Netskope narrative)
12. Netskope platform overview (Netskope ONE, Intelligent SSE, NewEdge network)

### Part D — Networking (Your Strength — Tie It In)
13. TCP/IP, DNS, HTTP/HTTPS, TLS handshake refresher
14. Proxies: forward vs reverse proxy, TLS/SSL inspection
15. Traffic steering: Netskope Client, PAC files, IPsec/GRE tunnels
16. How OneDrive/SharePoint traffic would flow through Netskope

### Part E — Identity & Access (Your AD Knowledge — Tie It In)
17. Authentication vs Authorization
18. SSO, SAML, OAuth 2.0, OIDC, SCIM
19. Identity Providers (Entra ID / Azure AD) and Netskope integration
20. Zero Trust principles and ZTNA in depth

### Part F — Cloud Access Security Broker (CASB)
21. Shadow IT, sanctioned vs unsanctioned apps, Cloud Confidence Index
22. Inline (forward/reverse proxy) vs API-based (out-of-band) CASB
23. Real-world CASB use cases

### Part G — Data Loss Prevention (DLP) — Bonus Differentiator
24. DLP concepts and data classification
25. DLP detection techniques (regex, dictionaries, EDM, fingerprinting, ML)
26. DLP policy workflow and incident management

### Part H — Threat Protection
27. Malware scanning, sandboxing, Advanced Threat Protection
28. Remote Browser Isolation (RBI), Cloud Firewall

### Part I — Customer Success Craft (Process & Soft Skills)
29. Post-sales handoff & success planning
30. Onboarding & time-to-value (TTV)
31. Cadence calls, adoption monitoring, health scoring
32. Quarterly Business Reviews (QBRs)
33. Risk monitoring, churn prevention, success playbooks
34. Expansion & upsell (land-and-expand)
35. Stakeholder management (technical teams + executives)

### Part J — Metrics & Business Acumen
36. CSM KPIs: NRR, GRR, CSAT, NPS, adoption rate, TTV, churn

### Part K — Behavioral & Closing
37. STAR stories — mapping your Microsoft escalation experience to CSM competencies
38. Smart questions to ask the interviewer
39. Quick-reference glossary / cheat sheet

### Part L — Miscellaneous & Deeper Topics
- Competitive landscape, Gartner MQ, compliance frameworks, OSI model, UEBA, posture (SSPM/CSPM/DSPM), RBAC/ABAC, GenAI, real-world adoption friction

### Part M — Interview Question Bank (100+)
- 130+ practice questions with model answers: Basic, Intermediate, Advanced, Behavioral/Situational, and Closing

---

## Section Files
Each Part lives in its own file under the `prep/` folder for focused study.

| Part | File | Status |
|------|------|--------|
| A — Role & Company Context | [prep/Part-A-Role-and-Company-Context.md](prep/Part-A-Role-and-Company-Context.md) | ✅ Done |
| B — Security Fundamentals | [prep/Part-B-Security-Fundamentals.md](prep/Part-B-Security-Fundamentals.md) | ✅ Done |
| C — SASE & SSE | [prep/Part-C-SASE-and-SSE.md](prep/Part-C-SASE-and-SSE.md) | ✅ Done |
| D — Networking | [prep/Part-D-Networking.md](prep/Part-D-Networking.md) | ✅ Done |
| E — Identity & Access | [prep/Part-E-Identity-and-Access.md](prep/Part-E-Identity-and-Access.md) | ✅ Done |
| F — CASB | [prep/Part-F-CASB.md](prep/Part-F-CASB.md) | ✅ Done |
| G — DLP | [prep/Part-G-DLP.md](prep/Part-G-DLP.md) | ✅ Done |
| H — Threat Protection | [prep/Part-H-Threat-Protection.md](prep/Part-H-Threat-Protection.md) | ✅ Done |
| I — Customer Success Craft | [prep/Part-I-Customer-Success-Craft.md](prep/Part-I-Customer-Success-Craft.md) | ✅ Done |
| J — Metrics & Business Acumen | [prep/Part-J-Metrics.md](prep/Part-J-Metrics.md) | ✅ Done |
| K — Behavioral & Closing | [prep/Part-K-Behavioral-and-Closing.md](prep/Part-K-Behavioral-and-Closing.md) | ✅ Done |
| L — Miscellaneous & Deeper Topics | [prep/Part-L-Miscellaneous-Deep-Topics.md](prep/Part-L-Miscellaneous-Deep-Topics.md) | ✅ Done |
| M — Interview Question Bank (100+) | [prep/Part-M-Interview-Questions.md](prep/Part-M-Interview-Questions.md) | ✅ Done |


My honest recommendation — to actually be ready
Step	Why
1. Read A–M for understanding	Foundation ✅
2. Self-quiz with Part M (cover answer, say aloud)	Converts reading → recall
3. Write your 3–5 STAR stories with real details/numbers	Your differentiator
4. Do a mock interview (I can run one)	Pressure-test articulation + follow-ups
5. Research the specific role/team + prep your questions	Shows intent