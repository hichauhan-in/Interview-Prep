# Part 034 - Encoding versus Hashing versus Encryption versus Signing

> Section goal: Fix the four operations that everything else in identity is built from. This is the most common conceptual confusion in the whole domain, and getting it wrong produces both wrong answers to customers and genuine security mistakes. After this Part you should be able to look at any string and say which operation produced it and what that guarantees.

Covers index item **034**. Maps to JD signals: *knowledge of encryption*, *basic security concepts*, *understanding of authentication and authorization concepts*, *strong analytical and problem-solving skills*, and *promote best practices*.

---

## 1. Start From Zero: Four Operations, Four Different Jobs

| Operation | Purpose | Reversible? | Needs a key? | Produces |
|---|---|---|---|---|
| **Encoding** | Change *representation* so data survives transport | ✅ Yes, by anyone | ❌ No | A different-looking version of the same data |
| **Hashing** | Produce a fixed-size fingerprint | ❌ No, by design | ❌ No | A fixed-length digest |
| **Encryption** | Make data unreadable to anyone without the key | ✅ Yes, with the key | ✅ Yes | Ciphertext |
| **Signing** | Prove origin and detect tampering | n/a — you verify, not reverse | ✅ Yes | A signature alongside the data |

```mermaid
flowchart TD
    Q["I have some data. What do I want?"] --> W{"What is the goal?"}
    W -->|"Survive transport safely"| E["ENCODING - Base64, URL-encoding, hex"]
    W -->|"Compare without storing the original"| H["HASHING - SHA-256, bcrypt, Argon2"]
    W -->|"Keep it secret from everyone without the key"| C["ENCRYPTION - AES, RSA-OAEP"]
    W -->|"Prove who created it and detect changes"| S["SIGNING - RS256, ES256, HMAC"]
    E --> E1["NO security property at all"]
    H --> H1["One-way. Same input always gives the same digest."]
    C --> C1["Confidentiality"]
    S --> S1["Integrity + authenticity. NOT confidentiality."]
    E1 --> Warn["Confusing these is the #1 identity misconception"]
    H1 --> Warn
    C1 --> Warn
    S1 --> Warn
```

> **Analogy.** Four things you can do to a letter. **Encoding** is writing it in block capitals so the sorting machine can read it — anyone can still read it. **Hashing** is recording its exact weight — you can check a letter matches, but you cannot rebuild it from the weight. **Encryption** is sealing it in a locked box. **Signing** is a wax seal with your crest — anyone can read the letter, but they can tell it came from you and has not been opened.
>
> **Where it stops:** a wax seal can be forged with a stolen stamp. A cryptographic signature cannot, without the private key — which is why key protection matters more than any other operational concern in identity.

### 🔍 Plain-English deep-dive: the misconception that causes real harm

The single most common and most damaging confusion is **"encoded means protected."**

Base64 looks unreadable. It contains letters and digits in an unfamiliar pattern. It *feels* secure. It is not:

```
eyJzdWIiOiJhdXRoMHxhYmMxMjMiLCJlbWFpbCI6ImFydGlAZXhhbXBsZS5jb20ifQ
```

That decodes, with no key and no effort, to:

```json
{"sub":"auth0|abc123","email":"arti@example.com"}
```

**Encoding provides zero security.** It is a transport convenience, nothing more.

Where this causes real harm:

| Belief | Reality |
|---|---|
| "The token is encoded, so the claims are private" | Anyone holding the token reads every claim |
| "We Base64 the password before sending it" | It travels in plaintext for practical purposes |
| "HTTP Basic auth encodes the credentials" | Base64 of `user:password` — trivially reversed |
| "The config is Base64 so the secret is safe" | It is a published secret |

**The correct one-liner to give a customer:** *"Base64 is not encryption. It is a way of writing binary data using safe characters. Anyone can decode it instantly, with no key."*

**Analogy:** writing a message in mirror writing. It looks scrambled and any reader can hold it up to a mirror. **Where it stops:** mirror writing at least requires a mirror. Base64 decoding is a single command anyone already has.

---

## 2. Encoding

**Encoding** changes representation so data can travel through a channel that would otherwise corrupt it.

