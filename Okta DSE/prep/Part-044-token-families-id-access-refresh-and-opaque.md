# Part 044 - Token Families: ID, Access, Refresh, and Opaque

> Section goal: Learn the four token types, what each is actually for, who is supposed to validate each one, and the misuse patterns that follow from confusing them — because "sending the wrong token to the wrong place" is one of the highest-volume ticket categories in identity support.

Covers index item **044**. Maps to JD signals: *basic security concepts*, *strong analytical and problem-solving skills*, *experience with troubleshooting web applications*, *communicate technical concepts clearly*, and *promote best practices*.

---

## 1. Start From Zero: Four Tokens, Four Jobs

Developers new to OAuth/OIDC often assume "the token" is one thing. It is four, with different audiences, lifetimes, and rules.

```mermaid
flowchart TD
    AS["Authorization server"] --> ID["ID TOKEN<br/>'Who the user is'<br/>For: YOUR APP<br/>OIDC"]
    AS --> AT["ACCESS TOKEN<br/>'What may be done'<br/>For: AN API<br/>OAuth 2.0"]
    AS --> RT["REFRESH TOKEN<br/>'Get more tokens'<br/>For: THE AUTH SERVER<br/>OAuth 2.0"]
    ID --> IDU["App reads claims, creates a session, then is DONE with it"]
    AT --> ATU["Sent to the API in Authorization: Bearer"]
    RT --> RTU["Sent ONLY back to the token endpoint. Never to an API."]
    IDU --> Rule1["NEVER send an ID token to an API"]
    ATU --> Rule2["NEVER read an access token's claims in your app"]
    RTU --> Rule3["NEVER expose a refresh token to a browser without rotation"]
```

| Token | Answers | Audience (`aud`) | Validated by | Typical life |
|---|---|---|---|---|
| **ID token** | *Who is this user?* | Your **client ID** | Your **application** | Minutes |
| **Access token** | *What may the bearer do?* | The **API identifier** | The **API** | 5–60 minutes |
| **Refresh token** | *May I have more tokens?* | The authorization server | The **auth server** | Days to months |
| **Opaque token** | (Either of the above, as a reference) | n/a | The issuer, via introspection | Varies |

> **Analogy.** An ID token is a **membership card** shown at the door to prove who you are. An access token is a **ticket for a specific event** at a specific venue. A refresh token is your **booking reference**, which you show only at the box office to collect more tickets. Showing your booking reference to the usher at the door achieves nothing and reveals something you should have kept.
>
> **Where it stops:** a physical usher would hand it back and explain. An API given the wrong token returns 401 with no explanation at all, which is why this Part exists.

---

## 2. The ID Token

**Defined by OIDC, not OAuth.** It is a statement *about a user*, issued to *your application*.

```json
{
  "iss": "https://tenant.us.auth0.com/",
  "sub": "auth0|abc123",
  "aud": "YOUR_CLIENT_ID",
  "exp": 1787500800,
  "iat": 1787497200,
  "nonce": "n-0S6_WzA2Mj",
  "auth_time": 1787497200,
  "email": "user@example.com",
  "name": "Test User"
}
```

| Property | Detail |
|---|---|
| Format | **Always a JWT** — required by OIDC |
| `aud` | Your **client ID** |
| Purpose | Establish a **local session** in your app |
| Lifetime | Short — it is consumed once |
| After use | Create your session, then **stop using it** |

### The rule and the reason

> **Never send an ID token to an API as a credential.**

Three reasons, and it is worth having all three ready:

1. **The `aud` is wrong.** It is your client ID, not the API's identifier. A correctly-configured API rejects it.
2. **It carries no authorization information.** No `scope`, no permissions. It says who, never what.
3. **A poorly-configured API might accept it** — and that is worse, because it means the API is not checking `aud`, and now an identity assertion is functioning as an access credential (Part 043).

**Why it happens anyway:** the ID token is the first token developers meet, it decodes into readable, obviously-useful claims like `email` and `name`, and it is right there in the SDK response. The access token often looks less interesting — sometimes opaque, sometimes with no profile claims at all. The intuitive choice is the wrong one.

### 🔍 Plain-English deep-dive: the ID token is consumed once, not carried around

