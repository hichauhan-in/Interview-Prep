# Part O — Miscellaneous & Deeper Topics (the extra edge)

> **Section goal:** Everything that makes you sound like someone who lives in this industry rather than someone who revised for an interview: the competitive landscape, the standards and compliance vocabulary, what's happening right now, the adjacent products you'll bump into, Microsoft's culture, and a complete glossary.

Covers index items **125–130**. Maps to JD: *"thirst for knowledge"*, *"Passion for customers"*, *"Excellent written and oral communication skills"*, and the general expectation that a senior hire understands the market.

---

## 125. The competitive landscape

You will not be asked to sell against competitors, but knowing them shows industry awareness and helps you understand *why* customers ask for certain things.

| Competitor | Strength | Where Intune wins | Where they win |
|---|---|---|---|
| **Jamf** (Jamf Pro / Jamf School / Jamf Now) | The Apple specialist; deep, fast adoption of new Apple APIs | Single console for all platforms; identity + security + compliance integration; licensing bundled in M365 | Depth and speed on Apple-only estates; Apple-native admin experience |
| **Omnissa Workspace ONE** (formerly VMware Workspace ONE / AirWatch) | Long-standing UEM with strong rugged/industrial and virtualization heritage | Cloud-native, Entra/Defender/Purview integration, Microsoft 365 bundling | Complex mixed estates, on-prem options, some vertical/rugged features |
| **Ivanti Neurons for UEM** (formerly MobileIron) | Mature MDM, strong in regulated verticals | Modern cloud architecture, ecosystem integration | Legacy estate support, specific vertical features |
| **Kandji, Mosyle, Addigy** | Modern Apple-focused MDMs with automation-first design | Cross-platform breadth | Apple-only shops wanting slick automation |
| **Google Workspace / Chrome Enterprise** | ChromeOS and Android management | Windows depth and enterprise security stack | ChromeOS estates, education |
| **ManageEngine, NinjaOne, Automox, Tanium** | Patch/endpoint management niches | Integrated identity and compliance story | Speed of patching, real-time query, SMB pricing |
| **Configuration Manager (ConfigMgr)** — Microsoft's own | Unmatched on-prem depth and OS deployment | Cloud reach, cross-platform, no infrastructure | Deep on-prem and complex application delivery |

**What customers actually cite when they choose Intune:** it's already licensed with Microsoft 365; the Entra + Defender + Purview integration is native; no infrastructure to run; and Conditional Access is a genuinely differentiating control point.

**What customers cite when they push back:** Apple feature parity lag versus Jamf; reporting latency and depth; the packaging burden for Win32 apps (which is why Enterprise App Management matters); and complexity when co-existing with ConfigMgr.

> 💡 **How to use this in an interview:** don't trash competitors. Say something like *"Jamf is genuinely excellent on Apple and adopts new Apple APIs fast — where Intune wins is the single control plane across platforms with identity and threat signal built in. When a customer raises a Jamf comparison, my job is to understand which specific capability they need rather than to argue."* That answer sounds like someone senior.

---

## 126. Standards, compliance and regulated environments

### Frameworks you should recognise

| Framework | What it is | Endpoint relevance |
|---|---|---|
| **Zero Trust** | Verify explicitly, least privilege, assume breach | The organising principle ([Part A](Part-A-cloud-and-modern-management.md)) |
| **NIST CSF / NIST 800-53 / 800-171** | US standards for cybersecurity framework and controls | Compliance policies and baselines map to controls |
| **CIS Benchmarks / CIS Controls** | Community-agreed hardening configurations | Customers often demand CIS-aligned baselines |
| **DISA STIG** | US Department of Defense hardening standards | Defence and government customers |
| **ISO 27001 / 27017 / 27018** | Information security management standards | Certification customers require of Microsoft |
| **SOC 1 / 2 / 3** | Audit reports on controls | Provided by Microsoft; customers ask for them |
| **PCI DSS** | Payment card security | Retail customers; endpoint controls in scope |
| **HIPAA / HITRUST** | US healthcare | Healthcare customers |
| **GDPR** | EU data protection | Data residency, subject rights, what MDM may collect |
| **FedRAMP** | US federal cloud authorization | GCC / GCC High / DoD environments |
| **NIS2 / DORA** | EU cyber-resilience and financial-sector regulation | Increasing enterprise demand for provable endpoint controls |
| **CMMC** | US defence supply-chain maturity certification | Defence suppliers |
| **Essential Eight** | Australian Signals Directorate mitigation strategies | APAC public sector; maps neatly to ASR, patching, admin rights |