| Encoding | Purpose | Where you meet it |
|---|---|---|
| **Base64** | Binary → 64 safe ASCII characters | Basic auth, certificates, SAML |
| **Base64url** | Base64 with URL-safe characters | **JWTs**, PKCE challenge |
| **Percent-encoding** | Escape URL-structural characters | Query parameters (Part 013) |
| **Hex** | Binary → 16 characters | Key thumbprints, fingerprints |
| **PEM** | Base64 plus header/footer lines | Certificates, keys |
| **DER** | Binary certificate encoding | The raw form PEM wraps |

### Base64 versus Base64url

| | Base64 | Base64url |
|---|---|---|
| Character 62 | `+` | `-` |
| Character 63 | `/` | `_` |
| Padding | `=` | Usually omitted |
| Safe in a URL | ❌ `+` and `/` have meaning | ✅ |

**Why this matters practically:** JWTs use Base64url. If a developer decodes a JWT segment with a standard Base64 decoder, it may fail or produce garbage on segments containing `-` or `_`. The fix is either a Base64url decoder or a character substitution before decoding. This is a real, common cause of "the token won't decode".

### Recognising an encoding at a glance

| Looks like | Probably |
|---|---|
| Three dot-separated chunks | **JWT** — Base64url |
| `-----BEGIN CERTIFICATE-----` | PEM |
| Long, `+` and `/` and trailing `=` | Standard Base64 |
| Long, `-` and `_`, no padding | Base64url |
| `%3A`, `%2F` | Percent-encoded |
| 64 hex characters | SHA-256 digest |
| Starts `PHNhbWxwOl` | Base64 of XML — **a SAML message** (Part 082) |

That last row is a genuinely useful trick. Base64-encoded XML almost always begins with a recognisable prefix, so you can identify a SAML message in a HAR without decoding anything.

---

## 3. Hashing

**Hashing** produces a fixed-size fingerprint of any input. It is **one-way** — you cannot recover the input from the digest.

| Property | Meaning |
|---|---|
| **Deterministic** | Same input → same digest, always |
| **Fixed length** | SHA-256 → 256 bits, whatever the input size |
| **Avalanche** | One-bit change → completely different digest |
| **One-way** | Infeasible to reverse |
| **Collision-resistant** | Infeasible to find two inputs with the same digest |

```mermaid
flowchart LR
    I1["'password123'"] --> H["SHA-256"] --> D1["ef92b778...  (64 hex chars)"]
    I2["'password124'"] --> H2["SHA-256"] --> D2["a1b2c3d4...  COMPLETELY different"]
    I3["A 2 GB file"] --> H3["SHA-256"] --> D3["Still exactly 64 hex chars"]
    D1 --> P["Deterministic + avalanche + fixed length"]
    D2 --> P
    D3 --> P
    P --> Use["Uses: password verification, integrity checks, PKCE, key thumbprints"]
    P --> Not["NOT for: keeping data secret and recoverable"]
```

### Where hashing appears in identity

| Use | Why hashing |
|---|---|
| **Password storage** | Store the digest; compare digests. The original is never stored (Part 036) |
| **PKCE** | `code_challenge` is a hash of `code_verifier` (Part 059) |
| **Key thumbprints** | A short identifier for a certificate or key |
| **Integrity checks** | Detect whether content changed |
| **`at_hash`, `c_hash`** | ID token claims binding a token or code to the ID token |

### 🔍 Plain-English deep-dive: why "we hash it, so it's encrypted" is wrong and dangerous

Customers say this constantly. The distinction is not pedantic — the two solve opposite problems.

| | Hashing | Encryption |
|---|---|---|
| Can you get the original back? | **Never** | Yes, with the key |
| Use it for | Verifying a match | Storing or transmitting data you need back |
| Password storage | ✅ **Correct** | ❌ Wrong — a key compromise exposes every password |
| Storing a token you must send later | ❌ Impossible — you cannot recover it | ✅ Correct |

**The rule: if you need the original value back, hashing is not an option.**

Two real consequences:

1. **Passwords should be hashed, never encrypted.** Encrypted passwords are recoverable, so a key compromise reveals every password in plaintext. Hashed passwords are not recoverable even by the operator — which is the point.
2. **You cannot "hash" a token you need to present later.** If a customer says they hash refresh tokens before storing them, ask how they retrieve them. Either they are actually encrypting, or they store a hash *for lookup* alongside the encrypted value — which is a legitimate pattern worth confirming.

