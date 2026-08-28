# Appendix A — Complete Glossary

[← Master index](../Amadeus%20Service%20Account%20Executive%20-%20Study%20Guide.md) · [Appendix B — Worked Scenario](Appendix-B-worked-scenario.md) · [Appendix C — Quick Reference](Appendix-C-quick-reference.md)

> Every term used anywhere in this guide, defined in one place. The **Part** column tells you where it's explained in depth.
>
> **How to use:** if you hit an unfamiliar word while reading, look here first. If a definition here isn't enough, go to the Part.

---

## A

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Active-active** | Two or more sites both serving live traffic at the same time. | J |
| **Active-passive** | One site serves traffic; a standby waits to take over. | J |
| **Alert** | An automated notification triggered when a monitored condition is met. | J |
| **API** | Application Programming Interface — a defined way for two systems to talk. Like a restaurant menu: a fixed set of requests in a fixed format. | J |
| **Asynchronous** | The caller hands off work and continues without waiting. Like posting a letter. Fails *silently* via growing backlogs. | J |
| **Auto-scaling** | Automatically adding or removing capacity as load changes. | J |
| **Availability** | The percentage of agreed service time the service was usable. | G |
| **Availability zone** | An isolated data centre within a cloud region. | J |

## B

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Backlog** | All incidents currently open. Watch its *age*, not just its size. | C, G |
| **Bandwidth** | How much data can flow at once. Shortage shows as slowness under load only. | J |
| **BIA** | Business Impact Analysis — identifies critical processes and how long they can be down. Sets RTO and RPO. | K |
| **Blameless** | A review culture that asks why the *system* allowed a failure, not who to punish. Not the same as unaccountable. | D |
| **Bridge call** | A continuously open call where everyone needed for a major incident is present. Also "war room". | D |
| **Business continuity** | How the organisation keeps operating at all when normal systems are unavailable. Broader than disaster recovery. | K |

## C

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **CAB** | Change Advisory Board — the group that reviews and authorises normal changes. | F |
| **Cache** | Fast temporary copies of frequently-used data. Fails as stale data or a "stampede" when it empties. | J |
| **Capacity** | How much load a system can handle. | J |
| **Cascading failure** | One slow component exhausting everything upstream, turning a partial fault into a total outage. | J |
| **CDN** | Content Delivery Network — distributed edge caching close to users. | J |
| **CES** | Customer Effort Score — "how easy was it to get this resolved?" | G |
| **Change (standard)** | Pre-approved, low-risk, repeatable. | F |
| **Change (normal)** | Assessed case by case, usually via a CAB. | F |
| **Change (emergency)** | Expedited approval because impact is urgent, e.g. a rollback mid-outage. | F |
| **Change failure rate** | Percentage of changes that cause incidents or need rollback. A *leading* indicator. | F, G |
| **Chaos engineering** | Deliberately injecting failure to prove resilience. | K |
| **Circuit breaker** | A protection that stops calling a failing dependency and fails fast instead of waiting. An electrical fuse. | J |
| **Clock-stopping** | Pausing an SLA timer while awaiting customer input. Legitimate, but a trust-destroyer if used as a loophole. | G |
| **CMDB** | Configuration Management Database — what components exist and how they connect. Enables impact assessment. | B |
| **Contributing cause** | A factor that made an incident worse or longer, but wasn't the root. | F |
| **Correlation** | Two things moving together. **Not** proof of causation. | G |
| **Crisis management** | Handling an event with public, regulatory or safety dimensions. Above major incident management. | K |
| **CSAT** | Customer Satisfaction — usually a 1–5 score on a specific interaction. | G |
| **CSI register** | Continual Service Improvement register — a prioritised portfolio of improvement opportunities with owners and dates. | I |

## D

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **DCS** | Departure Control System — check-in, bag tags, seat assignment, boarding, weight & balance. The system that physically moves passengers. | A |
| **Detractor** | An NPS respondent scoring 0–6. | G |
| **Disaster recovery** | Restoring technology and data after major loss. Governed by RTO and RPO. | J, K |
| **DMAIC** | Define, Measure, Analyse, Improve, Control — the Six Sigma project structure. | I |
| **DNS** | Domain Name System — translates a name into an address. The internet's phone book. | J |
| **DSAT** | Dissatisfaction — the low end of satisfaction data. | G |

