# Part P — Interview Question Bank (150 questions)

> **How to use this Part:** don't read it — *answer* it. Cover the answer column, say your answer **out loud**, then compare. Speaking is a different skill from knowing, and the gap between them is where interviews are lost. Mark each question in the [self-quiz tracker](#-self-quiz-tracker) as 🔴 can't answer · 🟡 shaky · 🟢 fluent. Target: everything 🟢 in the top 40, and no 🔴 anywhere.

**Composition:** 25 Basic · 25 Intermediate · 70 Advanced · 20 Behavioural (STAR) · 10 Closing = **150 questions**.

**Ref key:** the Part that explains the answer in full — [A](Part-A-cloud-and-modern-management.md) · [B](Part-B-entra-identity-and-access.md) · [C](Part-C-intune-architecture.md) · [D](Part-D-enrollment-and-autopilot.md) · [E](Part-E-configuration-and-compliance.md) · [F](Part-F-app-management.md) · [G](Part-G-endpoint-security.md) · [H](Part-H-cross-platform.md) · [I](Part-I-troubleshooting-and-diagnostics.md) · [J](Part-J-networking-for-intune.md) · [K](Part-K-live-site-and-availability.md) · [L](Part-L-support-process-and-voc.md) · [M](Part-M-sdlc-and-engineering-partnership.md) · [N](Part-N-ai-and-agentic-support.md) · [O](Part-O-misc-and-deeper-topics.md)

---

## 🟩 Section 1 — Basic (Q1–Q25)

*These must be instant and confident. Hesitating here is disqualifying.*

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 1 | What is Microsoft Intune? | Cloud-based unified endpoint management: enrols Windows, iOS/iPadOS, macOS, Android and Linux; delivers configuration, compliance, apps and security; its compliance signal feeds Entra Conditional Access. Also does app-level management (MAM) without enrollment. | A |
| 2 | What is SaaS, and why does it matter that Intune is SaaS? | Software you rent, fully run by the provider. Consequences: no server to restart, everyone shares the service (so blast radius and rings matter), and the blame boundary spans service / tenant config / network / client. | A |
| 3 | What is a tenant? | An organization's isolated instance of Microsoft's cloud, with one Entra ID directory and a unique Tenant ID GUID. The boundary for identity, licensing, policy, RBAC and data residency. | A |
| 4 | What is Microsoft Entra ID? | The cloud directory — users, groups, devices, applications — formerly Azure AD. It handles authentication and hosts Conditional Access. | B |
| 5 | Name the four Microsoft Security product families and what each does. | Entra = identity (who). Intune = endpoints (what device). Defender = threat protection (is it safe). Purview = data governance and compliance (the data). | A |
| 6 | What's the difference between MDM and MAM? | MDM manages the whole device (enrol, encrypt, restrict, wipe). MAM manages only corporate data inside apps, and works on unenrolled personal devices. | A/F |
| 7 | What is Conditional Access? | Entra's if-then policy engine evaluated at every sign-in: if these users, resources and conditions, then apply these grant or session controls. | B |
| 8 | What is device compliance? | Intune's verdict on whether a device meets defined rules; written to the Entra device object and consumed by Conditional Access. | E |
| 9 | Configuration profile vs compliance policy? | Configuration *sets* device state; compliance *judges* it. Independent — good design does both so drift is detected. | E |
| 10 | What is Windows Autopilot? | Cloud provisioning of new Windows PCs with no imaging: hardware identity is pre-registered, OOBE is customized, and the device configures itself when the user signs in. | D |
| 11 | What is the Enrollment Status Page? | The full-screen progress page during provisioning that can block the desktop until required setup finishes. Three phases: device preparation, device setup, account setup. | D |
| 12 | What are the three Zero Trust principles? | Verify explicitly · use least-privilege access · assume breach. | A |
| 13 | What is MFA and why does it matter? | Two or more of: something you know, have, or are. It blocks the overwhelming majority of password-based attacks. | B |
| 14 | What is a security baseline in Intune? | A versioned bundle of Microsoft-recommended security settings you can deploy as a starting posture. | G |
| 15 | What is BitLocker? | Windows full-volume encryption, with the key protected by the TPM; recovery keys can be escrowed to Entra. | G |
| 16 | What is a `.intunewin` file? | The encrypted package produced by the Win32 Content Prep Tool, containing an app's source folder for deployment via the Intune Management Extension. | F |
| 17 | What is a detection rule? | The test Intune runs to decide whether an app is installed — before install to skip work, and after install to confirm success. | F |
| 18 | Name the Windows device actions for wiping and what each does. | Retire = remove company data only. Wipe = factory reset. Delete = remove the Intune record only (does nothing to the device). Autopilot Reset = wipe user state but keep enrollment. | G |
| 19 | What is Microsoft Graph? | The single REST API behind Microsoft 365, Entra and Intune. The Intune admin center is itself a Graph client. `/v1.0` and `/beta`. | C |
| 20 | Roughly how often does a Windows device check in? | MDM channel roughly every 8 hours (more often just after enrollment); Intune Management Extension roughly hourly; plus push-triggered check-ins. | C |
| 21 | What does a push notification service (WNS/APNs/FCM) actually do? | Wakes the device so it checks in. It carries no policy — the device always initiates the outbound connection. | C/J |
| 22 | What is Group Policy and how does it differ from MDM? | GPO is on-prem, needs a domain controller and the corporate network, Windows-only. MDM is internet-based, cross-platform, and uses CSPs rather than registry policy. | A/E |
| 23 | What is co-management? | A Windows device managed by both Configuration Manager and Intune, with each of seven workloads assigned to one authority. | E |
| 24 | What is an App Protection Policy? | An Intune MAM policy enforced by the app itself (via the Intune App SDK): app PIN, encryption, block copy-paste to personal apps, selective wipe. | F |
| 25 | What does `dsregcmd /status` tell you? | Join type, device ID, tenant, whether a valid PRT exists, the MDM URLs, and the last registration error. The Windows identity X-ray. | B/I |

