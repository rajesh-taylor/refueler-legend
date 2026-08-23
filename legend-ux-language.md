# legend-ux-language.md — refueler-legend
> **Version:** 1.0 | **Created:** Legend-3B · 6 Aug 2026
> Canonical UX copy and information hierarchy reference for Legend.
> Load in every build session that touches copy, modals, result states, or user-facing language.
> This document is the authority. If copy is not in this document, it does not exist yet.
> Companion documents: `legend-design-spec.md`, `legend-scope.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Section 1 — Voice and register

### The governing register

Legal document precision at reader-first readability. Not a developer's README. Not a marketing brochure. Not a compliance filing. The register sits where those three would intersect if they all had the same author who cared about the reader's time.

Every word on every Legend surface is written as if:
- The reader arrived from a breach notification and has thirty seconds
- The reader is a solicitor who will quote this in a client letter
- The reader is a new Bitcoiner who is not sure whether "UTXO" is a brand name

All three are the same reader at different moments. The copy must work for all three simultaneously.

### Copy rules

**Precision over completeness.** Say the true thing, then stop. If a second sentence is needed to qualify the first, the first sentence was overclaiming.

**Factual language over emotional language.** Results are facts. Users draw conclusions. Legend does not tell users how to feel about what they found.

**One idea per element.** One idea per paragraph in modal body copy. One idea per line in status strings. One idea per section heading.

**Honest scope before the reader asks.** State what Legend cannot see before anyone asks. State what Legend can see before anyone assumes it cannot. State the UK operator caveat before the Enterprise buyer raises it. Anticipate the honest question and answer it first.

**Short sentences.** Particularly in result states, modal titles, and status lines. If it reads well at speed, it reads well under stress.

### What Legend copy never does

- Uses "anonymous" — the correct word is "private"
- Uses "military-grade", "bank-grade", "enterprise-grade", or any grade-modifier
- Uses "Swiss-grade privacy" or any jurisdiction as a quality signal
- Uses "end-to-end" loosely — this phrase implies a specific architecture; use only if the claim is technically exact
- Uses "zero-knowledge" as a headline claim — v1 does not implement ZK proofs; the precise v1 claim is stated in Section 6
- Uses "PIR" without qualification — v1 is collusion-resistant query splitting, not Spiral PIR; the latter is v2
- Uses "anonymous" for Lightning payments — they are pseudonymous; this is stated
- Uses exclamation marks
- Overstates what a canary proves
- Calls a degraded mode "secure" — it is reduced, not broken, and the copy says so plainly
- Uses the phrase "Chainalysis works for the observer. Legend works for the owner." in UI copy — this phrase is locked for article and presentation use only (coined Multi-5)
- Implies that using Tor makes a free-tier query fully private — Tor hides IP; the query architecture is separate
- Claims "no logs" without the structural qualifier — the correct form is "no query logs exist. Not because we chose not to keep them. Because the architecture makes them impossible to create."

### What Legend copy always does

- States the honest scope first, the feature claim second
- Uses "private" not "anonymous"
- Uses "we can't see" with its structural qualifier — not as a policy promise
- Distinguishes IP privacy (Tor) from query privacy (architecture) when both are relevant
- Refers to itself in the third person only when necessary — "Legend" not "the platform" or "the tool"
- Uses sats not BTC as the default denomination label (toggle available)
- Gives the reader the next step when something has gone wrong

---

## Section 2 — Landing page hierarchy

### Above the fold

**Headline (locked CC-77):**
`Bitcoin, privately.`

**Subhead:**
`The block explorer that cannot see what you searched.`

This is the second sentence after the headline — no hairline above, no new section. It reads as the headline's explanation.

**Query input placeholder:**
`Address, transaction ID, or block height`

**Tertiary line beneath input (always visible, not a tooltip):**
`Private query. No logs. No tracking.`

### Below the fold — three sections

**Section 1: What Legend does differently**

*Title:* `How your query stays private`

*Body:*
Every public block explorer logs what you search. The address you typed, when you typed it, and the IP address it came from. That log is not deleted. It exists. It can be subpoenaed, sold, or leaked.

Legend is architected differently. Your query is split across two separate nodes before it reaches them, so neither node receives the complete question. Your query credential is a blind signature — the mint that issued it cannot link it to your session. When your result arrives, there is nothing in any server log to reconstruct. Not because we chose not to log it. Because the architecture makes the log impossible to create.

The code is open source. Read it.

**Section 2: Who it's for**

*Title:* `Built for the moment it matters`

*Body:*
The Bitcoiner who needs to check whether funds moved, without telling anyone what they were watching.

The accountant verifying a client payment privately, from an IP address attached to a regulated firm.

The family office monitoring addresses without broadcasting their client's holdings to a third party's server.

The solicitor who needs a verified statement of holdings at a specific block height for a probate file.

The same interface serves all of them. The data is identical across every tier. What differs is the institutional wrapper around it.

**Section 3: How the free tier works**

*Title:* `Free. No account. No catch.`

*Body:*
Legend costs roughly €360 a month to run — three dedicated servers in two jurisdictions, chosen for their legal geography, not their price. One Enterprise contract covers that cost many times over. Your free queries are not the product. They are the proof that the architecture works.

No account. No payment. No rate limit. No friction at the moment you need it most.

---

## Section 3 — Query flow copy

All strings are exact. Nothing in this section is approximate. Build sessions pull directly from this table.

### Idle state

Input placeholder: `Address, transaction ID, or block height`

Tertiary line: `Private query. No logs. No tracking.`

### Focused state

Batch icon appears. No copy change. Tertiary line remains.

### Submitting — PIR progress text

The tertiary line beneath the input is replaced by progress text. Five discrete states, updated as each stage completes:

`Querying node 1 of 4…`
`Querying node 2 of 4…`
`Querying node 3 of 4…`
`Querying node 4 of 4…`
`Assembling result.`

No spinner. Text updates only. The three states are shown in sequence, each replacing the last. They do not stack.

### Result states

**Intact (no outbound activity):**

Status line: `[N] UTXOs · [balance] sats · Last checked: [relative time]`

No additional copy on a clean result. The absence of alarm is the information.

**Funds moved (outbound transaction detected):**

Status line: `[N] UTXOs · [balance] sats · Last outbound: [amount] sats on [date] at block [height]`

Below the status line, one plain sentence:
`Last outbound: [amount] sats on [date] at block [height].`

No red. No alarm copy. Precision is the signal.

**Not found — standard address:**

Primary: `No activity found for this address.`

Secondary (DM Sans 300, tertiary colour): `This address has not appeared on-chain. If you're expecting a payment, it may not have confirmed yet.`