## E

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **EDIFACT** | The older airline messaging standard that NDC is gradually displacing. | K |
| **ELS** | Early Life Support — heightened support right after go-live. Should end on **criteria**, not the calendar. | I |
| **Error budget** | 100% minus your SLO. Ship freely while budget remains; when exhausted, reliability work takes priority. | K |
| **Escalation** | Deliberately adding **capability** or **authority** because the current path won't deliver in time. | E |
| **Escalation (functional)** | Adding *skill* — moving to a specialist or engineering. Horizontal. | E |
| **Escalation (hierarchical)** | Adding *authority* — going to a manager for a decision or resource. Vertical. | E |
| **Event** | A detectable signal from a system. May or may not indicate a fault. | B |
| **Exclusions** | Things an SLA doesn't count as downtime, e.g. planned maintenance. Generous exclusions flatter a poor service. | G |

## F

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Failover** | Actually switching to the standby system. | J |
| **Fault tree analysis** | Top-down RCA using logical AND/OR conditions. For complex combinatorial failures. | F |
| **Firewall** | Controls what network traffic is allowed. Blocks show as "connection refused". | J |
| **First-time-fix rate** | Percentage resolved without escalation. Pair with reopen rate. | G |
| **Fishbone** | See *Ishikawa*. | F |
| **Follow-the-sun** | Handing work between regional teams for 24/7 coverage without night shifts. The handover is the baton. | B |
| **Freeze period** | A window where non-essential change is banned. Airlines freeze around peaks. | F |

## G

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **GDS** | Global Distribution System — the marketplace connecting airline seats to travel sellers worldwide. | A |
| **Governance** | The agreed structure of meetings, reports and decision rights that keeps a relationship healthy. | H |
| **Graceful degradation** | Turning off non-essential features to keep core function alive. | J |

## H

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Headroom** | Spare capacity above current peak usage. Shrinking headroom predicts capacity incidents months ahead. | J |
| **Holding statement** | The first communication issued before you have answers: acknowledge, scope, alternative, honest unknown, next update time. | D |
| **Hot potato** | The anti-pattern where a ticket bounces between teams while the customer's problem persists. | C |

## I

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **IATA** | International Air Transport Association — the airline trade body that sets messaging and operational standards. | A, K |
| **ICAO** | The UN aviation body; sets safety and operational standards. Also the 4-letter airport/airline codes used operationally. | A |
| **Impact** | How much damage a fault causes — users affected, business criticality, cost. | C |
| **Incident** | An unplanned interruption to a service **or a reduction in its quality**. Slow counts as broken. | C |
| **IROPS** | Irregular Operations — delays, cancellations, disruption and recovery. Load spikes when tolerance is lowest. | A |
| **Ishikawa** | Fishbone diagram — categorises possible causes (people, process, technology, data, environment, measurement). | F |
| **ISO/IEC 20000** | International standard for IT service management. | K |
| **ISO/IEC 27001** | International standard for information security management. | K |
| **ISO 22301** | International standard for business continuity management. | K |
| **ITIL** | The most widely adopted ITSM best-practice framework. Shared vocabulary and a checklist, not a rulebook. | B |
| **ITSM** | IT Service Management — the discipline of delivering IT as a service. | B |

## K

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Kaizen** | Small, continuous, everyone-participates improvement. | I |
| **KEDB** | Known Error Database — turns a two-hour diagnosis into a two-minute lookup. | F |
| **Kepner-Tregoe** | RCA by contrast: what **IS** affected vs what **IS NOT**, across what/where/when/extent. | F |
| **Known error** | A problem with documented cause and usually a workaround, permanent fix pending. Controlled risk — but it needs a date. | F |
| **KPI** | Key Performance Indicator — a metric chosen to show whether something is working. | G |

## L

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Lagging indicator** | Tells you what already happened. The bathroom scale. | G |
| **Latency** | Delay for data to travel. High latency makes everything feel slow. | J |
| **Leading indicator** | Predicts what's coming. Your calorie intake. | G |
| **Lean** | Removing waste — anything the customer wouldn't pay for. | I |
| **Load balancer** | Distributes requests across multiple servers. | J |
| **Load testing** | Simulating expected load before it arrives. | J |