---

## 🟦 Section 2 — Intermediate (Q26–Q50)

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 26 | Explain Entra registered vs joined vs hybrid joined. | Registered = personal device known to the directory (user signs in with a personal/local account). Joined = org-owned, cloud-only, user signs into Windows with the work account. Hybrid = domain-joined *and* Entra-registered, still gets Group Policy. | B |
| 27 | What is a PRT and why does it matter? | A refresh token bound to both user and device, protected by the TPM. Provides SSO and carries the device claim Conditional Access needs. No PRT = repeated prompts and failing device-based CA. | B |
| 28 | What is a CSP? | Configuration Service Provider — the on-device component that implements a group of settings, addressed by an OMA-URI. It turns an MDM instruction into a real OS change. | C |
| 29 | What is OMA-DM / SyncML / OMA-URI? | The MDM protocol Windows uses / its XML message format / the path addressing a specific setting, e.g. `./Device/Vendor/MSFT/Policy/Config/...`. | C |
| 30 | What does SyncML status 404 mean? | Node not found — the setting doesn't exist on that Windows edition or build. The classic custom-OMA-URI failure. | C/I |
| 31 | MDM channel vs Intune Management Extension — what does each deliver? | MDM channel: config profiles, compliance, certs, Wi-Fi/VPN, MSI, Store apps. IME: Win32 apps, PowerShell scripts, Remediations, new Store apps. Different logs, different cadence. | C/F |
| 32 | Where is the IME log and how do you read it? | `C:\ProgramData\Microsoft\IntuneManagementExtension\Logs\IntuneManagementExtension.log` — open in CMTrace; search the app GUID and follow policy → applicability → download → exit code → detection. | F/I |
| 33 | What are the Autopilot deployment scenarios? | User-driven · self-deploying (TPM 2.0) · pre-provisioning/white glove (Windows key ×5) · Autopilot for existing devices · Autopilot Reset · Autopilot device preparation. | D |
| 34 | What is a Group Tag and where is it stored? | An Autopilot label stored in the device's `devicePhysicalIds` as `[OrderID]:<tag>`, used in dynamic device group rules to target profiles. | B/D |
| 35 | What is the Settings Catalog and why prefer it? | The complete, searchable settings surface for a platform. It's where new settings appear first, shows underlying setting identity, and multiple catalog policies merge cleanly when they don't overlap. | E |
| 36 | Filters vs groups — when do you use each? | Groups express organizational intent; filters express device characteristics evaluated at assignment time. Target broadly with a few groups, narrow with filters — it scales far better. | E |
| 37 | What happens when two policies set the same setting differently? | Conflict — neither value applies reliably and Intune reports Conflict. Fix by removing one source, not by trying to out-rank the other. | E |
| 38 | What is MDMWinsOverGP? | A Windows policy that flips the default so MDM-delivered settings beat Group Policy for settings that exist in both. Relevant during GPO→Intune migration. | E |
| 39 | What are the Win32 app rule types? | Requirement (should this device get it), detection (is it installed), dependencies (install order), supersedence (replace an older app), plus return-code mappings. | F |
| 40 | Required vs Available vs Uninstall — and which wins? | Auto-install / user-initiated from Company Portal / active removal. **Uninstall wins** over Required. | F |
| 41 | What does Defender for Endpoint contribute to Intune? | A machine risk score that Intune compliance policies consume, plus security tasks from vulnerability management that appear in Intune for remediation. | G |
| 42 | SCEP vs PKCS? | SCEP: the device generates the key pair (private key never leaves the device) and NDES + Certificate Connector + CA issue the cert. PKCS: the connector requests the cert and delivers the key pair to the device. | G |
| 43 | Why must you deploy a trusted root profile? | So the certificate chain validates. Deploy the root *before* the leaf, or 802.1X and other cert-based scenarios fail despite a valid certificate being present. | G |
| 44 | What is supervision on Apple devices? | An elevated management state achievable only via ADE (through ABM/ASM) or Apple Configurator. Most strong restrictions are supervised-only. | H |
| 45 | Name the Android Enterprise modes. | Personally-owned work profile · corporate-owned work profile · fully managed · dedicated/kiosk · AOSP · (deprecated) device administrator. | H |
| 46 | What is the DPC on Android? | Device Policy Controller — the app enforcing policy. Intune app for fully managed/dedicated/corporate work profile; Company Portal for personally-owned work profile. | H |
| 47 | What is Delivery Optimization? | Windows' peer-to-peer plus HTTP content distribution, with download modes 0/1/2/3/99/100, bandwidth limits and peer grouping — the answer to branch-office bandwidth. | F/J |
| 48 | What is throttling and how should a client behave? | The service rejecting excess requests. Graph returns HTTP 429 with `Retry-After`; honour it, then exponential backoff with jitter. Never tight-retry. | C |
| 49 | What's the difference between a bug and a DCR? | A bug = the product doesn't do what it's designed to do. A DCR = it works as designed but the design causes pain. Most supportability issues are DCRs. | L |
| 50 | What's the difference between incident and problem management? | Incident = restore service fast (MTTR). Problem = eliminate the cause so incidents stop recurring. This role is problem management. | L |