### 🔍 Plain-English deep-dive: sovereign and national clouds

- **What they are:** physically and logically separate instances of Microsoft's cloud for specific jurisdictions or sensitivity levels — **GCC**, **GCC High**, **DoD** (US government), and **21Vianet-operated China**.
- **Analogy:** the same restaurant chain, but a completely separate kitchen, staff and supply chain for a different country's regulations. The menu is *similar*, not identical.
- **Why it matters for support:** endpoints and URLs differ (e.g. `.us` domains), **feature parity lags commercial**, some capabilities never arrive, documentation differs, and guidance that is correct in commercial can be flatly wrong there. **"Is this commercial or GCC High?" is a triage question worth asking early**, and asking it unprompted signals real experience.

### Privacy in device management

Worth being able to articulate clearly, because it comes up with customers, works councils and end users:

- MDM **can** see: device model, OS version, serial/IMEI (corporate), compliance state, managed app inventory, network/carrier info, storage, and — on corporate-owned devices — full app inventory and (where configured) location for lost-device scenarios.
- MDM **cannot** see: personal email content, SMS, photos, browsing history, calling history, or personal app data.
- On **BYOD**, app inventory is limited to managed apps, and MAM sees only corporate data inside managed apps.
- **Selective wipe / Retire** removes corporate data only.
- Being able to state this precisely builds enormous trust — and vagueness here destroys it.

---

## 127. Current trends and what's happening now

*(Dated: this section ages fastest. Refresh it in the week before any interview.)*

| Trend | What's happening | Why it matters to this role |
|---|---|---|
| **Windows 10 end of support and ESU** | Windows 10 reached end of support in October 2025; **Extended Security Updates** are available as a paid bridge for consumers and organizations | Enormous migration workload; a huge driver of Autopilot, Windows 365, hardware refresh and support volume |
| **Windows 11 adoption** | TPM 2.0 and hardware requirements, feature updates on an annual cadence | Compatibility, safeguard holds, and update-ring design questions |
| **Windows 365 / Cloud PC and Azure Virtual Desktop** | Cloud PCs are Entra-joined and Intune-managed like physical devices; Windows 365 Frontline for shift workers; Windows App as the unified client | An answer to unsupported hardware and to BYOD; a growing support surface |
| **Windows Autopatch** | Microsoft-managed update rings, progressive rollout and regression handling, now broadly available to eligible licences | Shifts update operations from the customer to Microsoft — and shifts the support conversation with it |
| **Intune Suite maturity** | EPM, Remote Help, Cloud PKI, Enterprise App Management, Advanced Analytics, Tunnel for MAM | Each maps to a known support cost ([Part G](Part-G-endpoint-security.md)) |
| **Microsoft Cloud PKI** | Cloud-hosted CA removing NDES dependence | Eliminates one of the most fragile infrastructure pieces in Intune |
| **Enterprise App Management** | Microsoft-curated catalog with managed packaging *and patching* | Attacks the packaging treadmill and third-party patch debt |
| **Declarative Device Management (Apple)** | Apple steadily moving management to declarations with proactive status | Better scale and diagnosability; changes how updates are enforced |
| **Android device administrator retirement** | Google's deprecation of the legacy API is being completed | Live migration projects in many tenants |
| **Passwordless and phishing-resistant auth** | Passkeys, Windows Hello for Business, FIDO2, Conditional Access authentication strength; passwordless-by-default direction | Changes enrollment and recovery flows; TAP becomes important |
| **Security Copilot and AI in security/management** | Domain assistants, promptbooks and agents across the Microsoft security stack | Directly relevant to the "Agentic" mission ([Part N](Part-N-ai-and-agentic-support.md)) |
| **MCP and agent interoperability** | Open standards for exposing tools and data to models | How support tooling gets connected to agents |
| **Entra Internet Access / Private Access (Global Secure Access)** | Microsoft's SSE offering replacing VPN patterns | Changes the network path — and therefore the troubleshooting path |
| **Endpoint Privilege Management adoption** | Removing local admin at scale becoming mainstream | Big reduction in a whole class of security incidents |
| **Sustainability and device lifecycle** | Longer refresh cycles, reuse, energy reporting | Shows up in customer conversations and in Endpoint Analytics |
| **Regulatory pressure (NIS2, DORA, CMMC)** | More organizations must *prove* endpoint control | Compliance reporting demand grows |