**Not found — Silent Payments address (BIP-352):**

Primary: `This is a Silent Payments static address (BIP-352).`

Secondary: `Outputs derived from this address do not appear on-chain as this address. Legend scans every block for derived outputs.`

Then either:

`No derived outputs found yet.`

Or the derived outputs table (block height, value, confirmation count).

**Error state:**

One sentence. Written from the reader's side of the screen.

Generic: `Something went wrong retrieving that result. Try again, or check that the address or transaction ID is correct.`

Timeout: `The query took too long to complete. The nodes may be under load. Try again in a moment.`

Invalid input: `That doesn't look like a Bitcoin address, transaction ID, or block height.`

---

## Section 4 — Result anatomy copy

### Status line format

`[N] UTXOs · [balance] sats · [activity line]`

Activity line variants:
- No activity: `Last checked: [relative time]`
- Last activity (inbound only): `Last activity: [relative time]`
- Outbound detected: `Last outbound: [amount] sats · [relative time]`

Separator: middot (·), not pipe. IBM Plex Mono for all values. DM Sans for labels. Same weight and colour throughout — no secondary-text treatment on any column.

### UTXO consolidation advisor flag

Appears when address has 5 or more UTXOs. Tertiary text. Below the UTXO table. Not a score. Not a recommendation.

Exact string (locked):
`Consolidating these UTXOs will permanently record co-ownership on-chain.`

Nothing follows this sentence. No link. No further explanation at the result level. The privacy explainer modal covers the architecture.

### Denomination toggle labels

Three states, session-persisted, reset on new session:

`sats` · `BTC` · `USD at time of transaction`

Toggle label (accessible): `Display denomination`

The third option is exact — "USD at time of transaction" not "USD" — because the fiat value at the time of a historical transaction is not the current fiat value and the distinction matters to an accountant.

### Silent Payments section

Section header (appears only on BIP-352 static address results):
`Derived outputs`

Body (appears above the derived outputs table):
`Payments to this Silent Payments address produce unique derived outputs per sender. Legend scans every block to find them.`

If no outputs found:
`No derived outputs found for this address. Scanning is complete to the current block height.`

Table columns (IBM Plex Mono throughout): Block height · Derived address (truncated) · Value (sats) · Confirmations

### Batch result values

Two values only. No others.

`Intact` — no outbound transaction detected.
`Activity detected` — at least one outbound transaction detected.