The most common structural misunderstanding is treating the ID token as a session credential that the application keeps and re-presents. It is not. It is a **one-time assertion**.

The intended lifecycle is short:

```mermaid
sequenceDiagram
    autonumber
    participant AS as "Authorization server"
    participant App as "Your application"
    participant Sess as "Your session store"
    AS-->>App: "id_token"
    App->>App: "Validate: signature, iss, aud=client_id, exp, nonce"
    App->>App: "Read sub, and any profile claims needed"
    App->>Sess: "Create MY session for this user"
    App->>App: "DISCARD the id_token - its job is done"
    Note over App,Sess: From here the app uses ITS OWN session,<br/>and an access token for APIs
```

**Why the discard step matters.** An application that keeps the ID token and re-validates it on every request has effectively adopted the identity provider's expiry as its own session expiry — usually a few minutes — and will either log users out constantly or start ignoring `exp` to avoid that. Both outcomes are bad, and both are avoidable by creating a normal application session.

**The corollary that trips people up:** because the ID token is discarded, the application no longer has a live source of profile data. If the user's name changes at the identity provider, the app's session will not know. The correct answers are to refresh the profile from **UserInfo** (Part 073) when it matters, or to accept that the profile is a snapshot from login — which is usually fine, and is worth saying explicitly rather than leaving as an accidental behavior.

**And the `nonce` check in that diagram is not optional.** It binds the ID token to the specific authorization request the application started, which is what prevents an attacker injecting a token obtained elsewhere (Part 065). It is easy to skip because everything works without it.

**Analogy:** showing a passport at hotel check-in. The clerk verifies it, records who you are, and hands it back — from then on you use a room key, not the passport. **Where it stops:** you keep the passport. The application genuinely discards the ID token, because unlike a passport it has an expiry measured in minutes and no mechanism to renew it without another login.

---

## 3. The Access Token

**Defined by OAuth 2.0.** A credential for calling an API.

```
GET /api/orders
Authorization: Bearer eyJhbGciOiJSUzI1NiIs...
```

| Property | Detail |
|---|---|
| Format | **JWT or opaque** — the spec does not require either |
| `aud` | The **API identifier** you requested |
| Purpose | Authorize an API call |
| Lifetime | Short — minutes to an hour |
| Validated by | The **API**, never the client |

### 🔍 Plain-English deep-dive: an access token is opaque *to the client*, by design

This is the single most useful mental model in this Part, and it resolves a whole ticket category.

**The client should treat an access token as an opaque string** — even when it happens to be a readable JWT. It obtains it, stores it, attaches it to requests, and never inspects it.

Why, when the claims are right there and easy to read?

| Reason | What goes wrong if you ignore it |
|---|---|
| **The format is not a contract** | An issuer may switch JWT ↔ opaque, or change claims. Client code that parses it breaks with no warning and no deployment on the client's side |
| **The token is not for the client** | `aud` names the API. Reading someone else's mail is a design smell even when it is legible |
| **It creates false confidence** | A client that reads `scope` to decide what to show is doing authorization in the browser, which is decoration, not enforcement |
| **It invites the worst bug** | Client-side decoding normalises reading unverified claims (Part 043 §7) |

**What the client should do instead:** get user information from the **ID token** or the **UserInfo endpoint** (Part 073), and get permissions from **the API's own response** — a 403 is the API telling you what is not allowed, authoritatively.

```mermaid
flowchart LR
    C["Client application"] --> Q{"What do I need?"}
    Q -->|"Who is the user?"| IDT["ID token claims, or UserInfo. Part 073."]
    Q -->|"What may they do?"| API["Ask the API. A 403 is the authoritative answer."]
    Q -->|"Call an API"| AT["Attach the access token. Do NOT read it."]
    AT --> Op["Treat it as an opaque string, even if it decodes"]
    Op --> Why["The format is not a contract. The audience is not you."]
    IDT --> Safe["This is what these are FOR"]
    API --> Safe
```

**The legitimate exception, stated precisely:** a client may read `exp` to decide when to refresh proactively. That is operational, not a security decision, and it degrades safely — if the read fails, the client just refreshes reactively on a 401 instead. Any other read is a smell worth questioning.