> 💡 **How to use trends in an interview:** tie a trend to a support consequence. *"Windows 10 end of support means a wave of migrations, which means a wave of Autopilot and ESP cases, which means the ESP blocking-app design conversation is going to be the top support driver for a lot of customers this year."* That's analysis, not recitation.

---

## 128. Adjacent products you'll bump into

| Product | What it does | Where it touches Intune |
|---|---|---|
| **Microsoft Entra ID Governance** | Access reviews, entitlement management, lifecycle workflows | Group membership that drives Intune assignment |
| **Entra Private Access / Internet Access** | Zero Trust network access replacing VPN | Changes the device's network path to Intune and to internal apps |
| **Microsoft Defender XDR** | Unified incident view across endpoint, identity, email, cloud apps | Where an endpoint alert becomes an incident |
| **Microsoft Sentinel** | Cloud SIEM/SOAR | Where Intune and Defender logs go for correlation and automation |
| **Microsoft Purview** | DLP (including endpoint DLP), sensitivity labels, insider risk, eDiscovery, audit | Endpoint DLP relies on managed, healthy devices |
| **Configuration Manager** | On-prem management; co-management and tenant attach | Workload ownership questions ([Part E](Part-E-configuration-and-compliance.md)) |
| **Windows Autopatch / Windows Update for Business reports** | Update management and reporting | Update workload |
| **Microsoft 365 Apps admin center** | Office servicing, inventory, add-in readiness | Office deployment issues |
| **Microsoft Store for Business (retired) → new Store / WinGet** | App sourcing | App deployment paths |
| **Power BI / Fabric** | Reporting on exported Intune and Log Analytics data | Customer executive reporting |
| **Microsoft Graph PowerShell SDK / Graph Explorer** | Automation and verification | Daily support tooling |
| **Azure Monitor / Log Analytics** | Diagnostic data destination and alerting | Evidence layer ([Part I](Part-I-troubleshooting-and-diagnostics.md)) |
| **Windows 365 / AVD** | Cloud desktops | Managed as endpoints |
| **Microsoft Teams / Surface Hub / Teams Rooms** | Meeting-room devices | Managed through Intune with specific profiles |
| **HoloLens / Meta Quest / frontline devices** | Specialty devices | Specialized device management |

---

## 129. Microsoft culture and how to talk about it

The interview loop will assess culture fit explicitly. These are the concepts and the language.

### The core cultural concepts

| Concept | What it means | How to demonstrate it |
|---|---|---|
| **Growth mindset** | "Learn-it-all, not know-it-all." Ability is developed, not fixed; failure is information | Tell a story where you were wrong, what you learned, and what you did differently afterwards |
| **Customer obsession** | Start from the customer's need, not the product's capability | Frame technical answers in terms of customer outcome |
| **Diverse and inclusive** | Seek different perspectives; make everyone able to contribute | Describe how you got a quiet person's input, or adapted communication to an audience |
| **One Microsoft** | Work across boundaries rather than optimising your own team | Cross-team escalation and joint calls ([Part L](Part-L-support-process-and-voc.md)) |
| **Make a difference** | Impact over activity | Quantify outcomes, not effort |
| **Model, Coach, Care** | The manager framework: model the behaviour, coach for growth, care about people | Relevant even as an IC — mentoring, enablement, TSG writing |
| **Disagree and commit** | Argue fully, then back the decision | Have a story about this |
| **Security is job zero** | Security takes priority over features | Show that you'd stop a rollout for a security risk |
| **Accessibility** | Products and communications usable by everyone | Mention it in design-review context |

### The three questions culture-fit answers must survive

1. **Did you make it about the customer, or about you?**
2. **Did you learn something, or just succeed?**
3. **Did you make others better, or just yourself look good?**

> 💡 **The growth-mindset trap:** don't tell a "failure" story where you were secretly right all along. A real growth-mindset story has a genuine mistake, a real cost, an honest reflection, and a changed behaviour that you can point to afterwards. Interviewers can tell the difference instantly.

---

## 130. Complete glossary

*(Every acronym used across this guide, in one place. Use this as a final-hour revision sheet.)*