**Analogy:** shredding a document versus locking it in a safe. Shredding proves you had it (you can shred an identical copy and compare the pieces) but you can never read it again. **Where it stops:** shredded paper can theoretically be reassembled. A cryptographic hash cannot, which is stronger than the analogy suggests.

---

## 4. Encryption

**Encryption** makes data unreadable without the key, and reversible with it.

| Type | Key arrangement | Speed | Used for |
|---|---|---|---|
| **Symmetric** | One shared key for both operations | Fast | Bulk data, TLS session traffic |
| **Asymmetric** | Public key encrypts, private key decrypts | Slow | Key exchange, small payloads |

Part 035 covers both in detail. What matters here is the **distinction from signing**, because they use the same key pairs in mirrored ways.

| | Encryption (asymmetric) | Signing |
|---|---|---|
| Who uses the **public** key | The sender, to encrypt | The verifier, to check |
| Who uses the **private** key | The recipient, to decrypt | The signer, to sign |
| Provides | Confidentiality | Integrity + authenticity |
| Who can read the content | Only the key holder | **Anyone** |

**That last row is the one that matters most in identity.** A signed JWT is **readable by anyone**. Signing does not hide anything; it only proves the content came from the signer and has not been altered.

---

## 5. Signing

**Signing** proves two things: the data came from a specific party (**authenticity**), and it has not been altered (**integrity**).

```mermaid
sequenceDiagram
    participant IdP as Identity provider
    participant Net as Network / browser / anyone
    participant API as Your API
    IdP->>IdP: Build the claims: {sub, aud, iss, exp}
    IdP->>IdP: Hash the header and payload
    IdP->>IdP: Sign the hash with the PRIVATE key
    IdP->>Net: header.payload.signature - the payload is READABLE
    Note over Net: Anyone can read the claims. Nobody can change them undetected.
    Net->>API: Present the token
    API->>IdP: Fetch the PUBLIC key from JWKS (cached)
    API->>API: Re-hash header.payload, verify against the signature
    API->>API: Match = authentic and unmodified. Mismatch = reject.
```

### The two signing families

| Family | How | Key | Trade-off |
|---|---|---|---|
| **HMAC** (`HS256`) | Symmetric — one shared secret | Same secret signs and verifies | Anyone who can verify can also **forge**. Only for same-party use |
| **Asymmetric** (`RS256`, `ES256`) | Private key signs, public key verifies | Different keys | Verifiers cannot forge. **The correct choice for federation** |

### 🔍 Plain-English deep-dive: why `HS256` is wrong for federated tokens

This is a real security decision you will encounter, and being able to explain it is genuinely valuable.

With **HMAC**, the same secret both signs and verifies. So every party you allow to *verify* your tokens can also *create* them.

In a federated system, that is fatal:

- Your identity provider signs tokens.
- Your API verifies them — so your API needs the secret.
- Your API can now **mint tokens claiming to be from the identity provider**.
- Any service holding the shared secret can impersonate the issuer to any other service.
- Rotating the secret means coordinating every holder simultaneously.

With **RS256** or **ES256**, the private key stays with the issuer. Verifiers only ever hold the public key, which is published openly at the JWKS endpoint. A verifier cannot forge, a leaked public key is harmless, and rotation is transparent because verifiers refetch the key set.

**The rule:** `HS256` is acceptable only when the signer and verifier are the same party — for example, an application signing its own session cookie. **For anything crossing a trust boundary, use asymmetric signing.**

**The related vulnerability to watch for:** if a verifier does not pin the expected algorithm, an attacker may be able to swap the algorithm in the token header. `algorithms` must be an explicit allow-list (Parts 028, 043). This is why that check appears on every code-review list in this guide.

**Analogy:** a rubber stamp of your signature versus a handwritten one. Give someone the stamp so they can check documents, and they can now sign as you. **Where it stops:** handwriting can be forged with skill; a private key cannot be derived from a public key at all.

---

## 6. Identifying What You Are Looking At

A practical skill: given an unfamiliar string in a HAR or a config file, name the operation.