**Analogy:** a sealed envelope addressed to someone else that you have been asked to deliver. That it is thin enough to read through a window does not make reading it your business, and the sender may switch to thicker paper tomorrow without telling you. **Where it stops:** a real envelope's contents genuinely do not concern the courier. Here the client *does* care about one thing — when to get a new one — which is exactly why `exp` is the one sanctioned read.

---

## 4. The Refresh Token

**A long-lived credential used only against the token endpoint**, to obtain new access tokens without re-prompting the user.

```mermaid
sequenceDiagram
    autonumber
    participant U as "User"
    participant App as "Application"
    participant AS as "Token endpoint"
    participant API as "API"
    U->>App: "Logs in once"
    App->>AS: "Exchange code (Part 058)"
    AS-->>App: "access_token (15 min) + refresh_token (30 days) + id_token"
    App->>API: "Bearer access_token"
    API-->>App: "200"
    Note over App,API: 15 minutes later
    App->>API: "Bearer access_token"
    API-->>App: "401 - expired"
    App->>AS: "grant_type=refresh_token - NEVER to the API"
    AS-->>App: "New access_token (+ a NEW refresh_token if rotation is on)"
    App->>API: "Bearer new access_token"
    API-->>App: "200 - user never noticed"
```

| Property | Detail |
|---|---|
| Format | Usually **opaque** |
| Sent to | **Only** the token endpoint |
| Lifetime | Days to months, sometimes sliding |
| Sensitivity | 🔴 **Highest** — it mints new access tokens |
| Storage | Server-side, or a browser **only** with rotation (Part 061) |

### Why it is the most sensitive token you handle

A stolen access token is bad for its remaining minutes. A stolen refresh token is bad **for its entire lifetime**, and it silently produces fresh access tokens the whole time — a persistent, quiet foothold that generates no failed logins and no alerts.

**Rotation with reuse detection** (Part 061) is the standard mitigation: each use returns a new refresh token and invalidates the old one, so a second use of a retired token signals theft and revokes the whole family.

### 🔍 Plain-English deep-dive: "can we just use a long-lived access token instead?"

This question arrives constantly, usually phrased as avoiding complexity. It is worth answering well, because the reasoning generalises to a lot of identity design.

The proposal is: skip refresh tokens, issue an access token that lasts thirty days, and remove all the refresh plumbing. It is genuinely simpler. It is also a poor trade, for three reasons that are easy to state.

| | Long-lived access token | Short access + refresh |
|---|---|---|
| **Theft window** | 30 days of API access | Minutes |
| **Where it travels** | To **every API**, constantly | Access token travels; refresh token only touches the token endpoint |
| **Revocation** | ❌ None — valid until `exp` | Revoke the refresh token; access dies within minutes |
| **Theft detection** | ❌ None | Reuse detection catches a stolen refresh token |
| **Complexity** | ✅ Lower | Higher, but the SDK does it |

**The second row is the argument most people have not considered.** A refresh token is sensitive but it is also *rarely transmitted* — it goes to exactly one endpoint, occasionally. A long-lived access token is sent to every API on every request, through every proxy and gateway in the path, and logged wherever anything logs headers. The design deliberately concentrates the long-lived secret in the place with the least exposure.

**And the complexity argument is usually false in practice**, because every mainstream SDK handles refresh transparently. The developer asking is often working around a specific problem — refresh failing in a browser due to third-party cookie restrictions (Part 017), or a mobile app losing tokens on restart. **Asking what actually went wrong is more useful than debating the design**, because the real question is almost always "our refresh isn't working" wearing a disguise.

**Analogy:** carrying your house deeds everywhere so you never have to visit the safe. Convenient until the day you are pickpocketed, and the loss is not a day's cash. **Where it stops:** deeds are replaceable through a slow legal process. A stolen long-lived token is not detectable at all — there is nothing to notice, because every request it makes looks legitimate.

---

## 5. Opaque Tokens and Introspection

An opaque token is a **reference**, not a container: a random string that means something only to the issuer.

