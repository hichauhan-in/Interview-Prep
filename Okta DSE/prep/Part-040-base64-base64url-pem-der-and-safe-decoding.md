# Part 040 - Base64, Base64url, PEM, DER, and Safe Decoding

> Section goal: Master the encodings identity actually runs on, and build the habit that protects you and your customers — decoding everything locally, never pasting a live credential into a third-party tool. This Part is short on theory and heavy on practice, because it produces tools you will use on every subsequent ticket.

Covers index item **040**. Maps to JD signals: *knowledge of encryption*, *basic security concepts*, *knowledge of HTTP*, *strong analytical and problem-solving skills*, and *promote best practices*.

---

## 1. Start From Zero: Why Encoding Exists Here

From Part 034: encoding changes representation so data survives a channel that would otherwise corrupt it. Identity is full of such channels.

| Channel | What it cannot carry | Encoding used |
|---|---|---|
| HTTP headers | Arbitrary bytes, newlines | Base64 |
| URLs | `/`, `+`, `?`, `#`, spaces | Base64url, percent-encoding |
| JSON strings | Raw binary | Base64 |
| Text files and email | Binary | PEM (Base64 plus delimiters) |
| XML attributes | Certain characters | Base64, XML entities |

```mermaid
flowchart LR
    Bin["Raw binary: a key, a signature, a certificate"] --> Need{"Where must it travel?"}
    Need -->|"An HTTP header"| B64["Base64 - 64 safe ASCII characters"]
    Need -->|"A URL"| B64U["Base64url - swaps + and / for - and _"]
    Need -->|"A text file"| PEM["PEM - Base64 plus BEGIN/END delimiters"]
    Need -->|"A query parameter value"| PCT["Percent-encoding - on top of the above"]
    B64 --> Zero["NONE of these provide any security - Part 034"]
    B64U --> Zero
    PEM --> Zero
    PCT --> Zero
```

> **Analogy.** Transcribing a photograph into words so it can be read down a telephone line. The photograph is unchanged; only its representation is. Anyone listening reconstructs it perfectly.
>
> **Where it stops:** a spoken description is lossy and slow. Base64 is exact and instant, which is precisely why it is a transport mechanism and never a protection.

---

## 2. Base64 and Base64url

**Base64** represents arbitrary bytes using 64 characters: `A–Z`, `a–z`, `0–9`, `+`, `/`, with `=` for padding.

Three bytes become four characters, so output is roughly **33% larger** than input — which matters when tokens travel in headers with size limits (Part 012).

### The two variants

| | Base64 | Base64url |
|---|---|---|
| Index 62 | `+` | `-` |
| Index 63 | `/` | `_` |
| Padding | `=` required | Usually **omitted** |
| Safe in a URL | ❌ | ✅ |
| Used by | Basic auth, PEM, SAML | **JWT, JWS, JWE, JWK, PKCE** |

### 🔍 Plain-English deep-dive: the three decoding failures, and how to tell them apart

"The token won't decode" is a real recurring ticket with three distinct causes.

**1. Wrong variant.** A standard Base64 decoder meets `-` or `_` and either errors or silently produces garbage.

*Tell:* the string contains `-` or `_` and has no `=` padding. **Fix:** substitute `-`→`+`, `_`→`/`, then decode.

**2. Missing padding.** Base64url usually omits `=`. Strict decoders reject an input whose length is not a multiple of four.

*Tell:* the error mentions length or padding. **Fix:** append `=` until the length is a multiple of four.

**3. Not actually Base64.** It is a JWE (five segments), an opaque reference token, or a URL-encoded value that must be percent-decoded first.

*Tell:* count the dots (Part 034), or look for `%` sequences.