| Observation | Operation | Guarantee | Can you read it? |
|---|---|---|---|
| Three dot-separated Base64url chunks | **Signed** (JWT) | Integrity, authenticity | **Yes** — decode segments 1 and 2 |
| Five dot-separated chunks | **Encrypted** (JWE) | Confidentiality | No, not without the key |
| `-----BEGIN CERTIFICATE-----` | Encoded (PEM) | None by itself | Yes |
| `-----BEGIN PRIVATE KEY-----` | Encoded — **a secret** | None; must never be shared | Yes — **treat as an incident** |
| `Basic dXNlcjpwYXNz` | **Encoded** only | **None** | Yes — credentials in plaintext |
| 64 hex characters | **Hashed** | One-way | No, and never |
| `$2b$12$...` | **Hashed** (bcrypt) | One-way, work factor 12 | No |
| Opaque random string | Reference token | None inherent | No — only the issuer can resolve it |
| Base64 starting `PHNhbWxw` | **Encoded** XML | None | Yes — a SAML message |

### 🔍 Plain-English deep-dive: JWT versus JWE, and the four-dot check

Developers sometimes ask why they "cannot decode the token". The count of dots answers it instantly.

| Dots | Segments | Type | Readable |
|---|---|---|---|
| **2** | header.payload.signature | **JWS** — signed | ✅ Yes |
| **4** | header.key.iv.ciphertext.tag | **JWE** — encrypted | ❌ No |

A **JWS** is signed: readable by anyone, unforgeable. A **JWE** is encrypted: unreadable without the key.

**Why this matters in support:**

- If a customer says "we can't read the token", count the dots. Four means it is encrypted, and that is expected behavior, not a bug.
- If a customer says "our tokens are secure because they're JWTs", correct that gently — a standard JWT is signed, not encrypted, and every claim is readable by anyone holding it. If they need confidentiality, they need JWE or an opaque reference token.
- **Never put sensitive data in a signed JWT's claims.** It travels through browsers, logs, and support tickets, and it is readable at every hop.

**Analogy:** a sealed envelope versus a postcard with an official stamp. Both prove origin; only one hides the message. Most people assume every token is an envelope, and most tokens are postcards. **Where it stops:** a postcard is obviously readable. A JWT looks scrambled, which is exactly why the misconception persists.

---

## 7. Failure Modes

| Failure mode | What it looks like | Consequence | Correction |
|---|---|---|---|
| **"Encoded means protected"** | Base64 treated as a security control | Credentials and claims exposed | Encoding provides zero security |
| **Sensitive data in JWT claims** | Personal or secret data in the payload | Readable in logs, HARs, browser history | JWE, or an opaque token, or omit it |
| **"We hash it so it's encrypted"** | Conflating the two | Wrong control chosen | Hashing is one-way; encryption is reversible |
| **Encrypting passwords** | Reversible password storage | A key compromise exposes every password | Hash with bcrypt/Argon2 (Part 036) |
| **`HS256` across a trust boundary** | Shared secret with verifiers | **Every verifier can forge tokens** | Asymmetric signing |
| **`algorithms` not pinned** | Verifier accepts whatever the header says | Algorithm-confusion vulnerability | Explicit allow-list |
| **Base64 decoder on Base64url** | "The token won't decode" | Wasted time | Use a Base64url decoder |
| **Pasting a token into an online decoder** | Live credential sent to a third party | Exfiltration | Decode locally (Part 040) |
| **Sharing a private key** | Key in a ticket or a repository | **Catastrophic** | Treat as an incident; rotate immediately |
| **"It's a JWT so it's secure"** | Assuming confidentiality | Claims exposed | Signed ≠ encrypted |
| **Trying to decode a JWE** | "Corrupted token" | Chasing a non-bug | Count the dots — four means encrypted |

---

## 8. Troubleshooting Decision Tree: Identifying and Reasoning About a String

```mermaid
flowchart TD
    S["An unfamiliar string in evidence"] --> D{"How many dots?"}
    D -->|"2"| JWS["JWS - signed JWT. Decode segments 1 and 2 LOCALLY."]
    D -->|"4"| JWE["JWE - encrypted. Cannot be read without the key. Expected, not a bug."]
    D -->|"None"| P{"What does it start with?"}
    P -->|"-----BEGIN CERTIFICATE-----"| Cert["PEM certificate - public, safe to share"]
    P -->|"-----BEGIN PRIVATE KEY-----"| Priv["PRIVATE KEY - treat as an incident. Do not copy or forward."]
    P -->|"Basic ..."| Basic["Base64 credentials - plaintext for practical purposes"]
    P -->|"$2b$ or $argon2"| Bcrypt["Password hash - one-way, never recoverable"]
    P -->|"PHNhbWxw..."| Saml["Base64 XML - a SAML message. Part 082."]
    P -->|"64 hex characters"| Hash["SHA-256 digest - one-way"]
    P -->|"Random, no structure"| Opaque["Opaque reference token - only the issuer can resolve it"]
    JWS --> Ask{"What is the customer assuming about it?"}
    JWE --> Ask
    Ask -->|"'It is secure because it is encoded'"| Corr1["Correct: encoding gives no security"]
    Ask -->|"'The claims are private'"| Corr2["Correct: a signed JWT is readable by anyone"]
    Ask -->|"'We hashed it so we can decrypt it later'"| Corr3["Correct: hashing is one-way"]
```