---

## 🟥 Section 3 — Advanced (Q51–Q120)

*The bulk of a senior loop. Answer these with mechanism, failure mode, evidence and prevention.*

### Architecture and internals

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 51 | Walk through a policy from admin click to OS change. | Graph write → assignment resolved against Entra groups + filters → push via WNS → device dials out and authenticates with its MDM cert → SyncML `Replace` on an OMA-URI → CSP applies → status returned → reporting aggregates. Seven hops, seven failure classes. | C |
| 52 | What is an ASU and why does it matter operationally? | Account Scale Unit — the service slice hosting a tenant. Incidents are frequently scoped to scale units, so knowing a customer's ASU answers "is this you or us?" | A/C |
| 53 | Why can identical policy behave differently in two tenants? | Different service release rings, different OS builds/editions (CSP support varies), different client versions, licensing/add-ons, scope tags, filters, or conflicting policies. | C |
| 54 | How do you tell a portal/UI issue from a service issue? | Call Graph directly for the same object; use F12 → Network to see the portal's own Graph calls and the raw error the UI swallows; check Tenant status and Service health; compare against a second tenant. | C/I |
| 55 | What does "Sync" actually do — and not do? | Makes the device request policy now. It does **not** accelerate Entra dynamic-group evaluation, Intune assignment recalculation, or reporting aggregation. | C/E |
| 56 | What's on the Tenant status page and why care? | MDM authority, service release (which ring), service health, and connector/token expiry — APNs, Apple ADE, VPP, certificate connector, Managed Google Play. Expiry is a preventable outage class. | C |
| 57 | Describe how Intune manages iOS versus Windows, architecturally. | Windows: OMA-DM/SyncML + CSPs + WNS. Apple: Apple MDM protocol, commands + `.mobileconfig` payloads + APNs (5223), moving to DDM. Android: Android Management API + FCM + a DPC app. | C/H |
| 58 | What is Declarative Device Management and why does it matter? | Device holds declarations of desired state, applies them itself and proactively reports status instead of being polled. Better scale and — importantly — better diagnosability. | H |
| 59 | How would you design policy for a 200,000-device tenant? | Few, broad policies narrowed with filters rather than thousands of groups; consistent naming and scope tags; ring-based rollout; avoid deep dependency graphs; monitor assignment evaluation cost. | C/E |
| 60 | What are the operational risks of multi-tenancy? | Noisy neighbours (mitigated by throttling), correlated failure from one bad change (mitigated by scale units and SDP rings), and data isolation. | A/K |