## M

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Major incident** | A high-impact incident that triggers a *different, coordinated process* — not merely a bigger incident. | D |
| **MIM** | Major Incident Manager — runs the bridge and drives resolution. Does not personally diagnose. | D |
| **MTBF** | Mean Time Between Failures — a measure of stability. | G |
| **MTTA** | Mean Time To Acknowledge — detection until someone owns it. | G |
| **MTTD** | Mean Time To Detect — fault until detection. Exposes monitoring blind spots. | G |
| **MTTF** | Mean Time To Failure — for non-repairable components. | G |
| **MTTR** | Mean Time To Restore / Resolve / Repair / Respond. **Ambiguous — always clarify which "R".** | G |
| **Muda** | Lean's word for waste: waiting, rework, handoffs, over-processing. | I |

## N

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **NDC** | New Distribution Capability — IATA's standard letting airlines sell dynamic, personalised offers. | A, K |
| **Nines** | Shorthand for availability levels: three nines = 99.9% ≈ 43 min/month. | G |
| **NPS** | Net Promoter Score = %Promoters (9–10) − %Detractors (0–6). Passives (7–8) count for nothing. | G |

## O

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Observability** | Being able to ask *new* questions about unexpected behaviour. A diagnostic workshop. | J |
| **OLA** | Operational Level Agreement — an internal agreement between teams that underpins the SLA. | G |
| **ONE Order** | IATA initiative replacing separate ticket/EMD/booking records with a single order. | K |
| **Outcome** | What the customer achieved. "The diners enjoyed dinner." | B |
| **Output** | What the provider produced. "We cooked 400 meals." | B |

## P

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Packet loss** | Data that doesn't arrive. Shows as intermittent failures and retries. | J |
| **Pareto analysis** | ~80% of impact comes from ~20% of causes. The table that funds improvement. | F |
| **PCI DSS** | Security standard for payment card data. Constrains how payment incidents are handled and logged. | J, K |
| **PDCA** | Plan-Do-Check-Act, the Deming cycle. **Everyone skips Check.** | I |
| **PII** | Personally Identifiable Information. A leak is a regulatory event, not just an incident. | J |
| **PIR** | Post-Incident Review — structured look-back. Without owned, dated, tracked actions it's a therapy session. | D |
| **PNR** | Passenger Name Record — the booking file for a trip. The currency of incident examples. | A |
| **Priority** | The order we work in. Derived from **impact × urgency**. | C |
| **Priority inflation** | Everything becoming "P1", which makes priority meaningless. | C |
| **Proactive problem management** | Finding and fixing causes *before* an outage, from trend data. The seniority signal. | F |
| **Problem** | The cause, or potential cause, of one or more incidents. | F |
| **Promoter** | An NPS respondent scoring 9–10. | G |
| **Proxy** | An intermediary that forwards requests. Causes odd, environment-specific failures. | J |
| **PSS** | Passenger Service System — the airline's engine room: inventory → ticketing → DCS. | A |

## Q

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **QBR** | Quarterly Business Review — senior stakeholders, business outcomes, strategic decisions. | H |
| **Queue (message)** | Holds work to be done later, decoupling systems. An inbox tray. Backlogs build invisibly. | J |

## R

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **RACI** | Responsible, Accountable (**exactly one**), Consulted, Informed. | B |
| **RCA** | Root Cause Analysis — the structured hunt for the real underlying cause. | F |
| **Redundancy** | Spare capacity ready to take over. | J |
| **Region** | A geographic location of cloud infrastructure. | J |
| **Reopen rate** | Percentage of tickets closed then reopened. Exposes premature closure. | G |
| **Resolution** | The underlying fault is genuinely gone. | C, G |
| **Response** | How fast we acknowledge and start work. Fully within provider control. | G |
| **Restoration** | The customer can operate again — possibly via workaround, with the fault still present. | C, G |
| **Retry storm** | Mass retries adding load to an already struggling system. Everyone jabbing the lift button. | J |
| **RPO** | Recovery Point Objective — how much **data** you can afford to lose. | J |
| **RTO** | Recovery Time Objective — how **long** you can be down. | J |
| **Runbook** | Step-by-step operational procedure followed under pressure. | I |
| **RUM** | Real User Monitoring — the honest truth about how the service feels to actual users. | J |