### Worked example

*"Our security team flagged that user emails are visible in our access tokens. Is that a bug in your platform?"*

1. **Not a bug — a misunderstanding, and a legitimate concern underneath it.** Handle both.
2. **Establish the facts.** Ask for the decoded token, redacted. It is a standard three-segment JWS with `email` in the payload.
3. **Explain the mechanism:** a JWT is **signed**, not encrypted. The header and payload are Base64url-encoded, which is a transport representation, not a security control. Anyone holding the token can read every claim. That is by design — the signature guarantees the claims cannot be *altered*, not that they cannot be *read*.
4. **Validate the concern.** Their security team is right to flag it. A token travels through browsers, proxy logs, browser history, application logs, and support tickets, and the claims are readable at every hop.
5. **Give the options, with trade-offs:**

| Option | Effect | Cost |
|---|---|---|
| Remove the claim from the access token | The API looks the email up itself | An extra lookup |
| Use an opaque access token | Nothing readable at all | Requires introspection on every call |
| Use JWE | Encrypted claims | Key management; verifiers need the decryption key |
| Keep `email` only in the **ID token** | The ID token is for the client, not sent to APIs | Requires them to stop sending ID tokens to APIs |

6. **Recommend:** the last row is usually right. Identity claims belong in the ID token, which stays with the client; access tokens should carry authorization data, not profile data (Part 044).
7. **The next trap to name:** if they are currently sending the ID token to their API as a credential, that is a separate and more serious problem.

That answer corrects a misconception, validates a real concern, offers a genuine choice, and surfaces an adjacent issue — the Part 004 structure applied to a conceptual question rather than a bug.

---

## 9. Lab: Perform All Four Operations

**Purpose.** Do each operation by hand so the distinctions become observations rather than definitions.

**Prerequisites.** Part 007's lab — Node, Python, OpenSSL. **All local; use only fake values.**

**Steps.**

1. Create `okta-prep/labs/034-four-operations/`.
2. **Encoding.** Base64-encode `{"sub":"user1","email":"a@example.com"}`. Decode it back. Then produce the **Base64url** variant and record which characters differ. **Record how long decoding took** — the point is that it is instant and keyless.
3. **Basic auth demonstration.** Encode `user:FAKEPASSWORD` as Base64, exactly as HTTP Basic auth does. Decode it. Save both. **This is the artifact you show someone who thinks encoding protects credentials.**
4. **Base64url failure.** Take a real JWT segment from your lab tenant containing `-` or `_` and decode it with a **standard** Base64 decoder. Record the error or garbage. Then decode correctly. This is the "token won't decode" ticket.
5. **Hashing.** SHA-256 the string `password123`, then `password124`. Record both digests and note the avalanche effect. Hash a large file and record that the digest length is unchanged.
6. **Hashing is one-way.** Attempt to reverse a digest programmatically for thirty seconds. Record that you cannot. Write one line on why a rainbow table works only for unsalted common passwords (previewing Part 036).
7. **Symmetric encryption.** Using OpenSSL or Node's crypto, encrypt a short message with a passphrase and decrypt it back. Record the ciphertext and confirm recovery.
8. **Asymmetric key pair.** Generate an RSA key pair with OpenSSL. Record the public and private key files. **Note explicitly which one must never leave your machine.**
9. **Signing.** Sign a message with the private key; verify with the public key. Then **alter one character** of the message and verify again. **Record the verification failure** — that is integrity, demonstrated.
10. **Signed but readable.** Build a minimal JWS by hand: Base64url a header and payload, sign, and join with dots. Then decode segments 1 and 2 **without any key**. **Record that the claims are fully readable.** This is the §6 lesson, proven.
11. **HMAC versus RSA.** Sign the same payload with `HS256` and a shared secret, then with `RS256`. Write one line on why possession of the HMAC secret allows forgery and possession of the RSA public key does not.
12. **Identification drill.** Collect ten strings — from your labs, from PEM files, from tokens, from hashes. Shuffle them. **Timed at 15 seconds each**, identify the operation, the guarantee, and whether it is readable. Score yourself.
13. **Reference + catalog.** Write `four-operations.md` — the §6 identification table, built from strings you personally produced. Add rows to the failure catalog. Complete `MANIFEST.md`.

