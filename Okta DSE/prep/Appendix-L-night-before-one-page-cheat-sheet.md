# Appendix L - Night-Before One-Page Cheat Sheet

> Purpose: The last thing you read. Everything that must be instantly available, compressed. **Do not learn anything new from here — only confirm what you already know.**

*Part of the* **[Okta Developer Support Engineer - Complete Study Guide](../Okta%20Developer%20Support%20Engineer%20-%20Complete%20Study%20Guide.md)**

---

## 🔴 The Seven Failure Patterns

**Most identity failures are one of these. Say the list aloud.**

> **expiry · identifier · size · silence · state · cookie · scope**

| # | Pattern | Signature |
|---|---|---|
| 1 | **Expiry** | Worked, then stopped, at an interval |
| 2 | **Unstable identifier** | Duplicate accounts; lost permissions |
| 3 | **Size** | Fails for *some* users only |
| 4 | **Silent absence** | Nobody noticed for weeks |
| 5 | **State across requests** | Intermittent; load-balancer dependent |
| 6 | **Cookie context** | One browser works, another does not |
| 7 | **Missing scope / context** | Valid token, 403 |

---

## 🔵 The Five Narrowing Questions

1. **Where** does it fail — browser, redirect, token endpoint, or API?
2. Does it work for **anyone** right now? *(the highest-value question)*
3. **When** did it start, and what changed then?
4. Is it **all** users or **some**? What is different about the some?
5. What is the **exact** error, and **who** produced it?

> **"Where does it fail" narrows faster than anything else.**

---

## Protocol Cues

| Concept | One line |
|---|---|
| **OAuth 2.0** | Delegated **authorisation**. Not a login protocol |
| **OIDC** | OAuth **+ who** — adds the ID token |
| **SAML** | XML federation via browser POST. Enterprise workforce |
| **SCIM** | **Provisioning.** Federation logs in; SCIM creates the account |
| **PKCE** | Stops a stolen auth code being usable by anyone else |
| **ID token** | For the **app**. 🔴 Never send it to an API |
| **Access token** | For the **API** |
| **Refresh token** | Renewal. Highest-value theft target |
| **`sub`** | The stable user ID. **Not email** |
| **`aud`** | Who it is for. Wrong = valid but rejected |
| **`exp`** | Pattern #1 |
| **`kid`** | How key rotation works |
| **401** | Not authenticated |
| **403** | Authenticated, not permitted |

**Which flow?**

| Situation | Flow |
|---|---|
| Server-side web app | Authorization code |
| SPA or mobile | **Code + PKCE** |
| No user | Client credentials |
| TV / CLI | Device authorization |
| 🔴 Implicit, password grant | **Never recommend** |

---

## Top Failure Catalogue

| Symptom | Cause |
|---|---|
| Everyone, suddenly, no deploy | **Certificate or secret expiry** |
| Some users, seemingly random | **Group / token size** |
| One browser only | **`SameSite` / third-party cookies** |
| New users only | Provisioning, not authentication |
| Intermittent, load-balanced | **State not shared across nodes** |
| Valid token, 403 | Scope, audience, or organisation |
| Duplicate accounts | **Transient or email NameID** |
| Nobody noticed for weeks | **Silent failure — pattern #4** |
| `invalid_grant` | **Code reused** (count token calls in the HAR) |
| Every token failed at once | **Key rotation + over-cached JWKS** |
| Access token is not a JWT | **No `audience` requested** |
| Attributes missing in SAML | **Attribute `Name` mismatch** |
| No tenant log entry at all | **Never reached the tenant — DNS/proxy** |
| Works in browser, fails from server | **Missing intermediate certificate** |
| Only this one machine | **Clock skew** |
| `52e` in AD | **Bad password** (user exists) |
| LDAP zero results | **Base DN or scope** — not permissions |

---

## Okta Facts

| Fact | |
|---|---|
| Founded | **2009**, **Todd McKinnon** and **Frederic Kerrest** |
| Positioning | **"The World's Identity Company™"** |
| Tagline | **"Securing every identity. Human & machine."** |
| Vision | **"To free everyone to safely use any technology"** |
| Platforms | **Okta Platform** (workforce) · **Auth0 Platform** (customer) |
| **This role** | **Customer identity = Auth0** |
| Scale | Two-thirds of the Fortune 100 |
| HQ | San Francisco; 15 countries |
| Programmes | Secure Identity Commitment · Okta for Good · Okta Ventures · Oktane |

**The four values:**

> **Love our customers** · **Always secure. Always on.** · **Build and own it** · **Drive what's next**

---