**Why not "Compromised" or "Swept":**

"Compromised" is a conclusion about cause. Legend knows what moved; it does not know why. An outbound transaction may be the holder moving funds to safety, not an attacker draining the wallet. The correct result is a fact, not an interpretation.

"Swept" implies the wallet is now empty. A partial outbound transaction is not a sweep. "Activity detected" is true in all cases where an outbound transaction exists. It is never true in cases where no outbound transaction exists.

The batch scenario (breach response) is the moment of highest stress for the user. Overclaiming in a result state at that moment causes harm. Factual language does not.

---

## Section 5 — Modal copy

### Onboarding modal

**Trigger:** first visit, no cookie `legend-seen`. Sets cookie on dismiss.

**Title:** `How Legend works`

**Body (three lines, exact):**
1. `Queries are split across separate nodes. Neither node receives your complete question.`
2. `No session is created. No query is stored. Nothing links one search to the next.`
3. `The code is open source. If you'd rather not take our word for it, read it.`

**Link:** `View source on GitHub` → `https://github.com/rajesh-taylor/refueler-legend`

**Dismiss CTA:** `Start querying`

No close icon in the top corner — the dismiss button is the only exit. This ensures the user reads at least the title before dismissing.

### Credential status modal

**Trigger:** credential status icon (top-right of SPA chrome).

**Free tier display:**
Title: `Private queries active`
Body: `You're querying privately. No account required. No query limit.`
Subline: `Your credential is a blind signature. The mint that issued it cannot link it to your session.`

**Enterprise display:**
Title: `Enterprise — [client name]`
Body: `Unlimited queries. PIR-sharded routing. Tor API endpoint active.`
Subline: `NUT-11 credential bound to your key. [Canary status: all nodes publishing / Node [X] canary expired — see status page]`

**Both tiers — footer line:**
`Architecture: collusion-resistant query splitting with blind credential unlinkability.`

### Batch input modal

**Title:** `Check multiple addresses`

**Textarea placeholder:** `Paste addresses, one per line. Up to 50 per batch.`

**Note below textarea:** `Each address is queried independently through separate node pairs. Results are assembled in your browser.`

**Button — disabled state:** `Run batch query` (disabled until two or more addresses present)
**Button — enabled state:** `Run batch query`
**Cancel:** `Cancel`

**Progress state (textarea replaced):**
`Checking address [N] of [total]…`

Cancellable during progress. Cancel button label: `Stop`

### Batch results modal

**Title:** `Batch results`

**Table columns:** Address (truncated Mono) · Status · Last activity · Balance (sats)

**Status values in table:** `Intact` · `Activity detected`

**Default sort:** Activity detected first.

**Single action:** `Copy results`

Copy output format (plaintext, newline-separated):
```
[address] — Intact — Last activity: [date] — [balance] sats
[address] — Activity detected — Last outbound: [date] — [balance] sats
```

No CSV. No filename in browser history. No third-party export.

**Footer line in modal:**
`Results are assembled in your browser. Nothing in this list was sent to a single server.`

### Privacy explainer modal

**Trigger:** `How is this private?` link below each result.

**Title:** `Why this query is private`

**Section 1 title:** `No session, no log`

**Section 1 body:** Your query is not associated with a session identifier, a cookie, or any persistent token. When your result arrives, there is no server-side record of the query that produced it. This is not a retention policy. The architecture makes the record impossible to create.

**Section 2 title:** `Split across two nodes`

**Section 2 body:** Your query is divided across two separate nodes before it reaches them. The first node receives a prefix — enough to identify a candidate set, not your specific address. The second node receives opaque row identifiers, not the original query. Neither node holds the complete question. Reconstructing your query from server logs would require both nodes to collude and retain data they are not designed to retain.

This is the v1 architecture: collusion-resistant query splitting with blind credential unlinkability. It is not single-server cryptographic PIR — that arrives in v2. The v1 claim is accurate and the v2 upgrade does not change the user interface.

**Section 3 title:** `Your IP and Tor`

**Section 3 body:** Your IP address is visible to the nodes you contact over standard HTTPS. The query architecture hides what you asked. It does not hide who asked. If you need IP privacy as well, route your traffic through Tor. Enterprise clients connect via a Tor hidden service endpoint, which is included in the Enterprise contract.

**Link text (appears after Section 3):** `Why Mempool over Tor isn't enough →` (links to Article 16 when published; hidden until published)

**Close CTA:** `Close`

---

## Section 6 — Honest-scope statements