```mermaid
flowchart LR
    subgraph JWTv["JWT access token"]
        J1["Self-contained"] --> J2["Verify locally with JWKS"] --> J3["No network call"]
        J3 --> J4["✅ Fast, scalable"]
        J3 --> J5["❌ Cannot be revoked before exp"]
    end
    subgraph Opq["Opaque access token"]
        O1["A reference only"] --> O2["POST /introspect on every request"] --> O3["Network call each time"]
        O3 --> O4["✅ Instantly revocable"]
        O3 --> O5["❌ Latency, availability coupling, rate limits"]
    end
```

**Introspection** (RFC 7662) is how a resource server resolves one:

```http
POST /oauth/introspect
Authorization: Basic <resource server credentials>

token=a7f3c9d2e1b8...
```
```json
{ "active": true, "sub": "auth0|abc123", "scope": "read:orders", "exp": 1787500800, "aud": "https://api.example.com" }
```

**`"active": false` is the only field you must handle correctly** — it means expired, revoked, or never valid, and the response deliberately tells you nothing more.

### The trade, stated plainly

| | JWT | Opaque + introspection |
|---|---|---|
| Validation | Local | Network call |
| Revocation | ⏳ Waits for `exp` | ⚡ Immediate |
| Latency | ~0 | Per request, unless cached |
| Availability | Independent of the issuer | **Coupled** — issuer down means API down |
| Claims visible to the bearer | ✅ Yes | ❌ No |

**Caching introspection results** for a short window recovers most of the performance while keeping revocation reasonably fast — and it is the usual real-world answer. The cache TTL *is* the revocation delay, so it is a dial rather than a fix.

---

## 6. Choosing and Diagnosing

```mermaid
flowchart TD
    Q["Which token, and where?"] --> A{"What is the purpose?"}
    A -->|"Establish who the user is, in MY app"| ID["ID token - validate in the app, aud = client_id"]
    A -->|"Call an API"| AT["Access token - aud = API identifier"]
    A -->|"Get new tokens without a login"| RT["Refresh token - token endpoint ONLY"]
    A -->|"Display the user's name or picture"| UI["ID token claims, or UserInfo. Part 073."]
    A -->|"Decide what the UI shows"| Warn["Use claims for CONVENIENCE only. The API enforces. Part 052."]
    AT --> F{"JWT or opaque?"}
    F -->|"JWT"| L["Validate locally with JWKS. Part 043."]
    F -->|"Opaque"| I["Introspect. Consider a short cache."]
```

### The high-volume confusion table

| What they sent | Where | Symptom | Cause |
|---|---|---|---|
| ID token | To an API | 401, `aud` mismatch | It is for the app, not the API |
| Access token | Decoded in the client | Breaks on format change | Not a contract |
| Refresh token | To an API | 401, and **exposure** | Token-endpoint only |
| Access token | Where an ID token was needed | No `email`/`name` claims | Wrong token for profile data |
| Opaque token | Decoded locally | "Cannot decode" | Nothing to decode; introspect |
| Any token | In a URL query string | Logged in proxies and history | Use the `Authorization` header |

**That last row deserves emphasis.** Tokens in URLs end up in server logs, proxy logs, browser history, and `Referer` headers. It is one of the few genuinely common ways a token leaks without any attack at all.

---

## 7. Failure Modes

| Failure mode | What it looks like | Consequence | Correction |
|---|---|---|---|
| **ID token sent to an API** | 401 `aud` mismatch | Nothing works | Send the access token |
| **API accepts an ID token** | It "works" | 🔴 The API is not checking `aud` | Fix the API (Part 043) |
| **Client parses the access token** | Works until the format changes | Silent breakage, no deployment | Treat it as opaque |
| **Refresh token in a browser without rotation** | Long-lived credential exposed | 🔴 **Persistent account access** | Rotation + reuse detection (Part 061) |
| **Refresh token sent to an API** | 401 plus exposure | 🔴 Credential leaked into API logs | Token endpoint only |
| **Token in a URL query string** | Convenient | 🔴 **Logged everywhere** | `Authorization` header |
| **Expecting profile claims in an access token** | `email` missing | Broken UI | ID token or UserInfo |
| **Trying to decode an opaque token** | "Corrupted" | Chasing a non-bug | Introspect instead |
| **Introspecting on every request, uncached** | Latency, rate limits | Slow API, throttling | Short-TTL cache |
| **Ignoring `"active": false`** | Only checking `exp` | 🔴 Revoked tokens accepted | `active` is authoritative |
| **Very long access-token lifetime** | Chosen to "avoid refresh complexity" | Wide theft window | Short access + refresh |
| **Refresh token stored in `localStorage`** | XSS-readable | 🔴 Theft via any script injection | Part 016's storage rules |