**Expected evidence.** Encoded and decoded samples with timings, a Basic-auth demonstration, a Base64url decode failure, two SHA-256 digests showing avalanche, an encryption round trip, a generated key pair, a signature verification success and a tamper failure, a hand-built readable JWS, an HMAC-versus-RSA note, and a scored identification drill.

**Validation rubric.**

| Check | Pass condition |
|---|---|
| Decode is keyless and instant | Timing recorded; you can state it was trivial |
| Basic auth shown | Credentials recovered from the encoded form |
| Base64url failure reproduced | Standard decoder failing on a real segment |
| Avalanche shown | Two near-identical inputs, entirely different digests |
| Encryption round trip | Original recovered exactly |
| Tamper detected | One-character change causes verification failure |
| JWS readable without a key | Claims decoded with no key at all |
| HMAC vs RSA explained | In your own words, naming the forgery risk |
| Drill scored | Ten strings, 15 seconds each |

**Cleanup and privacy.** **Use fake values throughout** — `FAKEPASSWORD`, `a@example.com`, synthetic claims. Do not hash or encrypt any real credential. The generated private key is a real private key: keep it in the git-ignored `secrets/` folder and **delete it when the lab is complete**. Never paste a real token into an online decoder — decode locally, which is the entire habit this Part is building.

---

## 10. JD Mapping

| Supplied JD signal | How this Part supports it |
|---|---|
| **Knowledge of encryption** | The Part is the foundation of that requirement, distinguishing it from its three neighbours |
| Basic security concepts | Confidentiality versus integrity versus authenticity, made concrete |
| Understanding of authentication and authorization concepts | Why signing enables federation and why `HS256` breaks it |
| Strong analytical and problem-solving skills | §8's identification tree reads any unfamiliar string |
| **Promote best practices** | Asymmetric signing across trust boundaries; no sensitive data in JWT claims; pin `algorithms` |
| Customer-obsessed attitude | §8's worked example validates the security team's concern rather than dismissing it |
| Resolve technical and non-technical issues | "Is this a bug?" answered as a conceptual question with real options |

---

## 11. Candidate Honesty Note

- **Production transfer:** TLS/SSL is on your CV and you have handled certificate issues in enterprise escalations. The vocabulary here is partly familiar; the precision is what this Part adds.
- **The strongest thing you can say:** *"The distinction that causes the most harm is encoding versus encryption. Base64 looks unreadable and provides zero security — HTTP Basic auth is just Base64 of `user:password`. So when a customer says their claims are private because the token is encoded, or their config is safe because it's Base64, that's a finding, not a reassurance."*
- **A second strong point:** *"A signed JWT is readable by anyone holding it. Signing gives integrity and authenticity, not confidentiality. So sensitive data should never be in access token claims — it travels through browsers, proxy logs, and support tickets, and it's readable at every hop."*
- **A third, which is a genuine security judgement:** *"`HS256` uses one shared secret for signing and verifying, so every party that can verify can also forge. That's fine for an application signing its own session cookie and wrong for anything crossing a trust boundary. For federation it has to be asymmetric, and the verifier must pin the expected algorithm explicitly."*
- **Do not claim cryptographic expertise.** You use standard libraries correctly and can explain what each operation guarantees. That is exactly what "knowledge of encryption" means in this JD.
- **A good honest line:** *"I understand these at the level of choosing and validating the right control, not at the level of implementing the primitives — and I'd be suspicious of anyone in a support role who claimed otherwise."*

---

## 12. Official Source Anchors

Accessed **26 August 2026**.