These are the canonical user-facing versions of each architecture claim. They are shorter than the Enterprise contract versions. They are equally honest. Build sessions pull the exact strings from this section for the privacy explainer modal and the below-the-fold explainer.

### Ephemeral sessions

`No session is created when you query Legend. No cookie identifies you across queries. No server-side token links your first search to your second. Each query is structurally isolated from every other.`

### Role-split querying (v1 — precise language)

`Your query is split across two nodes. The first node sees a prefix of your query — enough to return a candidate set, not enough to identify your address. The second node sees opaque identifiers, not the original query. Neither node holds the full question. This is collusion-resistant query splitting: reconstructing what you asked requires both nodes to cooperate and to have retained data the architecture does not produce.`

`This is the v1 privacy architecture. It is not cryptographic single-server PIR. That arrives in v2 and does not change how you use Legend.`

### Cashu credentials

`Your query credential is a blind signature issued by a Cashu mint. The mint signs the credential without seeing the plaintext. The credential you present at query time cannot be linked to the issuance event. The mint knows a batch of credentials was requested. It cannot determine which credential authorised which query, or which queries came from the same session.`

### No server-side query logs

`There are no query logs on any Legend node. This is not a retention policy. A retention policy is a promise; promises can be broken. The architecture is the constraint: ephemeral sessions produce no persistent query record. There is nothing to log, nothing to subpoena, nothing to leak.`

### IP privacy

`Your IP address is visible to the Legend nodes you contact over standard HTTPS. The query architecture hides what you asked. It does not hide who is asking. If you need both, route through Tor. Enterprise clients use a Tor hidden service endpoint included in their contract.`

`Tor hides your IP from Legend. Legend hides your query from itself. You need both to be fully private. We give you the second. You supply the first if you need it.`

### UK operator caveat (user-facing register)

`All five Legend nodes are operated by one person in London. The UK Investigatory Powers Act 2016 permits compelled disclosure and non-disclosure orders served on the operator personally, regardless of where the hardware is located.`

`The geographic distribution of nodes across Germany, Luxembourg, Iceland, Canada, and Switzerland protects against legal orders served on the hosting providers in those jurisdictions. It does not protect against an order served on the operator under UK law.`

`This is stated here because a product that overstates its legal protections is weaker than one that understates them. The architecture does what it says it does. The legal boundary is this.`

---

## Section 7 — Degraded mode notices

These strings appear in the browser SPA. They are never suppressed. A degraded privacy claim is always surfaced to the user. Exact strings.

### N-1 — Two nodes operational (reduced splitting)

Banner, top of SPA chrome, non-dismissible until full splitting is restored:

`Operating on two nodes. Query splitting is active but reduced — both queries route through the same node pair. One node offline. Full splitting restores automatically when the third node returns.`

Privacy mode indicator (credential status modal): `Reduced splitting — one node offline`

### N-2 — One node operational (no splitting)

Banner, top of SPA chrome, non-dismissible:

`Operating on a single node. Query splitting is unavailable — this node can see your full query. If you need full privacy, consider routing through Tor while the other nodes are offline.`

Privacy mode indicator (credential status modal): `Single node — splitting unavailable`

**Copy notes:**

Neither notice uses "secure" or "insecure". Neither catastrophises. The N-2 notice offers a concrete mitigation (Tor). Both notices state when the degraded mode ends (automatically when the node returns; or, for N-2, implicitly — the user checks the status page).

The status page URL is not embedded in the banner. If the user wants the status page, they find it in the footer. The banner is a functional notice, not a navigation element.

---

## Section 8 — Locked copy index

Every finalised string in this document with its location. Build sessions pull from this table. If a string is not here, it is not finalised.