```mermaid
flowchart TD
    F["'The token won't decode'"] --> D{"Count the dots"}
    D -->|"4 dots, 5 segments"| JWE["JWE - encrypted. Cannot be decoded without the key. Not a bug."]
    D -->|"No dots, no structure"| Op["Opaque reference token - only the issuer can resolve it"]
    D -->|"2 dots, 3 segments"| J{"Does it contain - or _ ?"}
    J -->|"Yes"| V["Base64URL - a standard decoder will fail or produce garbage"]
    J -->|"No"| P{"Is the segment length a multiple of 4?"}
    P -->|"No"| Pad["Missing padding - append = characters"]
    P -->|"Yes"| Pct{"Does it contain % sequences?"}
    Pct -->|"Yes"| URL["Percent-encoded first - decode that layer, then Base64"]
    Pct -->|"No"| Real["Genuinely malformed - truncated in transit, or copied incompletely"]
    V --> Fix["Substitute - to + and _ to /, then decode"]
    Pad --> Fix
```

**The "copied incompletely" case in that last box is worth knowing.** A customer pasting a token from a terminal or a log viewer frequently truncates it at a line wrap. If a token is a suspiciously round length or ends mid-segment, ask them to re-copy it from a HAR rather than a console.

**Analogy:** a phone number written in a different national format, or with the area code missing. The number is fine; the transcription convention differs. **Where it stops:** a wrong phone number rings someone else. A wrong decode usually fails loudly — except in the silent-garbage case, which is why substituting characters is safer than hoping.

---

## 3. PEM and DER

Certificates and keys exist in two forms.

| | DER | PEM |
|---|---|---|
| Format | **Binary** | Base64 of the DER, plus delimiters |
| Readable in a text editor | ❌ | ✅ |
| Safe to paste | ❌ | ✅ |
| File extensions | `.der`, `.cer` | `.pem`, `.crt`, `.cer`, `.key` |
| Typical use | Windows, Java, binary transport | Everything text-based |

```
-----BEGIN CERTIFICATE-----
MIIDXTCCAkWgAwIBAgIJAKL0UG+mRkSP...
-----END CERTIFICATE-----
```

**The delimiter line tells you what it is** — and this is a genuinely useful identification shortcut:

| Delimiter | Contents | Sensitivity |
|---|---|---|
| `BEGIN CERTIFICATE` | An X.509 certificate | **Public** — safe to share |
| `BEGIN PUBLIC KEY` | A public key | **Public** |
| `BEGIN PRIVATE KEY` | A private key (PKCS#8) | 🔴 **Secret — an incident if received** |
| `BEGIN RSA PRIVATE KEY` | An RSA private key (PKCS#1) | 🔴 **Secret** |
| `BEGIN ENCRYPTED PRIVATE KEY` | A passphrase-protected private key | 🔴 **Secret** |
| `BEGIN CERTIFICATE REQUEST` | A CSR | Public |

**Note that `.cer` appears in both rows.** The extension does not determine the format — open the file and look. A `.cer` may be DER or PEM, and treating one as the other is a common cause of "the certificate won't import."

### The multi-certificate PEM file

A PEM file can contain **several** certificates concatenated. That is exactly how a full chain is served (Part 037):

```
-----BEGIN CERTIFICATE-----   <- leaf
...
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----   <- intermediate
...
-----END CERTIFICATE-----
```

**Counting the blocks in a customer's bundle file is a fast chain check** — one block means an incomplete chain waiting to happen.

```mermaid
flowchart TD
    F["A certificate file from a customer"] --> B{"First bytes readable as text?"}
    B -->|"No - binary"| DER["DER - convert: openssl x509 -inform der"]
    B -->|"Yes - starts with -----BEGIN"| L{"Which delimiter label?"}
    L -->|"CERTIFICATE"| Pub["Public - safe to keep and quote"]
    L -->|"PUBLIC KEY"| Pub
    L -->|"CERTIFICATE REQUEST"| Pub
    L -->|"PRIVATE KEY / RSA PRIVATE KEY / ENCRYPTED PRIVATE KEY"| Sec["SECRET - incident. Do not copy or forward. Part 006."]
    Pub --> Count{"How many BEGIN CERTIFICATE blocks?"}
    Count -->|"1"| One["Leaf only - incomplete chain risk. Part 037."]
    Count -->|"2 or more"| Chain["Leaf plus intermediates - a proper bundle"]
    One --> Cmp["Compare against what the server ACTUALLY sends: s_client -showcerts"]
    Chain --> Cmp
```

### 🔍 Plain-English deep-dive: why the file extension lies, and what to do instead

Certificate files arrive as `.pem`, `.crt`, `.cer`, `.der`, `.key`, `.p12`, or with no extension at all — and **none of those reliably tells you the format or the contents.**

`.cer` in particular is used for both PEM and DER, on different platforms, by different tools. So "the certificate won't import" is very often a format mismatch rather than anything wrong with the certificate.

**The reliable check takes two seconds:** open the file and look at the first line.

| First line | Format | What it is |
|---|---|---|
| `-----BEGIN CERTIFICATE-----` | PEM | A certificate |
| `-----BEGIN PRIVATE KEY-----` | PEM | 🔴 A private key |
| Unreadable binary | DER | Could be either — inspect with `openssl` |
| `-----BEGIN PKCS7-----` | PEM | A certificate collection |
| Binary starting `0x30 0x82` | DER | An ASN.1 sequence — a certificate or key |

**Why this matters in support:** a customer says a certificate import failed, and the natural assumption is that the certificate is wrong. Very often it is correct and simply in the other encoding, and the fix is a one-line `openssl` conversion. Establishing the actual format before investigating anything else saves the whole detour.

**The second thing this check catches** is a private key where a certificate was expected — which is not a format problem at all, it is a credential exposure, and it needs the Part 006 response rather than a conversion command.

**Analogy:** a document filed under an extension that describes what someone intended rather than what it contains. Opening it takes a moment; guessing from the label wastes an afternoon. **Where it stops:** a document's contents are self-evident to a reader. DER is binary, so "open it and look" means `openssl`, not a text editor.

---

## 4. Decoding Safely

This is the operational heart of the Part.

### The rule

> **Decode locally. Never paste a customer's token, key, or credential into an online tool, a chat assistant, or any third-party service.**

### Why this matters more than it seems

| Risk | Reality |
|---|---|
| The tool logs input | Many do, and retention is unknown |
| The tool is operated by an unknown party | You cannot audit it |
| A signed JWT is a **live credential** until it expires | Pasting one is exfiltration |
| Browser history and clipboard | Persist after you close the tab |
| It normalises a dangerous habit | The one time it matters will look identical |

**Even removing the signature is not sufficient justification** for a third-party service, because the payload still contains personal data (Part 006). The habit has to be absolute, or it will fail exactly when it counts.

### 🔍 Plain-English deep-dive: what to say when a customer sends you a token

Customers paste tokens into tickets constantly. Your response does three jobs at once.

**1. Handle it safely.** Treat it as live: do not forward it, do not paste it anywhere, keep it in the ticket as the system of record (Part 006).

**2. Get what you need without asking for more risk.** You almost never need the whole token. Ask for the **decoded header and payload with the signature removed**, and name the specific claims you care about:

> *"I don't need the token itself — could you decode it locally and send me just the header and payload with the signature stripped? I mainly need `alg` and `kid` from the header, and `iss`, `aud`, `exp` and `scope` from the payload. Please redact any personal claims."*

**3. Teach the habit, in one sentence.** Include *how* to decode locally, so the ask is easy rather than an obstacle:

> *"`node -e "console.log(JSON.parse(Buffer.from(process.argv[1].split('.')[1],'base64url')))" <token>` will do it without the token leaving your machine."*

That message gets you better evidence, reduces exposure on both sides, and quietly establishes a practice the customer will keep using. **It also protects you** — you are not the person who asked a customer to hand over a live credential.

**If they have already pasted a live token**, note whether it is still valid. An expired token is inert and needs no action; a live one with a long lifetime may warrant a revocation recommendation.

**Analogy:** a bank asking for the last four digits rather than the full card number. Sufficient to identify, insufficient to misuse. **Where it stops:** a card number is fixed-length and structured. A token's claims vary, so you have to name which ones you actually need — otherwise the customer sends everything, which defeats the point.

---

## 5. The Decoding Toolkit

These are the commands you will use constantly. Build them once (§8) and keep them.

| Task | Approach |
|---|---|
| Decode a JWT segment | Base64url-decode segment 1 or 2, parse as JSON |
| Decode Base64 with padding issues | Append `=` to a multiple of four |
| Inspect a certificate | `openssl x509 -noout -text -in file.pem` |
| Read certificate SANs only | `openssl x509 -noout -ext subjectAltName` |
| Read certificate dates only | `openssl x509 -noout -dates` |
| Convert DER to PEM | `openssl x509 -inform der -in file.der -out file.pem` |
| Count certificates in a bundle | Count `BEGIN CERTIFICATE` occurrences |
| Inspect a JWKS | Fetch and pretty-print; list each `kid` and `alg` |
| Decode a SAML message | URL-decode, Base64-decode, then inflate if deflated (Part 082) |
| Decode a PKCE challenge | It is a **hash**, not encoding — it cannot be reversed |

**That last row is worth stating explicitly.** Developers occasionally ask how to "decode" a `code_challenge`. It is a SHA-256 digest (Part 059), so the answer is that it cannot be reversed — only recomputed from the verifier and compared.

---

## 6. Failure Modes

| Failure mode | What it looks like | Consequence | Correction |
|---|---|---|---|
| **Pasting a token into an online decoder** | Convenient, instant | **Live credential sent to an unknown party** | Decode locally, always |
| **Standard decoder on Base64url** | "The token won't decode" | Error or silent garbage | Substitute `-`→`+`, `_`→`/` |
| **Strict decoder, missing padding** | Length error | Failure on valid input | Append `=` to a multiple of four |
| **Trying to decode a JWE** | "Corrupted token" | Chasing a non-bug | Count the dots — four means encrypted |
| **Trying to reverse a hash** | "How do I decode the challenge?" | Impossible | It is a digest, not encoding |
| **Trusting the file extension** | `.cer` assumed PEM | Import fails | Open it and look at the first bytes |
| **Single-certificate bundle** | Chain incomplete | Server-side failures (Part 037) | Count `BEGIN CERTIFICATE` blocks |
| **Forwarding a private key** | Key in a ticket, then in an email | **Catastrophic** | Incident process; never copy or forward |
| **Truncated paste** | Decode fails mid-segment | Wasted round trip | Ask for a re-copy from a HAR, not a console |
| **Asking for the whole token** | Customer sends a live credential | Unnecessary exposure | Ask for named claims, signature stripped |
| **Assuming Base64 means protected** | "It's encoded so it's fine" | Part 034's core misconception | Encoding provides zero security |

---

## 7. Troubleshooting Decision Tree: Decoding Anything

```mermaid
flowchart TD
    S["An opaque string in evidence"] --> A{"What are the first characters?"}
    A -->|"-----BEGIN"| P{"Which delimiter?"}
    P -->|"CERTIFICATE"| C["Public - openssl x509 -noout -text"]
    P -->|"PUBLIC KEY"| PK["Public - safe"]
    P -->|"PRIVATE KEY or RSA PRIVATE KEY"| Inc["INCIDENT - do not copy or forward. Part 006."]
    A -->|"eyJ..."| J["Base64url of JSON - almost certainly a JWT header"]
    A -->|"PHNhbWxw..."| S2["Base64 of XML - a SAML message. Part 082."]
    A -->|"Contains % sequences"| U["Percent-encoded - decode that layer FIRST"]
    A -->|"Random, no structure"| O["Opaque reference token - cannot be decoded, only introspected"]
    A -->|"Binary, unreadable"| D["Likely DER - convert with openssl -inform der"]
    J --> Dots{"How many dots?"}
    Dots -->|"2"| JWS["JWS - decode segments 1 and 2 LOCALLY"]
    Dots -->|"4"| JWE["JWE - encrypted, cannot read. Expected behavior."]
    JWS --> Claims["Read: alg, kid, iss, aud, exp, scope - that is usually the whole answer"]
```

### Worked example

*"Your API is rejecting our token. Here it is: eyJhbGciOiJSUzI1NiIsImtpZCI6..."* — pasted in full into the ticket.

1. **First, handle the credential.** It is a live token. Do not paste it anywhere, do not forward it. Check `exp` once decoded — if still valid, note that in the case.
2. **Decode locally.** Two dots, so it is a JWS. Base64url-decode segments 1 and 2.
3. **Read the six values that matter:** `alg`, `kid`, `iss`, `aud`, `exp`, `scope`.
4. **Finding:** `aud` is the tenant's UserInfo endpoint rather than their API identifier — Part 033's row 21.
5. **Answer with the cause**, using the Part 004 structure.
6. **Then handle the credential exposure, kindly and briefly:**

> *"One quick note before the fix: the token you pasted was still valid when I received it. That happens constantly and it's an easy thing to miss, but access tokens are live credentials, so for future tickets it's safer to send just the decoded header and payload with the signature stripped. Here's a one-liner that decodes it without the token leaving your machine: `node -e "console.log(JSON.parse(Buffer.from(process.argv[1].split('.')[1],'base64url')))" <token>`. I mainly need `alg`, `kid`, `iss`, `aud` and `exp`."*

7. **Note it internally** per whatever the process is for credentials appearing in tickets.

That response resolves the technical issue, protects the customer without embarrassing them, and gives them a tool that makes the safer path the easier one — which is the only kind of security advice that sticks.

---

## 8. Lab: Build Your Decoding Toolkit

**Purpose.** Produce the local decoding tools you will use on every future ticket, so the safe path is also the fastest one.

**Prerequisites.** Part 007's lab — Node or Python, OpenSSL. Parts 034–039's artifacts. **All local.**

**Steps.**

1. Create `okta-prep/labs/040-decoding/`.
2. **`jwt-decode`.** Write a script taking a token and printing the decoded header and payload as formatted JSON. It must: handle Base64url correctly, add missing padding, **never print or transmit the signature**, and make **no network calls**. Test it against a real token from your lab tenant.
3. **Human-readable claims.** Extend it to convert `exp`, `iat`, and `nbf` from Unix timestamps into readable dates, and to print whether the token is currently expired and by how long. **This turns a raw dump into an answer.**
4. **Segment counter.** Extend it to count dots and refuse politely on a JWE — printing "5 segments: this is an encrypted JWE and cannot be decoded without the key" rather than failing with a parse error.
5. **`b64` helper.** Write a script that decodes both Base64 and Base64url, auto-detecting the variant and adding padding. Test it against: a standard Base64 string with `+`, `/` and `=`; a Base64url string with `-` and `_` and no padding; and a percent-encoded Base64url string.
6. **Reproduce all three decode failures.** Deliberately decode a Base64url string with a strict standard decoder, decode an unpadded string with a strict decoder, and attempt to decode a JWE. **Record all three exact errors.** These are the tickets.
7. **`cert-info`.** Write a script that, given a PEM file or a host:port, prints subject, issuer, SANs, dates, and **how many certificates are in the bundle**. Test it against your Part 037 chain and your lab tenant.
8. **DER conversion.** Convert one of your certificates to DER, confirm it is unreadable as text, convert it back, and verify it matches the original.
9. **Bundle check.** Create a bundle containing only a leaf and one containing leaf plus intermediate. Confirm your script reports 1 and 2. **This is a fast incomplete-chain check.**
10. **JWKS reader.** Write a script that fetches a JWKS URL and prints a table of `kid`, `kty`, `alg`, and `use` for each key. Run it against your lab tenant. **Record how many keys are published** — more than one usually means a rotation overlap (Part 035).
11. **SAML decoder stub.** Write a script that URL-decodes, Base64-decodes, and inflates a deflated SAML message, printing the XML. Test it with a synthetic deflated message you create yourself. (Part 082 uses this properly.)
12. **Safety audit.** Review every script and confirm: no network calls except the two that must fetch (JWKS, and `cert-info` against a host), no logging of signatures or full tokens to disk, and no clipboard access. **Write a one-line safety statement at the top of each file.**
13. **Customer one-liner.** Write `decode-locally.md` — the short message from §4 including a copy-pasteable one-liner for Node, Python, and PowerShell, so you can send whichever matches the customer's stack.
14. **Failure catalog + manifest.** Add the three decode failures. Complete `MANIFEST.md`.

**Expected evidence.** Five working scripts, three reproduced decode failures with exact errors, a DER round trip, a bundle count contrast, a JWKS key table, a SAML decode of a synthetic message, a safety audit, and a customer-ready one-liner in three languages.

**Validation rubric.**

| Check | Pass condition |
|---|---|
| JWT decoder handles Base64url and padding | Works on real tokens from your tenant |
| Signature never printed | Verified by reading the code |
| Timestamps humanised | `exp` shown as a date and an expired/valid verdict |
| JWE handled gracefully | Clear message, not a parse error |
| Three failures reproduced | Verbatim errors recorded |
| Bundle counter | Correctly reports 1 versus 2 |
| JWKS table | Every key listed with `kid` and `alg` |
| No unnecessary network calls | Audited and stated per file |
| One-liner in three languages | Tested, each actually works |

**Cleanup and privacy.** Everything runs locally. **Never paste a token into an online tool at any point in this lab** — the entire purpose is building the opposite habit. Use tokens only from your own lab tenant with synthetic users, redact them from any saved output, and store any private key or bundle in the git-ignored `secrets/` folder.

---

## 9. JD Mapping

| Supplied JD signal | How this Part supports it |
|---|---|
| **Knowledge of encryption** | The encodings cryptographic material actually travels in |
| **Basic security concepts** | §4's local-decoding rule and safe credential handling |
| Knowledge of HTTP | Base64 in headers, percent-encoding in URLs, PEM in configuration |
| Strong analytical and problem-solving skills | §7's tree identifies any opaque string in seconds |
| **Promote best practices** | Teaching customers to decode locally, with a one-liner that makes it easy |
| Exceed expectations on response quality | Asking for named claims rather than a whole live token |
| Customer-obsessed attitude | §7's correction protects the customer without embarrassing them |

---

## 10. Candidate Honesty Note

- **Production transfer:** you have handled encoded diagnostic data and certificates in enterprise support. The mechanics here are partly familiar; the discipline and the tooling are what this Part adds.
- **The strongest thing you can say:** *"I decode everything locally and I have scripts for it. A signed JWT is a live credential until it expires, so pasting one into an online decoder is exfiltration — and the habit has to be absolute, because the time it matters will look exactly like the times it didn't."*
- **A second strong point, and it is genuinely good support craft:** *"I don't ask customers for the whole token. I ask for the decoded header and payload with the signature stripped, name the specific claims I need, and include a one-liner that decodes it without the token leaving their machine. That gets me better evidence, reduces exposure on both sides, and makes the safe path the easy one — which is the only kind of security advice that actually sticks."*
- **A third, small and practical:** *"'The token won't decode' has three causes: a standard decoder meeting Base64url, missing padding, or it being a JWE with five segments. Counting the dots settles the third one instantly."*
- **A fourth:** *"Counting `BEGIN CERTIFICATE` blocks in a bundle is a ten-second incomplete-chain check. One block is a chain problem waiting to happen."*
- **Do not overstate:** these are small utility scripts, not tooling engineering. Their value is the habit they encode, and saying that plainly is more credible than inflating them.

---

## 11. Official Source Anchors

Accessed **26 August 2026**.

| Source | What it defines |
|---|---|
| IETF RFC 4648 | Base64, Base64url, padding rules, and the character differences |
| IETF RFC 7468 | PEM textual encodings and the exact delimiter labels |
| ITU-T X.690 | DER encoding |
| IETF RFC 7515 (JWS) and RFC 7516 (JWE) | Segment counts and which parts are Base64url |
| IETF RFC 7517 (JWK) | JWKS structure and the fields in §8's key table |
| IETF RFC 3986 | Percent-encoding |
| OASIS SAML 2.0 Bindings | Deflate plus Base64 for redirect binding (Part 082) |
| OpenSSL `x509`, `req`, `pkey` documentation | Every certificate command in §5 |
| Node.js `Buffer` and Python `base64` documentation | `base64url` support and padding behavior |

**Revalidate after 26 August 2026:** nothing here changes. The encodings are stable, which is why the toolkit is worth building once.

---

## ⭐ Likely Interview Questions for This Section

### Q1. "How do you decode a customer's token?"
> *Model answer:* "Locally, always, with a script I keep for it. A signed JWT is a live credential until it expires, so pasting one into an online decoder means sending an active credential to a party neither I nor the customer can audit — and many such tools log their input. Even stripping the signature isn't sufficient justification, because the payload still contains personal data. My decoder handles Base64url properly, adds missing padding, converts `exp` and `iat` into readable dates, tells me whether the token is expired and by how much, and never prints the signature. It makes no network calls. The habit has to be absolute rather than case-by-case, because the one time it genuinely matters will look identical to all the times it didn't."

### Q2. "A customer says the token won't decode. What are the causes?"
> *Model answer:* "Three, and counting the dots separates them. If there are four dots and five segments it's a JWE — encrypted, and genuinely not decodable without the key, so it's expected behavior rather than a bug. If there are two dots it's a JWS, and then either they're using a standard Base64 decoder on Base64url, which swaps `+` and `/` for `-` and `_` and will error or produce silent garbage, or the segment is missing its `=` padding, which strict decoders reject because the length isn't a multiple of four. The fixes are substituting the two characters, and appending padding. There's a fourth possibility worth checking too: a truncated paste. If a token ends mid-segment or is a suspiciously round length, they've probably copied it from a console that wrapped the line, and I'd ask them to re-copy from a HAR."

### Q3. "A customer pastes a live access token into a ticket. What do you do?"
> *Model answer:* "Handle it, then use it, then teach — in that order. Handling: treat it as live, don't paste or forward it anywhere, keep it in the ticket as the system of record, and check `exp` to see whether it's still valid, because an expired one is inert. Using: decode it locally and read the six values that usually answer the question — `alg`, `kid`, `iss`, `aud`, `exp`, `scope`. Teaching: one short, unembarrassing sentence noting that access tokens are live credentials, that this happens constantly, and that for future tickets the decoded header and payload with the signature stripped is enough — plus a one-liner that decodes it without the token leaving their machine. Giving them the tool is what makes the advice stick; telling them off just makes them defensive and they'll do it again anyway."

### Q4. "What's the difference between PEM and DER, and why does it matter?"
> *Model answer:* "DER is the binary encoding of a certificate or key; PEM is Base64 of that binary with `BEGIN` and `END` delimiter lines around it. Same content, different representation. It matters practically for two reasons. First, the file extension doesn't tell you which one you have — `.cer` is used for both, so 'the certificate won't import' is often just a format mismatch, and the fix is a one-line `openssl` conversion rather than anything to do with the certificate itself. Second, the delimiter label is a fast identification: `BEGIN CERTIFICATE` is public and safe to share, `BEGIN PRIVATE KEY` or `BEGIN RSA PRIVATE KEY` in customer evidence is an incident. And a PEM file can hold several certificates concatenated, which is how a full chain is served — so counting `BEGIN CERTIFICATE` blocks is a ten-second incomplete-chain check."

### Q5. "How do you get evidence from a customer without asking for credentials?"
> *Model answer:* "By asking for the smallest sufficient artifact and naming exactly what I need. For a token that's the decoded header and payload with the signature removed, and I'd list the claims — `alg` and `kid` from the header, `iss`, `aud`, `exp` and `scope` from the payload — plus a note to redact personal claims. Naming them matters, because 'send me the decoded token' gets you everything and defeats the purpose. And I include a copy-pasteable one-liner in whichever language matches their stack, because the safe path has to also be the easy path or people won't take it. It's the same principle as HAR redaction: I need header names and cookie attributes, not values, and I don't need signatures. Being specific about that usually gets a usable answer immediately instead of a negotiation about whether they're allowed to send it."

### Q6. "Can you decode a PKCE `code_challenge`?"
> *Model answer:* "No — it's a SHA-256 hash, not an encoding, so there's nothing to reverse. That question comes up because it *looks* like Base64url and everything else that looks like that is decodable. The only thing you can do with a challenge is recompute it: take the verifier, hash it with SHA-256, Base64url-encode the digest, and compare. That's exactly what the authorization server does at the token endpoint. It's a good example of why the four-operations distinction matters — encoding is reversible by anyone, hashing is one-way by design, and confusing them leads to asking for something impossible. Practically, if a customer is debugging a PKCE mismatch, the useful move is to have them log both the verifier and the challenge their code generated and confirm the challenge is genuinely the hash of that verifier."

### Q7. "You mentioned counting `BEGIN CERTIFICATE` blocks. Why?"
> *Model answer:* "Because a PEM bundle can contain multiple concatenated certificates, and that's exactly how a full chain is served — leaf first, then intermediates. So counting the blocks in whatever file a customer has configured as their certificate tells me immediately whether they're serving a chain or just a leaf. One block means an incomplete chain waiting to happen, which produces the 'works in my browser but our backend can't connect' pattern. It's a ten-second check I can do on a file they've already sent, before asking them to run anything. It pairs with `openssl s_client -showcerts` counting what the server *actually* sends — sometimes the bundle is right and the server configuration only references the leaf, and comparing the two locates which of those it is."

### Q8. "Why build your own decoding scripts rather than using existing tools?"
> *Model answer:* "Mainly so the safe option is also the fastest one. If decoding locally takes three commands and remembering flags, people paste into a web tool instead — including me, on a bad day. A script that takes a token and prints the claims with readable dates and an expired-or-not verdict removes that temptation entirely. It also lets me build in the things I actually want: never printing the signature, handling Base64url and padding automatically, giving a clear message on a JWE rather than a parse error, and making no network calls at all, which I can state and audit. I'd be honest that these are small utility scripts rather than tooling engineering — the value is the habit they encode, not the code. But they're the tools I'd reach for on every ticket, so building them once was worth it."

---

## 🧠 30-Second Memory Hooks

- **Encoding = transport, never protection.** Base64 is reversed instantly by anyone.
- **Base64url swaps `+/` → `-_` and drops padding.** JWT, JWS, JWE, JWK, PKCE all use it.
- **"Token won't decode" = 3 causes:** wrong variant · missing padding · it is a **JWE**. **Count the dots.**
- **Truncated paste is a fourth** — ask for a re-copy from a HAR, not a console.
- **PEM = Base64 of DER + delimiters.** The **delimiter label** identifies it, not the file extension.
- **`BEGIN PRIVATE KEY` in customer evidence = an incident.**
- **Count `BEGIN CERTIFICATE` blocks** — one means an incomplete chain waiting to happen.
- **Decode LOCALLY. Always. Absolutely.** A signed JWT is a live credential.
- **Ask for named claims, signature stripped** — not the whole token.
- **Send them the one-liner.** The safe path must also be the easy path.
- **A `code_challenge` is a HASH.** It cannot be decoded, only recomputed.

---

## ✅ Completion Checklist

- [ ] **Knowledge:** I can name the three decode failures, identify a string from its first characters, and state what each PEM delimiter means.
- [ ] **Lab artifact:** `040-decoding/` contains five working local scripts, three reproduced decode failures, a bundle count contrast, a JWKS key table, and a customer one-liner in three languages.
- [ ] **Spoken:** I can explain why I decode locally and deliver the customer correction in under 45 seconds.
- [ ] **Honesty check:** no token was pasted into any online tool during this lab; every script makes only the network calls it must, and says so.
- [ ] **Source check:** I have read RFC 4648's Base64url section and RFC 7468's delimiter labels myself.

---

*Next suggested section:* **[Part 041 - JWT Anatomy From Zero](Part-041-jwt-anatomy-from-zero.md)** — with the toolkit built, take a token apart claim by claim and learn what every field is actually for.