### Identity and access
| Term | Meaning |
|---|---|
| **AAD** | Azure Active Directory — now **Microsoft Entra ID** |
| **AD DS** | Active Directory Domain Services (on-premises directory) |
| **AU** | Administrative Unit — Entra scoping container |
| **B2B / B2C** | External collaboration / customer identity |
| **CA** | Conditional Access (in identity context) or Certificate Authority (in PKI context) — **watch the ambiguity** |
| **CAE** | Continuous Access Evaluation — near-real-time token revocation |
| **FIDO2** | Phishing-resistant hardware/passkey authentication standard |
| **GDAP** | Granular Delegated Admin Privileges (partner access) |
| **IdP** | Identity Provider |
| **MFA** | Multi-Factor Authentication |
| **OIDC** | OpenID Connect — authentication layer over OAuth 2.0 |
| **PIM** | Privileged Identity Management — just-in-time role activation |
| **PRT** | Primary Refresh Token — device+user bound token enabling SSO and device claims |
| **RBAC** | Role-Based Access Control |
| **SAML** | Legacy XML federation standard |
| **SCP** | Service Connection Point (used for hybrid join discovery) |
| **SSO** | Single Sign-On |
| **TAP** | Temporary Access Pass |
| **TPM** | Trusted Platform Module — hardware key storage |
| **UPN** | User Principal Name — sign-in name in email format |
| **WHfB** | Windows Hello for Business |

### Device management
| Term | Meaning |
|---|---|
| **ABM / ASM** | Apple Business Manager / Apple School Manager |
| **ADE** | Automated Device Enrollment (Apple; formerly DEP) |
| **ADMX** | Group Policy administrative template format |
| **AOSP** | Android Open Source Project (Android without Google services) |
| **APNs** | Apple Push Notification service |
| **APP** | App Protection Policy (Intune MAM) |
| **ASU** | Account Scale Unit — the service slice hosting a tenant |
| **BYOD** | Bring Your Own Device |
| **COBO / COPE / COSU / CYOD** | Corporate-Owned Business-Only / Personally-Enabled / Single-Use (dedicated) / Choose Your Own Device |
| **ConfigMgr / SCCM / MECM** | Microsoft Configuration Manager |
| **CSP** | Configuration Service Provider — on-device component implementing settings |
| **DDM** | Declarative Device Management (Apple) |
| **DEM** | Device Enrollment Manager |
| **DEP** | Device Enrollment Program (Apple; now ADE) |
| **DO** | Delivery Optimization |
| **DPC** | Device Policy Controller (Android) |
| **EMM** | Enterprise Mobility Management |
| **ESP** | Enrollment Status Page |
| **FCM** | Firebase Cloud Messaging (Android push) |
| **GMS** | Google Mobile Services |
| **GPO** | Group Policy Object |
| **IME** | Intune Management Extension |
| **LOB** | Line-of-Business (app) |
| **MAM / MAM-WE** | Mobile Application Management / without enrollment |
| **MDM** | Mobile Device Management |
| **OEMConfig** | Standard for OEM-specific Android settings |
| **OMA-DM** | Open Mobile Alliance Device Management protocol |
| **OMA-URI** | Path addressing a specific CSP setting |
| **OOBE** | Out-of-Box Experience (Windows setup) |
| **PPKG** | Provisioning package |
| **SyncML** | XML message format used by OMA-DM |
| **UEM** | Unified Endpoint Management |
| **VPP** | Volume Purchase Program (Apple Apps and Books) |
| **WNS** | Windows Push Notification Service |
| **WUfB** | Windows Update for Business |
| **ZTD** | Zero-Touch Deployment |

### Security
| Term | Meaning |
|---|---|
| **AIR** | Automated Investigation and Response (Defender) |
| **ASR** | Attack Surface Reduction |
| **EDR** | Endpoint Detection and Response |
| **EPM** | Endpoint Privilege Management (Intune Suite) |
| **EPP** | Endpoint Protection Platform |
| **HVCI** | Hypervisor-enforced Code Integrity (memory integrity) |
| **LAPS** | Local Administrator Password Solution |
| **MDE** | Microsoft Defender for Endpoint |
| **NDES** | Network Device Enrollment Service (SCEP on Windows Server) |
| **PDE** | Personal Data Encryption |
| **PKCS** | Public Key Cryptography Standards (certificate delivery method) |
| **PUA** | Potentially Unwanted Application |
| **SCEP** | Simple Certificate Enrollment Protocol |
| **SIEM / SOAR** | Security Information & Event Management / Security Orchestration, Automation and Response |
| **SSE / SASE** | Secure Service Edge / Secure Access Service Edge |
| **TVM** | Threat and Vulnerability Management |
| **WDAC** | Windows Defender Application Control (App Control for Business) |
| **XDR** | Extended Detection and Response |