| String | Surface | Element | Section |
|---|---|---|---|
| `Bitcoin, privately.` | Landing page | Headline (above fold) | 2 |
| `The block explorer that cannot see what you searched.` | Landing page | Subhead (above fold) | 2 |
| `Address, transaction ID, or block height` | Query input | Placeholder | 2, 3 |
| `Private query. No logs. No tracking.` | Query input | Tertiary line (idle and focused) | 2, 3 |
| `How your query stays private` | Landing page | Section 1 title (below fold) | 2 |
| `Built for the moment it matters` | Landing page | Section 2 title (below fold) | 2 |
| `Free. No account. No catch.` | Landing page | Section 3 title (below fold) | 2 |
| `Querying node 1 of 4…` | Query input | Progress state 1 | 3 |
| `Querying node 2 of 4…` | Query input | Progress state 2 | 3 |
| `Querying node 3 of 4…` | Query input | Progress state 3 | 3 |
| `Querying node 4 of 4…` | Query input | Progress state 4 | 3 |
| `Assembling result.` | Query input | Progress state 5 | 3 |
| `No activity found for this address.` | Result | Not found — primary | 3 |
| `This address has not appeared on-chain. If you're expecting a payment, it may not have confirmed yet.` | Result | Not found — secondary | 3 |
| `This is a Silent Payments static address (BIP-352).` | Result | SP not found — primary | 3 |
| `Outputs derived from this address do not appear on-chain as this address. Legend scans every block for derived outputs.` | Result | SP not found — secondary | 3 |
| `No derived outputs found yet.` | Result | SP not found — tertiary | 3 |
| `Something went wrong retrieving that result. Try again, or check that the address or transaction ID is correct.` | Result | Error — generic | 3 |
| `The query took too long to complete. The nodes may be under load. Try again in a moment.` | Result | Error — timeout | 3 |
| `That doesn't look like a Bitcoin address, transaction ID, or block height.` | Result | Error — invalid input | 3 |
| `Consolidating these UTXOs will permanently record co-ownership on-chain.` | Result | UTXO consolidation advisor | 4 |
| `sats` · `BTC` · `USD at time of transaction` | Result | Denomination toggle labels | 4 |
| `Derived outputs` | Result | Silent Payments section header | 4 |
| `Payments to this Silent Payments address produce unique derived outputs per sender. Legend scans every block to find them.` | Result | Silent Payments section body | 4 |
| `No derived outputs found for this address. Scanning is complete to the current block height.` | Result | Silent Payments — no outputs | 4 |
| `Intact` | Batch results | Status value | 4 |
| `Activity detected` | Batch results | Status value | 4 |
| `How Legend works` | Onboarding modal | Title | 5 |
| `Queries are split across separate nodes. Neither node receives your complete question.` | Onboarding modal | Body line 1 | 5 |
| `No session is created. No query is stored. Nothing links one search to the next.` | Onboarding modal | Body line 2 | 5 |
| `The code is open source. If you'd rather not take our word for it, read it.` | Onboarding modal | Body line 3 | 5 |
| `View source on GitHub` | Onboarding modal | Link | 5 |
| `Start querying` | Onboarding modal | Dismiss CTA | 5 |
| `Private queries active` | Credential status modal | Free tier title | 5 |
| `You're querying privately. No account required. No query limit.` | Credential status modal | Free tier body | 5 |
| `Architecture: collusion-resistant query splitting with blind credential unlinkability.` | Credential status modal | Footer line (both tiers) | 5 |
| `Check multiple addresses` | Batch input modal | Title | 5 |
| `Paste addresses, one per line. Up to 50 per batch.` | Batch input modal | Textarea placeholder | 5 |
| `Each address is queried independently through separate node pairs. Results are assembled in your browser.` | Batch input modal | Note below textarea | 5 |
| `Run batch query` | Batch input modal | Submit button | 5 |
| `Stop` | Batch input modal | Cancel during progress | 5 |
| `Batch results` | Batch results modal | Title | 5 |
| `Copy results` | Batch results modal | Action | 5 |
| `Results are assembled in your browser. Nothing in this list was sent to a single server.` | Batch results modal | Footer line | 5 |
| `How is this private?` | Result | Privacy explainer trigger link | 5 |
| `Why this query is private` | Privacy explainer modal | Title | 5 |
| `No session, no log` | Privacy explainer modal | Section 1 title | 5 |
| `Split across two nodes` | Privacy explainer modal | Section 2 title | 5 |
| `Your IP and Tor` | Privacy explainer modal | Section 3 title | 5 |
| `Why Mempool over Tor isn't enough →` | Privacy explainer modal | Article 16 link (hidden until published) | 5 |
| `Close` | Privacy explainer modal | Dismiss CTA | 5 |
| `Operating on two nodes. Query splitting is active but reduced — both queries route through the same node pair. One node offline. Full splitting restores automatically when the third node returns.` | SPA chrome | N-1 degraded mode banner | 7 |
| `Reduced splitting — one node offline` | Credential status modal | N-1 privacy mode indicator | 7 |
| `Operating on a single node. Query splitting is unavailable — this node can see your full query. If you need full privacy, consider routing through Tor while the other nodes are offline.` | SPA chrome | N-2 degraded mode banner | 7 |
| `Single node — splitting unavailable` | Credential status modal | N-2 privacy mode indicator | 7 |

---

*"Nothing stops this train."*