### Enrollment and provisioning

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 61 | ESP is stuck at 45 minutes. Triage it. | Identify the phase; suspect blocking apps first. `Shift+F10` / Collect logs → `Get-AutopilotDiagnostics` for a per-item timeline → IME log for the offending app → most often a bad detection rule. Long-term: minimal blocking-app list, realistic timeout, escape hatch enabled. | D/F |
| 62 | Why can Conditional Access break enrollment? | Chicken-and-egg: "require compliant device" can't be satisfied before enrollment, and MFA can't be satisfied where no interactive user exists (self-deploying). Design CA for the enrollment path explicitly. | B/D |
| 63 | Autopilot "didn't work" — device got normal OOBE. Why? | Not recognised as an Autopilot device at boot: hash not uploaded, profile not assigned yet (assignment isn't instant), group membership latency, or no network at OOBE. Verify profile status = Assigned before blaming the device. | D |
| 64 | How do you fully clean a half-enrolled Windows device? | Prefer reset/Autopilot Reset in production. Manually: disconnect work account → delete the object in **both** Intune and Entra → remove `EnterpriseMgmt\{GUID}` tasks → remove `HKLM\...\Enrollments\{GUID}` keys → remove the MDM device certificate → reboot → re-enrol. | D |
| 65 | What does enrollment actually create on a device? | An Entra device object + Intune managed-device record, MDM server URLs and scheduled sync tasks (DMClient CSP), and a device management certificate for authentication. | D |
| 66 | What is `EnrollmentState` and where does it live? | `HKLM\SOFTWARE\Microsoft\Enrollments\{GUID}` — 1 = in progress, 2/3 = enrolled. Combined with missing scheduled tasks it reveals a ghost enrollment. | D/I |
| 67 | Compare Apple enrollment types. | ADE (supervised, non-removable, zero-touch via ABM) · Apple Configurator (supervised, USB) · account-driven user enrollment (BYOD, separate managed volume, removable) · profile-based device enrollment (older BYOD). | D/H |
| 68 | An Android fleet won't enrol. What do you check? | Managed Google Play binding, enrollment restrictions for the mode, GMS presence and reachability of Play/Google endpoints, device country/app availability, and whether they're on the deprecated device-administrator path. | D/H |
| 69 | Why does device ownership (corporate vs personal) matter? | It changes visible inventory (full app inventory only on corporate), permitted actions, applicable enrollment restrictions and privacy expectations. Corporate identifiers/serial lists drive it. | D |
| 70 | Design an Autopilot rollout for 10,000 devices. | OEM registration at purchase; Group Tags driving dynamic device groups; profiles assigned to device groups and verified Assigned; pre-provisioning for slow apps; minimal ESP blocking list with a realistic timeout and escape hatch; pilot ring first; deployment report monitored; network validated at OOBE including proxy behaviour. | D |

### Configuration, apps and security

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 71 | Everything went non-compliant at once. Causes? | Tenant setting "no compliance policy = Not compliant"; a policy edited (e.g. min OS raised without grace); a broken connector (Defender risk, Apple/Google tokens); a service issue. Separate Non-compliant from Not evaluated — different investigations. | E |
| 72 | A setting reports Succeeded but has no effect. Why? | Another authority overwrites it (GPO on hybrid, or a second Intune policy); wrong scope (device vs user); needs reboot/sign-out; OS edition ignores it; a local app resets it. Succeeded = acknowledged, not true. | E/I |
| 73 | How do you check something Intune can't natively check? | Custom compliance: detection script emitting JSON + a JSON rules file with expected values, operators and remediation messages. Folds into the compliance verdict and therefore into CA. | E |
| 74 | App installs but reports failure. Why? | Detection rule mismatch (`0x87D00324`): wrong path/registry/version, 32-bit redirection (`Program Files (x86)`, `WOW6432Node`), or a script not honouring the contract (STDOUT + exit 0). | F |
| 75 | How do dependencies and supersedence fail? | A dependency's bad detection blocks the parent and the failure is reported on the parent; deep chains multiply risk; non-silent uninstall commands leave devices with neither version; circular supersedence. | F |
| 76 | Apps fail to download but policies apply. What's happening? | Content path problem: CDN endpoints blocked or TLS-inspected (look for hash/decrypt errors in the IME log), disk space, or DO/bandwidth limits. | F/J |
| 77 | How would you roll out ASR rules to 50,000 devices safely? | Audit mode everywhere first, across representative business units and long enough to cover monthly processes; review the ASR report and hunt the audit events; add narrow exclusions; move to Block ring by ring with rollback and helpdesk comms; document every exclusion. | G |
| 78 | Device non-compliant for encryption but the user says it's encrypted. | `manage-bde -status` for truth: encryption may be in progress, a data drive may be unencrypted, the method may differ, or protection suspended. Also check whether silent enablement was blocked by failed **escrow**, third-party encryption, or missing TPM/Secure Boot. | G |
| 79 | Why would Defender settings not apply? | Tamper protection blocking non-management changes; third-party AV putting Defender in passive mode; conflict with a baseline or another endpoint-security policy; multiple onboarding methods; unsupported edition; device hasn't checked in. | G |
| 80 | Design Windows update rings for an enterprise. | Canary (0-day) → pilot (~2–3 days) → broad (~7 days) → rest (~10–14 days), with deadlines, grace periods and active hours; separate rings for exec/frontline/critical systems; expedite for zero-days; feature-update pinning; expect safeguard holds; consider Autopatch. | G |
| 81 | What breaks most often in certificate deployment? | Trusted root missing or deployed after the leaf; NDES unreachable; connector down or its own cert expired; template misconfiguration; IIS request-filtering limits (404.15); clock skew; wrong subject/SAN variables; mass renewal/expiry. Cloud PKI removes much of this. | G |
| 82 | What is Endpoint Privilege Management and what problem does it solve? | Elevate specific approved apps for standard users with audit, so local admin rights can be removed without breaking the users who need one tool. Removes a major lateral-movement enabler and produces data on what actually demands elevation. | G |
| 83 | Explain the compliance → Conditional Access loop and where it breaks. | Device reports state → Intune evaluates → writes to the Entra device object → CA reads it. Breaks at: propagation delay, stale token lacking the device claim, duplicate device objects, browser not passing device state, unsupported platform. | B/E |
| 84 | Two Intune surfaces can set the same security setting. Why is that a problem? | Baselines, endpoint-security policies, Settings Catalog and legacy templates overlap, producing conflicts. Pick one authoring surface per setting domain, document it, and use per-setting status to detect overlap. | G |
| 85 | How would you migrate 300 GPOs to Intune? | Export to XML → Group Policy analytics for MDM equivalence → re-derive *intent* rather than translating one-for-one → prefer baselines for security posture → scripts/remediations for gaps → pilot rings with MDMWinsOverGP and the old GPO removed from scope → validate device state → expand → decommission. | E |

### Troubleshooting and networking

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 86 | Describe your troubleshooting methodology. | Scope → reproduce → isolate the layer (identity/service/network/client/OS/app) → falsifiable hypothesis → prove with evidence (device truth over reports) → fix or mitigate → prevent (TSG, remediation, bug/DCR, monitoring). | I |
| 87 | How do you use error-code prefixes? | `0x8018` MDM enrollment · `0x801c` Entra device registration · `0x87D0/0x87D1` Intune client agent · `0x80072Exx` WinHTTP (proxy/TLS/DNS) · `0x8007` wrapped Win32. The prefix routes you to the right log and the right team. | I |
| 88 | What does `0x87D00324` mean and what do you do? | Installed but not detected — a detection-rule problem, not an install problem. Fix the rule; check 32/64-bit redirection and the script contract. | F/I |
| 89 | What does `0x80072f8f` usually mean? | Date/time or certificate error — very often clock skew. Check `w32tm /query /status`; skew also breaks Kerberos, tokens and SCEP challenges. | I/J |
| 90 | The customer says nothing changed. What do you do? | Open the audit log (who changed what, when), Entra audit/sign-in logs for CA changes, Tenant status for expiries (changes that happen *to* you), Message Center for service changes, and their own change record for network/proxy/cert work. | I |
| 91 | Walk the network stack for "device won't check in". | DNS → TCP → TLS → HTTP, each with a distinct signature. Fastest discriminator: does it work on a mobile hotspot? If yes, it's the customer's network path. | J |
| 92 | Explain TLS inspection and how it breaks Intune. | A middlebox terminates and re-encrypts TLS with its own certificate. Breaks certificate pinning, mutual TLS for MDM check-in (`0x80072f0c`), content hash verification (hash/decrypt errors), and large downloads. Bypass Microsoft endpoints by SNI. | J |
| 93 | Why does something work in a browser but fail for Intune? | Proxy context: WinINET (per-user) vs WinHTTP (SYSTEM). Authenticated proxies give SYSTEM services HTTP 407 silently. Same reason authenticated proxies kill Autopilot — OOBE has no user. | J |
| 94 | Apple devices only check in at home. Why? | TCP 5223 (APNs) blocked on the corporate network. Management still works on the slow poll, so it looks like extreme latency rather than an outage. | H/J |
| 95 | What is a captive portal and why does it matter? | Guest-Wi-Fi interception: the device looks connected but reaches nothing. Windows detects it with connectivity probes; conversely, blocking those probes makes Windows report "no internet" when traffic works. | J |
| 96 | How do you troubleshoot something intermittent? | Stop chasing repro; instrument. Increase logging, ship to Log Analytics, collect exact timestamps from several occurrences, then look for correlation — time of day, site, model, build, whether it follows user or device. Change nothing until you have several data points. | I |
| 97 | What logs do you collect per platform? | Windows: `MdmDiagnosticsTool` → `MDMDiagReport.html`, event logs, IME logs. iOS: Company Portal send-logs (get the incident ID) + sysdiagnose. macOS: `/Library/Logs/Microsoft/Intune/`, `profiles show`, `log collect`. Android: send-logs, `logcat`/`bugreport`. All: Intune **Collect diagnostics**. | H/I |
| 98 | How would you use KQL in this role? | Turn anecdote into evidence: "1,347 devices, 22 tenants, from 14:10 UTC, one build". Also proactive detection of silent problems and alerting. Key operators: `where`, `summarize by bin()`, `dcount()`, `render timechart`. | I |
| 99 | How do you prove a fault is the customer's network vs Microsoft's service? | Controlled comparison: same device/user on a hotspot, a second site, a second tenant; plus wire evidence (certificate issuer, DNS answers, 407s) and service-side evidence (Service health, cross-tenant pattern). Present it as evidence, not opinion. | I/J |
| 100 | A user's device is compliant in Intune but CA still blocks. Full answer. | Entra sign-in logs → Conditional Access tab shows the exact policy and failing control. Causes: stale token (sign out/in), propagation delay, duplicate Entra device object (compare device IDs), browser not passing device state, unsupported platform. Validate fixes with What If and report-only. | B/E |

### Live site, process and leadership

| # | Question | Concise answer / hint | Ref |
|---|---|---|---|
| 101 | SLA vs SLO vs SLI vs error budget? | Contractual promise / internal target (tighter) / the measurement behind it / the allowed failure that governs whether you ship or fix. | K |
| 102 | How would you define availability for Intune? | Scenario-level: can devices enrol, check in and get policy in the expected window, do apps install, does compliance propagate, are connectors healthy — not "is the portal up". Compliance-propagation failure is worse than a portal outage because CA blocks users while dashboards look green. | K |
| 103 | Walk me through handling a live-site incident. | Scope and impact → engage (ICM at the right severity, DRI, bridge) → **mitigate before root-cause** → communicate early on a committed cadence → resolve → blameless RCA with owned, dated repair items including the detection gap. | K |
| 104 | Why mitigate before diagnosing? | Impact accrues continuously while you investigate; mitigation takes minutes, root cause takes hours. Exception: if mitigation destroys evidence, capture it in parallel — never delay mitigation for it. | K |
| 105 | What are Safe Deployment Practices and why should support care? | Progressive rollout with health gates and bake time. It explains cross-tenant differences, gives the highest-yield live-site question ("did a deployment reach this scale unit?"), and is the advice you give customers about their own changes. | K |
| 106 | How do you write an RCA engineering will act on? | Quantified customer impact, a UTC timeline that exposes the *gaps*, root cause plus contributing factors via Five Whys until it's systemic, and specific owned dated repair items covering fix, test, monitor and guardrail. Blameless, and record what went well. | K |
| 107 | How do you communicate during an outage? | Early (before you know the cause), on a committed cadence, in customer terms, stating what is *not* affected, never speculating on cause, with any workaround, and closing the loop with the RCA on the promised date. Tailor to audience. | K |
| 108 | Customer asks you to force 200,000 devices to sync after an incident. | Push back: a thundering herd can re-degrade the recovering service and trip throttling, making convergence slower. Prefer natural check-in drain, or staggered batches prioritising real business impact. | K |
| 109 | How do you cost a problem-management ticket? | Volume × AHT × loaded rate + escalation hours × engineering rate + customer-side impact + risk cost, plus the growth rate. Express the output as a payback period so it becomes an investment decision. | L |
| 110 | What makes a bug engineering will fix? | Precise title, quantified impact and trend, minimal deterministic repro, expected vs actual, curated evidence with correlation IDs, environment details, regression status, workaround, and an explicit ask. Do the first hour of their work. | L |
| 111 | What makes a good TSG? | Confirm-you're-in-the-right-place check → decision tree → exact commands, log paths and sample output → explains *why* → explicit escalation criteria and destination → dated, owned, linked to the known error. | L |
| 112 | How would you run Voice of the Customer? | An evidence pipeline: signals (cases, telemetry, community, CSAT, advisory boards) → consistent taxonomy → clustering → quantification → prioritisation → routing to bug/DCR/docs/automation/enablement → **close the loop and measure the reduction**. Guard against anecdote-driven prioritisation and open loops. | L |
| 113 | How do you coordinate a case spanning Intune, Entra, Windows and the customer's network? | Establish the boundary with evidence, then bring teams together rather than relaying serially; single owner, agreed next test, one message to the customer; own the outcome, not the hand-off; close the loop with everyone including the innocent teams. | L |
| 114 | Where in the SDLC is support feedback most valuable, and why? | Design — the cost of change rises ~10× per stage. A reason code at design costs an enum value; the same gap after GA costs a permanent TSG and thousands of cases. | M |
| 115 | What do you look for in a design review? | Diagnosability (unique codes, correlation IDs, admin-visible reasons, remote-collectable structured logs), observability (health signal, probe, alert, scopeable), recoverability (flag, tested rollback, in-flight behaviour), UX, scale/compat, readiness. The 2am question. | M |
| 116 | How do you influence engineers you have no authority over? | Speak their currency (evidence, effort, risk), reduce their effort, make asks specific and bounded, build reciprocity, escalate transparently, give credit publicly, and protect credibility by never over-claiming. | M |
| 117 | Explain RAG and why it matters for support. | Retrieve authoritative sources, answer only from them, cite them. Fixes the three fatal flaws of raw LLMs for support: no internal knowledge, training cutoff, hallucination — and makes answers auditable and fixable at the source. Insist on hybrid search and metadata filtering. | N |
| 118 | Give three concrete AI use cases for Intune support and the metric each moves. | Log triage (AHT) · knowledge retrieval with citations (AHT, escalation rate) · case clustering and anomaly narration (time-to-detect for problem management). All read-only, low risk, measurable. | N |
| 119 | How do you stop an AI assistant giving wrong answers? | Grounding + citations + low temperature + permitted refusal + schema-constrained output; then a golden eval set re-run on every change, measuring groundedness, retrieval precision/recall, accuracy and **calibrated refusal**; HITL for state changes; full audit trail. | N |
| 120 | What's the risk of measuring deflection? | It's trivially gamed by making humans hard to reach. Never report deflection without CSAT and reopen rate as guardrails, and count inference cost honestly in cost-per-case. | N |

---

## 🟪 Section 4 — Behavioural / STAR (Q121–Q140)

*Prepare a written STAR story for each. Reuse a small set of strong stories across multiple questions — see [Part Q](Part-Q-behavioral-and-closing.md).*

| # | Question | What they're testing | Story to prepare |
|---|---|---|---|
| 121 | Tell me about a time you solved a difficult technical problem. | Methodology, depth, evidence | A problem where you *proved* the cause rather than guessing; show the hypothesis-test loop |
| 122 | Describe a time you were wrong about a diagnosis. | Growth mindset, honesty | A genuine mistake, its cost, and the behaviour you changed afterwards |
| 123 | Tell me about a time you handled a major incident. | Live-site judgement | Emphasise mitigation-first, comms cadence, and the RCA repair items |
| 124 | Describe a time you had to make a decision with incomplete information. | Ambiguity | Reversible vs irreversible, stated assumptions, timeboxing |
| 125 | Tell me about a time you influenced a team you had no authority over. | Leadership without authority | Evidence, reduced effort for them, transparent escalation |
| 126 | Describe a conflict with a colleague or another team. | Maturity, One Microsoft | Focus on the shared goal and how you re-framed it; never trash anyone |
| 127 | Tell me about a time you disagreed with a decision and had to support it anyway. | Disagree and commit | Argued fully with data, then backed it, and how you validated the outcome later |
| 128 | Describe a time you improved a process. | Systemic thinking | Measure → analyse → improve → control, with a number attached |
| 129 | Tell me about a time you turned a recurring issue into a permanent fix. | Problem management | This is *the* story for this role — recurring tickets → quantified case → bug/DCR or automation → measured reduction |
| 130 | Describe a time you had to deliver bad news to a customer. | Communication, integrity | Honesty, ownership, what you offered instead |
| 131 | Tell me about a time you dealt with a very frustrated customer. | Empathy under pressure | Acknowledge, get specific, commit to a cadence, deliver |
| 132 | Describe a time you had to learn something completely new quickly. | Learn-it-all | Method, not just outcome — how you learn is the answer |
| 133 | Tell me about a time you documented or taught something that helped others. | Enablement | TSG/KB/training, and how you measured whether it worked |
| 134 | Describe a time you prioritised badly, or missed something. | Self-awareness | Own it cleanly; describe the guardrail you added |
| 135 | Tell me about a time you pushed back on a customer request. | Judgement | Explain the risk, offer the alternative, document the decision |
| 136 | Describe a time you worked across many teams to solve something. | Coordination | Boundary evidence, joint call, single owner |
| 137 | Tell me about a time you spotted a problem before anyone reported it. | Proactivity | Telemetry/trend detection — the CVC mindset |
| 138 | Describe a time you had to balance speed and quality. | Trade-off judgement | Mitigate now / fix properly later, with the follow-through actually done |
| 139 | Tell me about a time you automated something. | Impact, tooling | Remediation script or query; quantify hours saved |
| 140 | Describe your biggest professional achievement. | Impact framing | Choose one with a *number* and a *customer outcome*, not just effort |

---

## 🟨 Section 5 — Closing / "why" questions (Q141–Q150)

| # | Question | What a strong answer contains | Ref |
|---|---|---|---|
| 141 | Why do you want this role? | The specific combination: deep technical troubleshooting + owning one major customer + turning problems into product change + the AI/agentic angle. Name things from the JD, not generic enthusiasm. | Q |
| 142 | Why Microsoft Security / why this team? | Endpoint management is the foundation of Zero Trust; CVC's mission is systemic customer problem-solving rather than ticket-closing; the Agentic framing shows where the team is going. | A/N/Q |
| 143 | Why should we hire you? | Three concrete strengths mapped to JD bullets, each with a one-line proof point. Then the honest gap and how you'd close it. | Q |
| 144 | What's your biggest weakness? | A real one, with the concrete mitigation you already use. Never a humblebrag. | Q |
| 145 | Where do you see yourself in 3–5 years? | Growth *in this direction* — deeper technical mastery plus broader systemic influence. Don't describe leaving the job. | Q |
| 146 | What would you do in your first 90 days? | 0–30 learn and baseline the customer + case data; 30–60 build the operational rhythm (expiry register, health review, top-3 problem clusters); 60–90 convert clusters into fixes and measure. | K/N/Q |
| 147 | What's your approach to learning a product that changes monthly? | Message Center and What's New with a "what does this touch for my customer?" lens; platform release notes; community for early regression signal; a lab tenant; and the case queue as the real change log. | O |
| 148 | How do you handle being on call / high-pressure situations? | Structure beats adrenaline: scope, mitigate, communicate on a cadence, hand off cleanly, and protect sustainability so judgement stays good. | K |
| 149 | Do you have any questions for us? | Always yes — 4–6 prepared questions that show you've thought about the *work* (see [Part Q](Part-Q-behavioral-and-closing.md)). | Q |
| 150 | Is there anything you'd like to add? | A 30-second close: the one thing you most want them to remember about you, tied to their biggest need. | Q |

---

## 📊 Self-quiz tracker

Copy this table and fill it in after each pass. Do at least **three passes**, spaced days apart.

| Section | Range | Pass 1 (🔴/🟡/🟢) | Pass 2 | Pass 3 | Notes |
|---|---|---|---|---|---|
| Basic | Q1–Q25 | | | | Must be 100% 🟢 |
| Intermediate | Q26–Q50 | | | | Aim 100% 🟢 |
| Advanced — architecture | Q51–Q60 | | | | |
| Advanced — enrollment | Q61–Q70 | | | | |
| Advanced — config/apps/security | Q71–Q85 | | | | |
| Advanced — troubleshooting/network | Q86–Q100 | | | | ⭐ Highest priority |
| Advanced — live site/process/AI | Q101–Q120 | | | | ⭐ Highest priority |
| Behavioural | Q121–Q140 | | | | Must be *written*, not improvised |
| Closing | Q141–Q150 | | | | Rehearse aloud |

### The 20 questions to be perfect on

If you only have limited time, these carry the most weight for *this* role:

**1** (what is Intune) · **26** (join types) · **27** (PRT) · **28** (CSP) · **31** (MDM vs IME) · **51** (policy end to end) · **61** (ESP stuck) · **62** (CA breaks enrollment) · **74** (installed but not detected) · **86** (methodology) · **87** (error prefixes) · **91** (network stack) · **92** (TLS inspection) · **100** (compliant but blocked) · **103** (live-site incident) · **104** (mitigate first) · **109** (costing a problem) · **112** (Voice of the Customer) · **115** (design review) · **118** (AI use cases).

---

## 🧠 Answering technique — the shapes that work

**For any technical "what is X":**
> what it *is* in one sentence → the mechanism underneath → how it fails in the real world → how you'd prove which failure it is → how you'd prevent it recurring.

**For any "how would you troubleshoot":**
> scope → isolate the layer → hypothesis → the specific evidence you'd collect → the fix → the prevention.

**For any "have you ever" (STAR):**
> **S**ituation (2 sentences of context) → **T**ask (what *you* owned) → **A**ction (what *you* did — 60% of the answer, and say "I", not "we") → **R**esult (**with a number**) → **Reflection** (what you'd do differently or what you carried forward).

**When you genuinely don't know:**
> "I haven't worked with that directly. Here's how I'd reason about it — *[apply the mechanism you do know]* — and here's how I'd verify: *[the specific check]*." **This scores far higher than bluffing, and it's the answer to "never go blank".**

---

*Next suggested section:* **[Part Q — Behavioral & Closing](Part-Q-behavioral-and-closing.md)** — where the STAR stories get written properly, the "why" answers get built, and you get a one-page night-before cheat sheet.
