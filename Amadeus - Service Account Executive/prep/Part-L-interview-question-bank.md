# Part L — Interview Question Bank

[← Master index](../Amadeus%20Service%20Account%20Executive%20-%20Study%20Guide.md) · [← Part K](Part-K-misc-and-deeper-topics.md) · **Part L of M** · [Part M →](Part-M-behavioral-and-closing.md)

> Section goal: test whether the knowledge from Parts A–K is *retrievable under pressure*. Reading is recognition; answering is recall. These are different skills, and only one of them is tested in an interview.

Covers index item **37**.

**How to use this bank:**
1. Cover the answer. Say your response **out loud**. Compare.
2. Mark each question in the [self-quiz tracker](#self-quiz-tracker) at the end.
3. Any question you can't answer in 30 seconds, revisit the referenced Part.
4. Answers here are **compressed prompts**, not scripts. Speak them in your own words — a memorised answer sounds memorised.

**Distribution:** 25 Basic · 25 Intermediate · 70 Advanced · plus Behavioral and Closing.
**Part references** (A–K) point to the corresponding section file in this folder.

---

## Section 1 — Basic (Q1–Q25)

Foundational definitions. You should answer these instantly and without hesitation.

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 1 | What is an incident? | An unplanned interruption to a service **or a reduction in its quality**. Slow counts as broken. | C |
| 2 | What is a problem? | The cause, or potential cause, of one or more incidents. | F |
| 3 | What is the goal of incident management? | Restore service as fast as possible — not to find the cause. | C |
| 4 | What is the goal of problem management? | Prevent recurrence by removing the underlying cause. | F |
| 5 | What is a workaround? | A temporary means of restoring the ability to operate while the fault still exists. | C |
| 6 | What is an SLA? | A contractual agreement with the customer defining service targets, often with credits. | G |
| 7 | What is an SLO? | An internal target, normally stricter than the SLA, providing buffer. | G |
| 8 | What is an OLA? | An internal agreement between teams that underpins the SLA. | G |
| 9 | What is ITIL? | The most widely adopted framework of ITSM best practice — vocabulary and a checklist. | B |
| 10 | What does priority depend on? | Impact × urgency. | C |
| 11 | Difference between impact and urgency? | Impact = how much damage. Urgency = how fast it grows / how soon a deadline bites. | C |
| 12 | What is a major incident? | A high-impact incident that triggers a *different, coordinated* process, not just a bigger one. | D |
| 13 | What is escalation? | Deliberately adding capability or authority because the current path won't deliver in time. | E |
| 14 | Two types of escalation? | Functional (more skill) and hierarchical (more authority). | E |
| 15 | What is a PIR? | Post-incident review — structured look-back producing owned, dated actions. | D |
| 16 | What does MTTR stand for? | Mean time to restore/resolve/repair — always clarify which "R". | G |
| 17 | What is MTBF? | Mean time between failures — a measure of stability. | G |
| 18 | What is CSAT? | Customer satisfaction, usually a 1–5 survey score on an interaction. | G |
| 19 | What is NPS? | Net Promoter Score: %Promoters (9–10) − %Detractors (0–6). | G |
| 20 | What is a known error? | A problem with a documented cause and usually a workaround, fix pending. | F |
| 21 | What is a CMDB? | A record of configuration items and their relationships — enables impact assessment. | B |
| 22 | What is RACI? | Responsible, Accountable (exactly one), Consulted, Informed. | B |
| 23 | What is a GDS? | Global Distribution System — connects airline inventory to travel sellers. | A |
| 24 | What is a PNR? | Passenger Name Record — the booking file for a trip. | A |
| 25 | What does 99.9% availability allow monthly? | About 43 minutes of downtime. | G |

---

## Section 2 — Intermediate (Q26–Q50)

Applied understanding. You should be able to explain *why*, not just *what*.

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 26 | Why should the SLO be stricter than the SLA? | To create buffer; equal targets mean a breach on any bad day. | G |
| 27 | Why is an SLA only as strong as its OLAs and UCs? | If the vendor contract allows 8 hours, a 4-hour SLA is undeliverable by arithmetic. | G |
| 28 | Why is "slow" treated as "broken"? | For real-time systems, degradation blocks the business process just as effectively as an outage. | A, C |
| 29 | Three qualifiers needed for any availability figure? | Measurement window, measurement point, exclusions. | G |
| 30 | Why do averages mislead in service reporting? | They hide outliers — one site down 30 hours can still average 99%. Show distribution. | G |
| 31 | Leading vs lagging indicator? | Lagging = what happened (scale). Leading = what's coming (calorie intake). | G |
| 32 | Give three leading indicators. | Change failure rate, backlog ageing, capacity headroom, near-misses, open problems without fix dates. | G |
| 33 | What is a watermelon report? | Green metrics, red customer — measuring compliance rather than outcome. | C, H |
| 34 | Why pair MTTR with reopen rate? | MTTR alone is gamed by premature closure; reopen rate exposes it. | G |
| 35 | What is the difference between output and outcome? | Output = what we produced. Outcome = what the customer achieved. | B |
| 36 | Utility vs warranty? | Utility = fit for purpose (what). Warranty = fit for use (how well). Service lives in warranty. | B |
| 37 | Why must there be exactly one "A" in RACI? | Two accountable people (or none) is the most common cause of stalled incidents. | B |
| 38 | Standard, normal and emergency change? | Pre-approved / assessed case-by-case / expedited for urgent impact. | F |
| 39 | Why is change failure rate a leading indicator? | Changes cause a large share of incidents, so it predicts future instability. | F, G |
| 40 | What is a freeze period and why do airlines use them? | A ban on non-essential change during peaks, to reduce risk when tolerance is lowest. | F |
| 41 | What is Early Life Support? | Heightened support immediately after go-live, ended by **criteria**, not the calendar. | I |
| 42 | Why does the update cadence matter more than the update content? | Silence reads as neglect; a reliable "no change" update preserves trust. | C, E |
| 43 | Why commit to the next update time, not the fix time? | You control when you communicate; you don't control when engineering succeeds. | C |
| 44 | What are RTO and RPO? | RTO = how long down. RPO = how much data lost. Independent; both cost money. | J |
| 45 | Sync vs async failure modes? | Sync fails loudly (timeouts). Async fails silently (growing backlog). | J |
| 46 | Why is certificate expiry a classic outage? | Instant, total, simultaneous, predictable — root cause is process ownership, not technology. | J |
| 47 | What is PDCA and which stage is skipped? | Plan-Do-Check-Act; **Check** is skipped, so improvement is never proven. | I |
| 48 | What is a CSI register? | A prioritised improvement portfolio with owners, dates and verification. | I |
| 49 | What is the Pareto principle in incident analysis? | ~80% of impact from ~20% of causes; ranks where to invest. | F |
| 50 | Why is proactive problem management the seniority signal? | Anyone can investigate after a disaster; preventing one requires trend analysis and influence. | F |

---

## Section 3 — Advanced (Q51–Q120)

Judgement, trade-offs and scenarios. These are where interviews are won.

### Incident and major incident (Q51–Q70)

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 51 | Walk through managing a major incident end to end. | Declare → mobilise roles → facts → impact → holding statement → checkpoints → parallel workaround/fix → verify with users → close loop → PIR. | D |
| 52 | When would you *not* apply a workaround immediately? | If it destroys diagnostic evidence — capture diagnostics fast first, and make the trade-off explicit to the customer. | C, D |
| 53 | Why shouldn't the SAE lead diagnosis? | Going deep means nobody assesses impact, protects the peak window, or communicates. Widest view, not deepest. | D |
| 54 | How do you handle an exec who joins the bridge? | 60-second impact summary, then move them to a dedicated briefing with a committed cadence. | D |
| 55 | Two teams blaming each other mid-incident — what do you do? | Ban causation debate; assign cause at the PIR. Each validates/eliminates their component in a timebox. | D |
| 56 | What makes a workaround unusable? | Operational impracticality — e.g. 4 min/passenger × 300 passengers, 50 min to departure. | D |
| 57 | Engineering has gone silent for 40 minutes. Intervene how? | Call a checkpoint: what's known, what's being tested, who owns it, what's the timebox. Split deep work from coordination. | D |
| 58 | How do you decide to declare a major incident? | Pre-agreed triggers; asymmetric cost — declare when unsure, stand down early if wrong. | D |
| 59 | Customer found out before you told them. Response? | Own it as a distinct failure; diagnose whether the gap was detection, decision or mechanism; agree a notification threshold and max time-to-contact. | D |
| 60 | How do you hand over a live major incident across regions? | Written handover **plus** verbal overlap: state, timeline, ruled out, hypothesis, owners, commitments, sensitivities. | D |
| 61 | What is priority inflation and how do you manage it? | Everything becomes P1. Resolve with objective questions (which process, how many), act urgently, refine criteria at the review. | C |
| 62 | Why is verification with users mandatory before closure? | Monitoring shows components healthy; only users know the business process works. Prevents reopens. | C |
| 63 | Six elements of a good incident update? | Status, impact, current action, workaround, what's needed from them, next update time. | C |
| 64 | Three rules of incident writing? | No jargon, no blame, no speculation. | C |
| 65 | How do you avoid the hot-potato ticket? | Single-threaded ownership plus explicit written handovers between teams. | C |
| 66 | What is a blameless PIR and what makes it fail? | Investigate the system, not the person; it fails when actions have no owner, date, or tracking. | D |
| 67 | Six time metrics a PIR should produce? | Time to detect, declare, engage, workaround, resolve, first customer update. | D |
| 68 | Same fault at 03:00 and at 07:00 — same priority? | No. Impact may be identical but urgency differs with the peak window. | C |
| 69 | What are the parallel tracks in a major incident? | Restore the business (workaround) **and** remove the fault (fix). Never sequential. | D |
| 70 | How do you keep composure in a chaotic bridge? | Fall back on structure: facts → impact → owners → timebox → checkpoint. Structure replaces adrenaline. | D |

### Escalation, communication, relationship (Q71–Q88)

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 71 | Diagnostic question for choosing escalation type? | "What's missing — a skill or a decision?" Skill → functional. Decision → hierarchical. | E |
| 72 | How do you escalate without damaging relationships? | Warn first, escalate the issue not the person, bring facts and a specific ask, close the loop. | E |
| 73 | What's in a good escalation message? | Issue, impact, status, what's been tried, the specific ask with a deadline, consequence if unresolved. | E |
| 74 | Why is explanation early perceived as defensiveness? | It signals you're protecting yourself rather than registering their impact. Acknowledge first. | E |
| 75 | Best question to ask an escalating customer? | "What do you need to be able to tell your leadership?" — disarms and produces a deliverable. | E |
| 76 | How do you deliver bad news? | Yourself, early, direct, with a plan and a revised checkpoint. Late bad news reads as concealment. | E |
| 77 | How do you rebuild trust after a serious failure? | Small, visible, reliably-kept commitments — not bigger promises. Frequency proves reliability. | E |
| 78 | How do you avoid the dual-loyalty trap? | Be loyal to the outcome. Over-siding either way destroys the credibility that makes you useful. | E |
| 79 | How do you make internal advocacy credible? | Quantify it; include praise as well as complaints; don't commit teams to things they haven't agreed. | E |
| 80 | Customer bypasses you to contact engineers — response? | Treat it as a symptom that your channel felt slow. Don't block; stay copied; make your channel fastest. | E |
| 81 | How do you say "no" to a customer? | Name the constraint, offer a concrete alternative, avoid vagueness. Customers accept "no" better than ambiguity. | E |
| 82 | What are the signals a relationship is deteriorating? | Seniors stop attending, more attendees appear, forensic questions, execs copied on routine mail, requests route around you, sudden contract interest. | H |
| 83 | How do you manage stakeholders with conflicting priorities? | Map power/interest; make the conflict explicit with impact data; escalate as a decision with options, not as a problem. | E |
| 84 | Why does a customer invoke service credits? | Usually a relationship warning, not a money claim — credits are capped far below real loss. | K |
| 85 | You have nothing new at the promised update time. What do you send? | The update — what's still true, what's been ruled out, who's working it, next update time. | E |
| 86 | How do you handle a request you can't meet at all? | Explain the constraint, propose the nearest achievable alternative, and make the decision visible rather than silent. | E |
| 87 | Why must there be one voice to the customer? | Contradictions between internal people read as incompetence and destroy confidence faster than the fault. | D |
| 88 | When is escalation *not* appropriate? | When nothing is missing but your patience — supply better impact data and chase instead. | E |

### Analysis, problem management, improvement (Q89–Q104)

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 89 | Name five RCA techniques and when to use each. | 5 Whys (linear), Ishikawa (multi-category), fault tree (combinatorial), Pareto (prioritise), Kepner-Tregoe (ambiguous scope). | F |
| 90 | Two guard-rails for 5 Whys? | Stop at the actionable structural cause; run parallel chains (failed / not detected / slow to restore). | F |
| 91 | Explain Kepner-Tregoe is/is-not. | Define the problem by contrast — what IS vs IS NOT affected across what/where/when/extent — to narrow the search. | F |
| 92 | Is there always one root cause? | Rarely. Swiss cheese: trigger, root, contributing, systemic. Fix prevention, detection and response. | F |
| 93 | Highest-yield first question in any incident? | "What changed in the preceding window?" | F, J |
| 94 | How do you make preventive actions actually happen? | One named owner, a date, specific wording, customer-review visibility, then verification by data. | F |
| 95 | When is a problem genuinely closed? | When the incident category measurably declined — not when the fix was deployed. | F, I |
| 96 | Correlation vs causation — how do you strengthen a claim? | Mechanism, temporality, dose-response, control, reversal. | G |
| 97 | How do you know a trend isn't noise? | Sample size, rolling windows, absolute numbers alongside percentages, comparable periods, a plausible mechanism. | G |
| 98 | Five patterns to hunt in incident data? | Repetition, concentration, timing, trajectory, correlation. | G |
| 99 | Why does variation matter as much as the average? | Consistency is a feature — same 4-hour average, but predictable beats erratic. Report percentiles. | I |
| 100 | Four gates where improvement dies? | Not recorded, not prioritised, not owned, not verified. | I |
| 101 | Why deliver a quick win early? | Improvement programmes earn political capital through evidence; a small proven win funds the big one. | I |
| 102 | What is toil and why target it? | Manual repetitive work that scales with load; quantifying it ("15 specialist hours/month") makes it fundable. | I, K |
| 103 | What's the most valuable improvement artefact? | The verification slide: before/after data proving the category actually declined. | I |
| 104 | How do you break the firefighting trap? | Evidence (trend data) plus external visibility (customer-tracked actions) so prevention competes with urgency. | F |

### Governance, transition, technical, industry (Q105–Q120)

| # | Question | Answer / hint | Part |
|---|----------|---------------|------|
| 105 | How do you structure a monthly service review? | Opening → **previous actions** → performance → majors → problems → forward look → improvements → customer input → new actions. | H |
| 106 | Why do previous actions go near the front? | Proves reliability, creates internal pressure, prevents the end-of-meeting ambush. | H |
| 107 | What makes a report readable? | Inverted pyramid; page one stands alone; number → context → cause → action. | H |
| 108 | The credibility equation? | Accuracy × Candour × Consistency — a product, so any zero zeroes it. | H |
| 109 | Why include a declined item in "you said, we did"? | A list of only successes reads as marketing; a transparent decline makes the whole list believable. | H |
| 110 | Two questions before any go-live? | Who's on the phone at 3am on day one, by name? Has a non-author followed the runbook end to end? | I |
| 111 | Why do transitions fail? | Context-free documentation, no named owner, unrealistic testing, unknown real workflow, calendar-based ELS exit. | I |
| 112 | What's the SAE's unique contribution to a transition? | Customer operational reality — peaks, real workflows, intolerable failure modes, actual decision-makers. | I |
| 113 | The 3am test? | Can a tired responder who has never seen the system follow this article successfully under pressure? | I |
| 114 | What is a cascading failure and its defences? | One slow dependency exhausts callers upstream; defences are circuit breakers and graceful degradation. | J |
| 115 | Monitoring vs observability? | Monitoring checks known thresholds; observability lets you ask new questions about novel failures. | J |
| 116 | Best SAE questions on a technical bridge? | What changed? Failing or slow? What's it waiting on? All users or a subset? Anything queued? Do users actually see it? | J |
| 117 | What changes when an incident becomes a suspected breach? | Legal/security engage immediately, notification clocks start, evidence may outrank restoration, comms become strictly factual. | J |
| 118 | Why are capacity failures predictable? | Utilisation trends warn for months; failure only appears sudden. Watch headroom against the peak calendar. | J |
| 119 | Explain error budgets. | 100% − SLO. Ship freely while budget remains; when exhausted, reliability work takes priority. Arithmetic, not argument. | K |
| 120 | Why does automation reduce the value of workarounds? | Automated journeys (e.g. biometric boarding) often have no manual fallback at equivalent throughput — availability matters more. | K |

---

## Section 4 — Behavioral (STAR)

These need **your own examples**. Part M covers how to build them. Use these prompts to prepare stories in advance — do not improvise them on the day.

| # | Prompt | What they're testing |
|---|--------|---------------------|
| B1 | Tell me about a time you managed a critical incident. | Composure, coordination, structure |
| B2 | Describe a time you handled an angry or escalating customer. | Emotional regulation, ownership |
| B3 | Tell me about a time you had to deliver bad news. | Candour, timing, courage |
| B4 | Describe a time you influenced people who didn't report to you. | Influence without authority |
| B5 | Tell me about a time you identified a pattern others had missed. | Analytical capability, proactivity |
| B6 | Describe an improvement you drove end to end. | Initiative, follow-through, verification |
| B7 | Tell me about a time you disagreed with a colleague or a customer. | Professional conflict handling |
| B8 | Describe a time you made a mistake. | Accountability, learning |
| B9 | Tell me about a time you worked across time zones or cultures. | Global working maturity |
| B10 | Describe a time you had to prioritise between competing urgent demands. | Judgement under pressure |
| B11 | Tell me about a time you had to learn something complex quickly. | Learning agility |
| B12 | Describe a time you said no to a customer. | Boundary setting, alternatives |
| B13 | Tell me about a time a fix didn't work. | Resilience, honesty, iteration |
| B14 | Describe how you built a relationship with a difficult stakeholder. | Relationship investment |
| B15 | Tell me about a time you had to work outside normal hours. | Commitment, sustainability |

---

## Section 5 — Closing questions

| # | Question | Approach |
|---|----------|----------|
| C1 | Why do you want this role? | Connect the role's actual content (major incidents, customer ownership, improvement) to what you demonstrably enjoy and do well. |
| C2 | Why this company / this industry? | Show you understand the domain: high switching costs, low failure tolerance, service as part of the product. |
| C3 | Why are you leaving your current position? | Forward-looking and positive. Never criticise a current employer. |
| C4 | What are your strengths? | Two or three, each with a one-line proof point. |
| C5 | What's your biggest weakness? | A real one, with the concrete mitigation you've built. |
| C6 | Where do you see yourself in five years? | Growth within the service/customer domain — depth or leadership. |
| C7 | How do you handle pressure? | Describe your *structure*, not your feelings. |
| C8 | What would your first 90 days look like? | 30 learn the service, 60 learn the business rhythm, 90 deliver a first improvement. |
| C9 | Are you comfortable with weekend work? | Yes, with a mature framing about sustainable coverage design. |
| C10 | Do you have any questions for us? | **Always yes.** See Part M for a prepared list. |

---

## Self-quiz tracker

Score each pass: ✅ answered confidently · ⚠️ hesitant or incomplete · ❌ couldn't answer.
**Target: two consecutive full passes with zero ❌ and fewer than five ⚠️.**

| Block | Questions | Pass 1 | Pass 2 | Pass 3 | Weak areas to revisit |
|-------|-----------|--------|--------|--------|----------------------|
| Basic | 1–25 | | | | |
| Intermediate | 26–50 | | | | |
| Adv: Incident & major | 51–70 | | | | |
| Adv: Escalation & comms | 71–88 | | | | |
| Adv: Analysis & improvement | 89–104 | | | | |
| Adv: Governance & technical | 105–120 | | | | |
| Behavioral | B1–B15 | | | | |
| Closing | C1–C10 | | | | |

### Diagnostic guide

| Pattern in your results | What it means | What to do |
|-------------------------|---------------|------------|
| Basic strong, advanced weak | You have vocabulary but not judgement | Re-read Parts D, E, F; practise scenarios aloud |
| Advanced strong, basic hesitant | You'll stumble on easy openers | Drill definitions until instant |
| Technical weak (105–120) | Credibility risk with engineers | Re-read Part J; focus on the bridge questions |
| Behavioral unprepared | The most common cause of failed interviews | Go to Part M and write the stories out |
| Confident reading, weak speaking | Recognition ≠ recall | Answer out loud; record yourself |

---

*Next suggested section:* **[Part M — Behavioral & Closing](Part-M-behavioral-and-closing.md)** — the final Part: how to build STAR stories, answer the "why" questions, ask good questions, and walk in prepared.

---

[← Master index](../Amadeus%20Service%20Account%20Executive%20-%20Study%20Guide.md) · [← Part K](Part-K-misc-and-deeper-topics.md) · [Part M →](Part-M-behavioral-and-closing.md) · [Glossary](Appendix-A-glossary.md) · [Worked scenario](Appendix-B-worked-scenario.md)