---

## 8. Troubleshooting Decision Tree: Wrong Token

```mermaid
flowchart TD
    S["An API call fails with a token attached"] --> D["Decode it locally - Part 040"]
    D --> T{"Does it decode at all?"}
    T -->|"No, and it is random"| Op["OPAQUE - it cannot be decoded. Introspect it instead."]
    T -->|"Yes - 3 segments"| A{"What is aud?"}
    A -->|"A client ID"| IDt["This is an ID TOKEN sent to an API. Send the access token."]
    A -->|"Ends in /userinfo"| NoAud["Access token with NO audience requested. Part 041."]
    A -->|"A different API identifier"| Other["Token for another API - correct rejection. Part 043."]
    A -->|"This API's identifier"| Cont{"Any profile claims expected but missing?"}
    Cont -->|"Yes"| Prof["Wrong expectation - use the ID token or UserInfo. Part 073."]
    Cont -->|"No"| Next["Audience is right. Continue the Part 043 checklist: exp, iss, kid, scope."]
    IDt --> Sec{"Did the API ACCEPT it before?"}
    Sec -->|"Yes"| Flag["SECURITY FINDING - the API is not validating aud. Raise it."]
```

### Worked example

*"Our SPA calls our API and gets 401. The token is definitely valid — we can see the user's email inside it."*

1. **"We can see the user's email inside it" is the diagnosis.** Access tokens do not normally carry `email`; ID tokens do. They are sending the ID token.
2. **Confirm with a decode.** `aud` equals their client ID. Confirmed.
3. **Find why.** Their SDK returned an object with several fields and they picked the one whose contents looked useful. This is the normal reason, not carelessness.
4. **Fix:** use the access token — in Auth0 SDKs, `getAccessTokenSilently()` rather than the ID token — and pass the `audience` parameter so the access token is actually addressed to their API (Part 041).
5. **Explain the model in two lines**, because this recurs otherwise: *"The ID token tells your app who the user is — use it once to establish your session. The access token is the credential for calling your API. Your app shouldn't read the access token at all; if you need the user's email for the UI, take it from the ID token or UserInfo."*
6. **Check the security direction too.** Ask whether the API ever accepted the ID token. If it did, the API is not validating `aud`, and that is a finding worth raising even though they did not ask.

---

## 9. Lab: Four Tokens, Side by Side

**Purpose.** Obtain every token type from one tenant, compare them directly, and reproduce each misuse so the symptoms are familiar.

**Prerequisites.** Parts 040–043 artifacts. A free Auth0 tenant, a test API, and a minimal SPA plus a Node API (Part 028).

**Steps.**

1. Create `okta-prep/labs/044-token-families/`.
2. **Get all four.** Perform a login requesting `openid profile email offline_access` plus your API's `audience`. Capture the ID token, access token, and refresh token. Then configure the API for opaque tokens if your tenant supports it, or simulate one.
3. **Comparison table.** For each token: format, segment count, `aud`, `iss`, `exp`, computed lifetime, which claims are present, and which party validates it. **This single table is the whole Part.**
4. **Lifetime contrast.** Record all three lifetimes. Note the ratio between the access and refresh tokens and reason about why it is that large.
5. **Misuse 1 — ID token to the API.** Send it. **Record the exact error.**
6. **Misuse 1b — the dangerous variant.** Temporarily remove the `aud` check from your API and send the ID token again. **Confirm it is accepted.** Write one line on why that is a security defect, then restore the check immediately.
7. **Misuse 2 — refresh token to the API.** Send it as a bearer token. Record the error, and note that it now appears in your API's logs — **this is the exposure, demonstrated.**
8. **Misuse 3 — access token for profile data.** Attempt to read `email` from it. Record what is actually present. Then obtain the same data correctly from the ID token and from UserInfo.
9. **Misuse 4 — token in a URL.** Send a token as a query parameter. **Then find it in your server's access log.** Screenshot the log line. This is the most persuasive artifact in the lab.
10. **Refresh flow.** Use the refresh token to obtain a new access token. Compare old and new: `iat`, `exp`, `jti`. Note whether a **new refresh token** was returned — that tells you rotation is on (Part 061).
11. **Introspection.** If available, introspect an access token. Record the response. Then revoke it and introspect again — **confirm `"active": false`** and note how quickly it takes effect.
12. **Revocation contrast.** Revoke a session and immediately call the API with an already-issued JWT access token. **Confirm it still works until `exp`.** This is Part 041's snapshot lesson made concrete, and it is the demonstration that makes the JWT trade real.
13. **Write the explainer.** `which-token.md` — a one-page customer-facing note with the four tokens, their audiences, who validates each, and the three "never" rules. **You will reuse this on real tickets.**
14. **Failure catalog + manifest.** Complete `MANIFEST.md`.