| Source | What it defines |
|---|---|
| IETF RFC 4648 | Base64 and Base64url, including the character and padding differences |
| IETF RFC 7515 (JWS) | Signed token structure — three segments |
| IETF RFC 7516 (JWE) | Encrypted token structure — five segments |
| IETF RFC 7519 (JWT) | How JWS and JWE relate to JWT |
| IETF RFC 2104 (HMAC) | Symmetric signing |
| IETF RFC 7617 (HTTP Basic) | Confirms Basic auth is Base64, explicitly not encryption |
| NIST FIPS 180-4 | SHA-2 family, including SHA-256 |
| IETF JWT Best Current Practices (RFC 8725) | Algorithm pinning and the algorithm-confusion attack |
| OWASP — cryptographic storage cheat sheet | Password hashing versus encryption (Part 036) |
| OpenSSL documentation | The commands used in §9 |

**Revalidate after 26 August 2026:** algorithm recommendations evolve. The four operations and their properties do not.

---

## ⭐ Likely Interview Questions for This Section

### Q1. "What's the difference between encoding, hashing, encryption, and signing?"
> *Model answer:* "Four different jobs. Encoding changes representation so data survives transport — Base64, URL-encoding — and it provides *zero* security; anyone reverses it instantly with no key. Hashing produces a one-way fixed-size fingerprint, so you can verify a match but never recover the original — that's why passwords are hashed rather than encrypted. Encryption makes data unreadable without a key and is reversible with it, giving confidentiality. Signing proves the data came from a specific party and hasn't been altered, giving integrity and authenticity — but crucially *not* confidentiality, because signed content is fully readable. The confusion that causes the most harm is encoding versus encryption. HTTP Basic auth is just Base64 of `user:password`, and people genuinely believe it's protected because it looks scrambled."

### Q2. "A customer says their access tokens are secure because they're JWTs. Are they?"
> *Model answer:* "Not in the sense they mean, and it's worth correcting gently because the consequence is real. A standard JWT is a JWS — it's *signed*, not encrypted. The header and payload are Base64url-encoded, which is a transport representation rather than a security control, so anyone holding the token can read every claim. What the signature guarantees is that the claims can't be *altered* undetected, and that they came from the expected issuer. So if they've got personal data or anything sensitive in the payload, it's readable in browser history, proxy logs, application logs, and any support ticket the token gets pasted into. If they genuinely need confidentiality there's JWE, which is encrypted and has five segments rather than three, or an opaque reference token — but usually the right answer is simply not to put sensitive data in an access token."

### Q3. "Why shouldn't you use `HS256` for tokens consumed by other services?"
> *Model answer:* "Because HMAC is symmetric — the same secret both signs and verifies. So every party you allow to verify your tokens can also mint them. In a federated system that's fatal: your API needs the secret to verify, which means your API can now create tokens claiming to be from your identity provider, and so can every other service holding it. Rotation also becomes a coordinated outage because every holder has to change simultaneously. With RS256 or ES256 the private key stays with the issuer and verifiers only ever hold the public key, which is published openly at the JWKS endpoint — a verifier can't forge, a leaked public key is harmless, and rotation is transparent because verifiers refetch. `HS256` is fine when signer and verifier are the same party, like an app signing its own session cookie. Anything crossing a trust boundary should be asymmetric."

### Q4. "A customer says they encrypt passwords. Is that right?"
> *Model answer:* "No, and it's worth being direct about it. Encryption is reversible, so if the key is compromised every password in the database is recoverable in plaintext — and the operator can read them too, which is itself a problem. Passwords should be hashed with a purpose-built password hashing function like bcrypt or Argon2, which is one-way, salted, and deliberately slow. You never recover the password; you hash the submitted one and compare digests. The rule I'd give them is: if you need the original value back, hashing isn't an option — and for passwords you specifically *don't* want it back. A related question I'd ask is what they do with refresh tokens, because those genuinely do need to be recoverable, so those should be encrypted at rest, not hashed. People often apply one rule to both."

### Q5. "How do you tell whether a token is signed or encrypted?"
> *Model answer:* "Count the dots. Two dots means three segments — header, payload, signature — so it's a JWS and it's signed, and I can decode the first two segments locally with no key at all. Four dots means five segments, which is a JWE and it's encrypted, so it genuinely can't be read without the decryption key. That matters practically because 'we can't decode the token' is sometimes reported as a bug when it's actually a JWE working exactly as intended. The other quick identification I use is that Base64 of XML beginning `PHNhbWxw` is a SAML message, which lets me spot one in a HAR without decoding anything. And `-----BEGIN PRIVATE KEY-----` anywhere in customer evidence is an incident, not an observation."