## S

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **SAE** | Service Account Executive — owns the outcome, not the repair. | A |
| **Scribe** | The bridge role that records timeline, decisions and actions. Does not diagnose. | D |
| **Service** | A means of enabling value by delivering an outcome the customer wants, without them owning the cost and risk. | B |
| **Service acceptance criteria** | The checklist a service must pass before going live. | I |
| **Service catalogue** | The published menu of what the provider offers. | K |
| **Service credits** | Financial remedy for a missed SLA. Usually capped far below real loss — a **relationship warning**, not a payment dispute. | G, K |
| **Service request** | A routine, pre-approved ask. Not an incident. | B |
| **Service transition** | Moving a service into live operation such that it can actually be supported. | I |
| **Severity** | The technical seriousness of a fault. **Not the same as priority.** | C |
| **Shift-left** | Moving resolution closer to the user — self-service and Tier 1 handle more. | K |
| **Six Sigma** | Data-driven reduction of variation and defects. Core insight: consistency is a feature. | I |
| **SLA** | Service Level Agreement — the contractual promise to the customer. | G |
| **SLO** | Service Level Objective — the internal target, stricter than the SLA. The gap is your buffer. | G |
| **SRE** | Site Reliability Engineering — engineering-led reliability. Complements rather than replaces ITIL. | K |
| **Stakeholder** | Anyone affected by, or able to affect, the service. | E |
| **Swarming** | Experts collaborating immediately instead of escalating up tiers. Swarm the novel, tier the known. | K |
| **Swiss cheese model** | Incidents happen when weaknesses in multiple defensive layers line up. | F |
| **Synchronous** | The caller waits for an answer. A phone call. Fails *loudly* via timeouts. | J |
| **Synthetic check** | A robot simulating a user journey to detect failure before users do. | J |

## T

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Tabletop exercise** | Rehearsing a crisis scenario without touching production. Cheapest resilience investment there is. | K |
| **Throughput** | Transactions successfully processed per unit of time. | J |
| **Ticket hygiene** | Accurate categorisation, timestamps, impact data and resolution notes. Bad hygiene destroys trend analysis. | C |
| **Tier 1 / 2 / 3** | Service desk / specialist support / engineering and product. | B |
| **Timebox** | A fixed period after which an investigation must report back either way. Prevents silent rabbit holes. | D |
| **Timeout** | Giving up waiting for a response. Causes cascading failures under load. | J |
| **TLS / SSL certificate** | Proves a server's identity and encrypts traffic. Expiry causes instant, total, predictable outages. | J |
| **Toil** | Manual repetitive work that scales with load. Quantify it to make automation fundable. | I, K |
| **Trace** | The path of a single request through many systems. Shows *where* time is spent. | J |
| **Trigger** | The immediate event that set off an incident — distinct from the root cause. | F |

## U

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **UC** | Underpinning Contract — a third party's contractual commitment to you. | G |
| **Urgency** | How fast damage grows, or how soon a deadline bites. | C |
| **Utility** | Fit for **purpose** — does it do the job? | B |
| **Utilisation** | How much of available capacity is being used. | J |

## V

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **Value stream mapping** | Mapping every step to find where time is actually lost. Usually queueing, not work. | I |
| **Verification (improvement)** | Proving with data that incidents actually declined. Deployed ≠ solved. | I |
| **Verification (incident)** | Confirming restoration with affected **users**, not just monitoring. | C |
| **VPN** | An encrypted tunnel between networks. | J |

## W

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **War room** | See *bridge call*. | D |
| **Warranty** | Fit for **use** — does it do the job reliably, quickly, securely? Most service work lives here. | B |
| **Watermelon report** | Green on the outside, red on the inside — metrics all met while the customer is unhappy. | C, H |
| **Weight & balance** | The safety-critical calculation ensuring an aircraft is loaded correctly. No workaround. | A |
| **Workaround** | A temporary means of operating while the fault still exists. Must be **practical**, not merely valid. | C, D |

## X

| Term | Plain-English meaning | Part |
|------|----------------------|------|
| **XLA** | Experience Level Agreement — targets based on user experience rather than system metrics. The cure for watermelons. | K |

---

## The 25 terms you must know cold

If time is short, these are the ones an interviewer is most likely to test directly.

Incident · Problem · Workaround · Restoration vs Resolution · Priority (impact × urgency) · Severity · Major incident · Escalation (functional vs hierarchical) · SLA · SLO · OLA · Availability / the nines · MTTR · MTBF · CSAT · NPS · PIR · Blameless · Known error · RCA · 5 Whys · CMDB · RACI · RTO / RPO · GDS / PSS / PNR

---

[← Master index](../Amadeus%20Service%20Account%20Executive%20-%20Study%20Guide.md) · [Appendix B — Worked Scenario →](Appendix-B-worked-scenario.md)