### Networking
| Term | Meaning |
|---|---|
| **BITS** | Background Intelligent Transfer Service |
| **CDN** | Content Delivery Network |
| **CRL / OCSP** | Certificate Revocation List / Online Certificate Status Protocol |
| **DoH / DoT** | DNS over HTTPS / over TLS |
| **NCSI** | Network Connectivity Status Indicator (Windows connectivity detection) |
| **NTP** | Network Time Protocol |
| **PAC / WPAD** | Proxy Auto-Config file / Web Proxy Auto-Discovery |
| **RST** | TCP reset — a connection actively refused |
| **SNI** | Server Name Indication (hostname in the TLS ClientHello) |
| **TLS** | Transport Layer Security |
| **WinHTTP / WinINET** | System-context / user-context HTTP stacks on Windows |

### Service operations
| Term | Meaning |
|---|---|
| **AHT** | Average Handle Time |
| **CAB** | Change Advisory Board (also: a `.cab` archive file — context matters) |
| **CSAT / NSAT / DSAT** | Customer Satisfaction / Net Satisfaction / Dissatisfaction |
| **DCR** | Design Change Request |
| **DRI** | Directly Responsible Individual |
| **ICM** | Incident Manager (Microsoft's incident system) |
| **ITIL** | IT service-management framework |
| **KB** | Knowledge Base article |
| **MTTD / MTTM / MTTR** | Mean Time To Detect / Mitigate / Resolve |
| **RCA** | Root Cause Analysis |
| **SDP** | Safe Deployment Practice |
| **SLA / SLO / SLI** | Service Level Agreement / Objective / Indicator |
| **TSG** | Troubleshooting Guide |
| **TTR / TTM** | Time To Resolve / Mitigate |
| **VoC** | Voice of the Customer |

### Engineering and AI
| Term | Meaning |
|---|---|
| **ADO** | Azure DevOps |
| **CI/CD** | Continuous Integration / Continuous Delivery |
| **DoD** | Definition of Done (also: US Department of Defense cloud — context matters) |
| **HITL** | Human-in-the-Loop |
| **KQL** | Kusto Query Language |
| **LLM / SLM** | Large / Small Language Model |
| **MCP** | Model Context Protocol |
| **PR** | Pull Request |
| **RAG** | Retrieval-Augmented Generation |
| **ReAct** | Reason–Act–Observe agent loop |
| **SDLC** | Software Development Life Cycle |
| **WIP** | Work In Progress (Kanban limit) |

### Microsoft organizational
| Term | Meaning |
|---|---|
| **CSAM** | Customer Success Account Manager |
| **CSS** | Customer Service and Support |
| **CVC** | Customer Value Creation (this role's org) |
| **GA** | General Availability |
| **GCC / GCC High / DoD** | US government cloud environments |
| **MCS** | Mission Critical Support |
| **PG** | Product Group (engineering) |
| **TAP** | Technology Adoption Program (also: Temporary Access Pass — context matters) |

---

## ⭐ Likely Interview Questions for This Section

**Q1. "How does Intune compare to Jamf or Workspace ONE?"**
> *Model answer:* "Jamf is genuinely excellent on Apple — it's Apple-only by design and it tends to adopt new Apple APIs very quickly, so Apple-heavy organizations often prefer its depth and admin experience. Workspace ONE has a long UEM heritage and is strong in complex mixed and rugged estates. Where Intune wins is being a single control plane across Windows, Apple, Android and Linux, with native integration into Entra Conditional Access, Defender risk signals and Purview data protection, no infrastructure to run, and licensing that's usually already owned via Microsoft 365. Where customers push back is Apple feature parity timing, reporting depth and latency, and the Win32 packaging burden — which is exactly why Enterprise App Management exists. When a customer raises a competitor, I don't argue; I ask which specific capability they need, because that's usually a concrete gap I can either solve, work around, or feed back as product input."

**Q2. "What's different about supporting a GCC High customer?"**
> *Model answer:* "It's a physically and logically separate cloud instance, so several things I'd take for granted are wrong. The endpoints and URLs differ, often with `.us` domains, so allow-lists and troubleshooting guidance from commercial don't transfer. Feature parity lags — capabilities arrive later and some never arrive at all — so 'this works in commercial' isn't evidence of a bug. Documentation and Message Center content differ. And there are extra constraints on data handling, personnel and support tooling. Practically, my first triage question with any government customer is 'commercial, GCC, GCC High or DoD?', because getting that wrong sends the whole investigation down the wrong path, and because it changes what I'm even allowed to ask them to send me."

**Q3. "What are the biggest trends affecting Intune customers right now?"**
> *Model answer:* "The Windows 10 end-of-support wave is the dominant one — it drives hardware refresh, Autopilot deployments, Windows 365 as an option for unsupported hardware, and ESU as a paid bridge, and all of that generates support volume in exactly the areas that are hardest: enrollment and provisioning. Alongside that, Windows Autopatch is shifting update operations from customers to Microsoft, which changes both the failure modes and who owns them. The Intune Suite is maturing into a set of features that each attack a known support cost — Cloud PKI removing NDES fragility and Enterprise App Management attacking the packaging treadmill are the two I'd call out. On mobile, Apple is steadily moving to Declarative Device Management and Google is finishing the retirement of Android device administrator, so there are live migration workloads on both platforms. And across everything, passwordless and phishing-resistant authentication is changing enrollment and recovery flows. I'd tie each of those to a support consequence rather than just listing them — that's the analysis that's actually useful."

**Q4. "What can and can't IT see on a personal device?"**
> *Model answer:* "I'd be precise, because vagueness here destroys trust. On an enrolled personal device, IT can see the device model, OS version, whether it's compliant, and the inventory of *managed* apps — not personal apps. On Android with a personally-owned work profile, management is limited to the work profile and a wipe removes only that container. On iOS with user enrollment, work data sits on a separate managed volume with a Managed Apple ID. Across all platforms, MDM cannot read personal email content, SMS, photos, browsing history, call history or personal app data. With MAM only — App Protection Policies without enrollment — the only thing under management is corporate data inside managed apps, and selective wipe removes exactly that and nothing else. Corporate-owned devices are different: full app inventory is visible, and location can be configured for lost-device scenarios. Being able to state that clearly is what gets works councils and privacy-conscious users to say yes."

**Q5. "Tell me about Microsoft's culture and why it appeals to you."**
> *Model answer:* "The concept I connect with most is growth mindset — the 'learn-it-all rather than know-it-all' framing — because it matches how I think this kind of work actually improves. In support you are permanently confronted with things you don't know, and the engineers who do well are the ones who treat that as interesting rather than threatening. Alongside it, customer obsession maps directly onto what this team does: CVC exists to anticipate and systemically solve customer needs, which is a much more ambitious statement than 'answer tickets well'. And 'One Microsoft' matters practically here, because Intune cases cross Entra, Windows, Defender and platform vendors constantly, and the difference between a two-hour investigation and a two-week one is usually whether people work across boundaries or defend them. What appeals to me is that this role sits exactly at those intersections — customer, support, and engineering — which is where I think the most useful work happens."

**Q6. "How do you keep current in a product that changes monthly?"**
> *Model answer:* "Deliberately, and with a routine rather than good intentions. Message Center and the Intune What's New notes are the non-negotiable baseline, because they're what actually affects customers, and I'd read them with a specific question: which of my customer's configurations does this touch? Beyond that, the CSP and Graph reference documentation for depth, the Windows and Apple and Android platform release notes because half of Intune's behaviour is really platform behaviour, and community channels — forums, blogs, Reddit — for early signal on regressions, which are often visible there before they're formally acknowledged. I'd also keep a lab tenant with representative devices, because reading a release note is not the same as seeing the behaviour. And honestly, the most effective mechanism is the case queue itself: the cases arriving this month *are* the change log for the things that matter."

---

## 🧠 30-Second Memory Hooks

- **Jamf = Apple depth. Workspace ONE = mixed/rugged heritage. Intune = one control plane + identity + threat signal + already licensed.**
- **Never trash a competitor — ask which capability the customer actually needs.**
- **"Commercial, GCC, GCC High or DoD?"** — ask early; endpoints, parity and guidance all differ.
- **Zero Trust · NIST · CIS · STIG · Essential Eight** — the frameworks customers map baselines to.
- **Privacy: MDM sees posture, not content.** Say it precisely; vagueness destroys trust.
- **Windows 10 EOL → migration wave → Autopilot and ESP case wave.** Trend → support consequence.
- **Each Intune Suite feature maps to a known support cost.**
- **Growth mindset stories need a real mistake, a real cost, and a changed behaviour.**
- **Customer obsession · One Microsoft · Disagree and commit · Model, Coach, Care.**
- **Refresh the trends section the week of the interview** — it ages fastest.

---

*Next suggested section:* **[Part P — Interview Question Bank](Part-P-interview-question-bank.md)** — 100+ questions across basic, intermediate, advanced, behavioural and closing, each with an answer or hint and a cross-reference back to the Part that explains it.