### Q6. "Why does encoding exist if it provides no security?"
> *Model answer:* "Because it solves a transport problem, not a security one. Binary data contains bytes that would corrupt or be misinterpreted by text-based channels — HTTP headers, JSON strings, URLs — so Base64 re-expresses it using a safe character set. Percent-encoding does the same for characters that have structural meaning in a URL, like colons and ampersands. It's essential and it's not pretending to be anything else; the misconception is entirely on the reading side. The variant worth knowing is Base64url, which swaps `+` and `/` for `-` and `_` and usually drops the padding, because those two characters have meaning in URLs. JWTs use Base64url, so a developer decoding a token with a standard Base64 decoder can get an error or garbage — which is a real and quite common 'the token won't decode' ticket."

### Q7. "A customer's security team flagged emails visible in access tokens. How do you respond?"
> *Model answer:* "By correcting the misconception and validating the concern, because both are present. The mechanism first: a JWT is signed, not encrypted, so every claim is readable by anyone holding the token — that's by design, and the signature only prevents alteration. But their security team is right to flag it, because that token travels through browsers, proxies, logs, and support tickets, and it's readable at every hop. Then I'd give them options with trade-offs: remove the claim and have the API look it up; use an opaque access token, which needs introspection on every call; use JWE, which brings key management; or keep identity claims in the *ID token*, which stays with the client and isn't sent to APIs. Usually the last one is right — access tokens should carry authorization data, not profile data. And I'd check whether they're currently sending ID tokens to their API, because that would be a separate and more serious problem."

### Q8. "How deep is your cryptography knowledge?"
> *Model answer:* "At the level of choosing and validating the right control, not implementing primitives — and I'd be suspicious of anyone in a support role claiming otherwise. Concretely: I can tell you which of the four operations produced a given string and what it guarantees; why passwords are hashed and refresh tokens encrypted; why asymmetric signing is required across a trust boundary and what goes wrong with `HS256`; why a verifier must pin the expected algorithm explicitly, because otherwise there's an algorithm-confusion path; and how to verify a signature and demonstrate that a one-character change breaks it. I've done all of that hands-on with OpenSSL and Node rather than only reading about it. What I can't do is assess a novel cryptographic construction, and that's not what this role needs — it needs someone who can spot a customer using the wrong control and explain why."

---

## 🧠 30-Second Memory Hooks

- **Four operations, four jobs:** encoding = transport · hashing = one-way fingerprint · encryption = confidentiality · signing = integrity + authenticity.
- **Encoding provides ZERO security.** HTTP Basic auth is Base64 of `user:password`.
- **A signed JWT is READABLE BY ANYONE.** Signing hides nothing.
- **Never put sensitive data in access token claims.** It is readable at every hop.
- **Count the dots: 2 = JWS (signed, readable) · 4 = JWE (encrypted, not readable).**
- **Hashing is one-way.** If you need the value back, hashing is not an option.
- **Passwords: hash (bcrypt/Argon2). Refresh tokens: encrypt.** Different needs, different controls.
- **`HS256` = one shared secret = every verifier can FORGE.** Asymmetric across any trust boundary.
- **Pin `algorithms` explicitly** — otherwise algorithm confusion.
- **Base64url swaps `+/` for `-_`** and drops padding. Standard decoders fail on it.
- **`PHNhbWxw...` = Base64 XML = a SAML message.**
- **`-----BEGIN PRIVATE KEY-----` in customer evidence = an incident.**

---

## ✅ Completion Checklist

- [ ] **Knowledge:** I can define all four operations, state what each guarantees, and identify an unfamiliar string by sight.
- [ ] **Lab artifact:** `034-four-operations/` contains all four operations performed by hand, a tamper-detection failure, a hand-built readable JWS, and a scored identification drill.
- [ ] **Spoken:** I can explain "signed is not encrypted" and its consequence for claims in under 45 seconds.
- [ ] **Honesty check:** only fake values were used; the generated private key was stored git-ignored and deleted; nothing was pasted into an online decoder.
- [ ] **Source check:** I have read RFC 4648's Base64url section and RFC 7617's note that Basic auth is not encryption.

---

*Next suggested section:* **[Part 035 - Symmetric and Asymmetric Cryptography From Zero](Part-035-symmetric-and-asymmetric-cryptography-from-zero.md)** — go one level deeper into the key arrangements that make federation between untrusting parties possible at all.