## Your Seven Stories

| # | Story | Value it shows |
|---|---|---|
| 1 | Difficult customer | Love our customers |
| 2 | Complex diagnosis | Build and own it |
| 3 | **Being wrong** | Self-awareness |
| 4 | Saying no | Always secure. Always on. |
| 5 | Unasked initiative | Build and own it |
| 6 | Disagreement | Collaboration |
| 7 | Learning fast | Drive what's next |

**STAR proportions:** **15s / 10s / 45–60s / 15s** — **the Action is the answer.**
**"I" where you acted.** **Every Result ends with a learning.**
🔴 **Redact.** Shape, not names.

---

## The Four Answers That Must Be Fluent

**1. Explain a protocol aloud** — accurate and accessible, 90 seconds, no jargon.

**2. "You have no Okta/Auth0 experience"**

> *"That's accurate — I haven't used either in production. What I did about it is build a free-tier tenant and work through the flows properly: authorization code with PKCE, a SAML connection, a SCIM provisioning run, Actions with custom claims, and reading tenant logs until I could tell the failure shapes apart. What I bring is several years of business-critical escalation work and the substrate identity runs on — Active Directory, LDAP, Entra ID, TLS, DNS, HTTP, HAR analysis — which is where a large share of identity failures actually live. And to be honest about the boundary: what labs can't give me is architecture judgement, which needs real customer patterns over six to nine months."*

**3. "Why this role?"**

> *"It's the intersection of what I already do and what I want to go deeper into. Several years of enterprise escalations where the hardest problems were nearly always identity, networking, or the boundary between them. I want to be in the product where that's the whole job — and the developer-facing side specifically, because the person on the other end is building something."*

**4. "First ninety days?"** — Learn failure shapes → own routine work and write it up → contribute deflection and product feedback. **Not claiming architecture guidance at 90 days.**

---

## Your Three Questions

1. *"How does someone get recognised here — is writing things up and helping colleagues visible, or is it measured output?"*
2. *"What happens when someone declines a customer request on security grounds?"*
3. *"How do support findings reach product?"*

> 🔵 **A candid "that's something we're working on" is a good answer to receive.**

---

## Delivery

| ✅ Do | 🔴 Do not |
|---|---|
| **Two-second pause before answering** | Answer before the question finishes |
| **Conclusion first**, then two supporting points | Build up to the conclusion |
| **60–90 seconds** | Run past two minutes |
| **Then STOP** | Keep adding qualifications |
| **State gaps as facts** | Apologise for them |
| **"Let me restart that"** | Push through a lost sentence |
| Narrate on diagnosis questions | **Guess the cause** |
| Ask for clarification | Answer the wrong question fluently |

> **Silence after your answer is not a problem to fill.**

---

## Three Shapes of Technical Question

| Shape | Opening move |
|---|---|
| **"What is X?"** | One-line answer, then why it exists |
| **"X vs Y?"** | **The difference that matters**, then when to choose each |
| **"Users can't log in…"** | 🔵 **Narrate.** Questions, hypothesis, what would disprove it |

> 🔴 **Guessing right on a diagnosis question scores badly.** The method is what is assessed.

---

## If You Don't Know

> *"I haven't hit that specifically. What I do know that's nearby is \[X\]. Reasoning from that, I'd expect \[Y\], and I'd confirm it by \[specific check\]."*

**Never bluff.** In developer support, **a confident wrong answer gets implemented.**

---

## 🔴 Never Recommend

- Disabling TLS or certificate verification · `-k` · `rejectUnauthorized: false`
- `alg: none`, or trusting the `alg` header
- Implicit flow · password grant
- Tokens in URLs · refresh tokens in `localStorage`
- Embedding a login form in an app
- Disabling browser web security
- Authorising from `user_metadata`
- Sharing an unredacted HAR or any token

---

## Tonight

| Do | Do not |
|---|---|
| Read this page | Learn anything new |
| Re-read your seven stories | Rewrite your answers |
| Check setup, route, timing | Cram the question bank |
| **Sleep** | "One more section" |

**Tomorrow:** setup checked · water · arrive early · two-second pause · answer, then stop.

---

## The Position You Are Arguing

**Several years of real escalation work. A strong technical substrate that identity sits on top of. Deliberately lab-built product knowledge. One named gap — architecture judgement — that takes six to nine months and has no shortcut.**

**That is credible, defensible, and genuinely strong.**

**Go and use it.**

---

*Return to:* **[Okta Developer Support Engineer - Complete Study Guide](../Okta%20Developer%20Support%20Engineer%20-%20Complete%20Study%20Guide.md)**