**Expected evidence.** All four token types captured, a full comparison table, computed lifetimes, four reproduced misuses with exact errors, a token found in a server log, a refresh exchange with rotation observed, an introspection before/after revocation, a demonstrated revocation gap for a JWT, and a reusable customer explainer.

**Validation rubric.**

| Check | Pass condition |
|---|---|
| Four token types obtained | All present, decoded locally |
| Comparison table | `aud` and validator differ correctly per token |
| ID-token misuse | Error recorded; unsafe acceptance demonstrated and explained |
| Refresh-token misuse | Error recorded; log exposure noted |
| URL leakage | Token located in a server log |
| Refresh exchange | New token compared; rotation state determined |
| Introspection | `active` true then false, with timing |
| Revocation gap | JWT still accepted after revocation, until `exp` |
| Customer explainer | One page, four tokens, three rules |

**Cleanup and privacy.** Lab tenant, synthetic users, localhost only. **Revoke every refresh token at the end** — they are the longest-lived credential in the lab. Delete all captured tokens. Store nothing in `localStorage` beyond the deliberate demonstration, and clear it. Never repeat the URL-leakage step against any system you do not own.

---

## 10. JD Mapping

| Supplied JD signal | How this Part supports it |
|---|---|
| **Basic security concepts** | Refresh-token sensitivity, URL leakage, `aud` enforcement |
| Strong analytical and problem-solving skills | §8's tree identifies the wrong token in one decode |
| Experience troubleshooting web applications | The SPA-to-API 401 investigation |
| **Communicate technical concepts clearly** | The two-line model explanation and the one-page explainer |
| Promote best practices | Opaque-to-the-client discipline; header not query string |
| Exceed expectations on response quality | Raising the unasked `aud` finding |
| Knowledge of HTTP | `Authorization` header versus query parameters and logging |

---

## 11. Candidate Honesty Note

- **Claim safety:** *Learned architecture and lab experience.* You have obtained and misused all four token types in a lab; you have not run a production token strategy.
- **The strongest thing you can say:** *"Four tokens, four jobs. The ID token tells your app who the user is — `aud` is your client ID and your app validates it. The access token is the credential for an API — `aud` is the API identifier and the API validates it. The refresh token only ever goes back to the token endpoint. And an opaque token is a reference you introspect rather than decode."*
- **A second point that is genuinely the most useful idea here:** *"A client should treat an access token as opaque even when it's a readable JWT. The format isn't a contract — an issuer can switch to opaque or change claims and client code that parses it breaks with no deployment on their side. The one sanctioned read is `exp`, for proactive refresh, because that degrades safely."*
- **A third, on diagnosis:** *"'We can see the user's email in the token' usually means they're sending the ID token to their API. Access tokens don't normally carry `email`. That one sentence from a customer is often the entire diagnosis before I've seen anything."*
- **A fourth, and it shows security instinct:** *"If the API ever accepted that ID token, that's a bigger finding than the ticket — it means the API isn't checking `aud`. Worth raising even though they didn't ask."*
- **A fifth:** *"Tokens in query strings leak into server logs, proxy logs, browser history and `Referer` headers. It's one of the few ways a token escapes with no attacker involved at all."*
- **Do not overstate:** you have not designed a token lifetime or revocation strategy for a production system. Say the model is clear and the operational trade-offs at scale are what the role would add.

---

## 12. Official Source Anchors

Accessed **26 August 2026**.

| Source | What it defines |
|---|---|
| IETF RFC 6749 (OAuth 2.0) | Access and refresh tokens and their roles |
| IETF RFC 6750 | Bearer usage: the `Authorization` header, and why not the URI |
| IETF RFC 7662 | Token introspection and the `active` field |
| IETF RFC 7009 | Token revocation |
| IETF RFC 9068 | JWT access-token profile |
| OpenID Connect Core §2 and §3.1.3.7 | ID token definition and validation |
| OAuth 2.0 Security BCP | Refresh-token rotation and storage guidance |
| Auth0 documentation — tokens | Vendor behavior: lifetimes, rotation, opaque versus JWT |
| Okta developer documentation — token types | Okta's token model |

**Revalidate after 26 August 2026:** RFCs are stable. Recheck vendor documentation for default lifetimes and rotation defaults, and OAuth 2.1 (Part 066) for consolidated guidance.

---

## ⭐ Likely Interview Questions for This Section

### Q1. "What's the difference between an ID token and an access token?"
> *Model answer:* "Different questions, different audiences, different validators. The ID token comes from OIDC and answers 'who is this user?' — its `aud` is your client ID, your application validates it, and its job is to let you establish a local session. The access token comes from OAuth and answers 'what may the bearer do?' — its `aud` is the API identifier, the API validates it, and it goes in the `Authorization` header. The practical rule is that you never send an ID token to an API: the audience is wrong, it carries no scopes, and if the API accepts it anyway that's worse, because it means the API isn't checking `aud` and an identity assertion is now working as an access credential."

### Q2. "Why shouldn't a client read the access token's claims?"
> *Model answer:* "Because the format isn't a contract. An issuer can switch between JWT and opaque, or change the claim set, and client code that parses it breaks with no warning and no deployment on the client's side. The token also isn't addressed to the client — `aud` names the API — so reading it is a design smell even when it's legible. And it creates false confidence: a client deciding what to show based on `scope` is doing authorization in the browser, which is decoration, not enforcement. If you need user information, use the ID token or UserInfo. If you need to know what's permitted, ask the API — a 403 is the authoritative answer. The one legitimate read is `exp`, to refresh proactively, and that's operational rather than a security decision, so it degrades safely."

### Q3. "Why is a refresh token the most sensitive token?"
> *Model answer:* "Because of the time window. A stolen access token is a problem for its remaining few minutes. A stolen refresh token is a problem for its entire lifetime — days or months — and it silently mints fresh access tokens the whole time. That's a persistent, quiet foothold: no failed logins, no anomalies, nothing that trips an alert. So it only ever goes back to the token endpoint, never to an API, and it's stored server-side where possible. If it has to live in a browser, rotation with reuse detection is the mitigation: every use returns a new refresh token and retires the old one, so a second use of a retired token is strong evidence of theft and revokes the whole family."

### Q4. "JWT or opaque access tokens — how would you advise a customer?"
> *Model answer:* "It's a revocation-versus-performance trade. A JWT is self-contained, so the API verifies it locally against JWKS with no network call — fast, scalable, and independent of the issuer's availability. The cost is that it can't be recalled: it's valid until `exp`, so revocation waits. An opaque token is a reference, so the API introspects it on every request — instantly revocable, at the cost of a network call per request, coupling the API's availability to the issuer's, and rate-limit exposure. The usual real-world answer is a JWT with a short lifetime, which keeps the revocation window small without the per-request call. If they genuinely need immediate revocation, opaque with a short-TTL introspection cache is the middle ground — though it's worth being explicit that the cache TTL *is* the revocation delay, so it's a dial rather than a fix."

### Q5. "A customer says 'the token is valid, we can see the user's email in it,' but their API returns 401."
> *Model answer:* "That sentence is usually the whole diagnosis. Access tokens don't normally carry `email` — ID tokens do. So they're sending the ID token to their API, and the API is correctly rejecting it because `aud` is their client ID rather than the API identifier. I'd confirm with one decode, then explain the model in two lines so it doesn't recur: the ID token tells your app who the user is and you use it once to establish a session; the access token is the credential for calling your API, and your app shouldn't read it at all. The fix is to use the access token and pass the `audience` parameter so it's actually addressed to their API. And I'd ask whether the API ever accepted that ID token — if it did, that's a bigger finding than the ticket."

### Q6. "Why shouldn't tokens go in URLs?"
> *Model answer:* "Because URLs get written down everywhere by default. Web server access logs, proxy and load balancer logs, browser history, bookmarks, and the `Referer` header sent to any third-party resource the page loads — analytics, fonts, images. None of that requires an attacker; it's just normal infrastructure doing normal logging. And log retention often outlives the token by months, so a credential that should have lived fifteen minutes is sitting in a log aggregator indefinitely. RFC 6750 defines the `Authorization` header as the way to do this. It's a good one to demonstrate rather than argue: sending a token in a query string and then finding it in your own access log is more convincing than any explanation."

### Q7. "A customer revoked a session but the user can still call the API. Bug?"
> *Model answer:* "Almost certainly not — it's the JWT trade working as designed. Revocation invalidates the session and the refresh token, but an access token already issued is self-contained: the API verifies it locally without asking the issuer anything, so it stays valid until `exp`. Nothing checks. I'd confirm by comparing the token's `iat` against the revocation time, and by noting that the user won't be able to obtain a *new* token — so the problem self-resolves in minutes. If minutes are too long for their threat model, the options are a shorter access-token lifetime, opaque tokens with introspection, or a revocation check on specifically sensitive operations rather than every request. That's a design conversation, and framing it as a deliberate trade rather than a defect keeps it productive."

### Q8. "What does `active: false` from introspection mean?"
> *Model answer:* "That the token is not usable — and deliberately nothing more. It could be expired, revoked, malformed, or never issued at all, and the spec has the endpoint stay vague on purpose so it can't be used as an oracle to probe which tokens exist. The important implementation point is that `active` is authoritative and must be checked on its own: code that only looks at `exp` in the introspection response will happily accept a revoked-but-unexpired token, which defeats the entire reason for choosing opaque tokens. It's the same class of mistake as skipping `aud` — the check that gives the design its value is the one that gets omitted, because everything appears to work without it."

---

## 🧠 30-Second Memory Hooks

- **ID token** → who the user is → `aud` = **client ID** → **your app** validates → OIDC.
- **Access token** → what may be done → `aud` = **API identifier** → **the API** validates → OAuth.
- **Refresh token** → get more tokens → **token endpoint ONLY** → most sensitive.
- **Opaque token** → a reference → **introspect**, do not decode.
- **Never** send an ID token to an API. **Never** send a refresh token to an API.
- **Treat an access token as opaque in the client.** The only sanctioned read is `exp`.
- **"We can see the email in it"** = they are sending the **ID token**.
- **If the API accepted the ID token → it is not checking `aud`.** Security finding.
- **Tokens in URLs leak** into logs, history, and `Referer`. Use the header.
- **JWT = fast, no revocation. Opaque = revocable, per-request call.**
- **`active: false` is authoritative.** Checking only `exp` accepts revoked tokens.
- **Revoked session + working API call = the JWT trade**, not a bug.

---

## ✅ Completion Checklist

- [ ] **Knowledge:** I can state all four tokens, their audiences, and their validators without hesitation.
- [ ] **Lab artifact:** `044-token-families/` contains a four-token comparison, four reproduced misuses, a token found in a server log, an introspection before/after revocation, a demonstrated revocation gap, and a one-page customer explainer.
- [ ] **Spoken:** I can explain the four tokens in 60 seconds and diagnose "we can see the email in it" in 15.
- [ ] **Judgement:** I raise the unasked `aud` finding when an ID token was accepted.
- [ ] **Honesty check:** I say "lab experience," not production token strategy.
- [ ] **Source check:** I have read RFC 6750 §2 and RFC 7662's `active` definition myself.

---

*Next suggested section:* **[Part 045 - Token Lifetime, Revocation, Introspection, and Caching](Part-045-token-lifetime-revocation-introspection-and-caching.md)** — how long tokens should live, what revocation actually achieves, and the caching decisions that make or break an API.
