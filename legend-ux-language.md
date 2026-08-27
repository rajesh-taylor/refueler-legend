# legend-ux-language.md — refueler-legend
> **Version:** 1.5 | **Created:** Legend-3B · 6 Aug 2026 | **Updated:** Multi-15 · 26 Aug 2026
> Canonical UX copy and information hierarchy reference for Legend — Sections 1–5.
> Honest-scope statements (§6), degraded mode notices (§7), and locked copy index (§8)
> are in `legend-copy-index.md`.
> Load in build sessions that touch copy, modals, result states, or user-facing language.
> This document is the voice and register authority. `legend-copy-index.md` is the string authority.
> Companion documents: `legend-design-spec.md`, `legend-copy-index.md`, `legend-scope.md`, `CLAUDE.md`.

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
- Uses "zero-knowledge" as a headline claim — v1 does not implement ZK proofs; the precise v1 claim is stated in `legend-copy-index.md` §6
- Uses "PIR" without qualification — v1 is collusion-resistant query splitting, not Spiral PIR; the latter is v2
- Uses "anonymous" for Lightning payments — they are pseudonymous; this is stated
- Uses exclamation marks
- Overstates what a canary proves
- Calls a degraded mode "secure" — it is reduced, not broken, and the copy says so plainly
- Uses the phrase "Chainalysis works for the observer. Legend works for the owner." in UI copy — this phrase is locked for article and presentation use only (coined Multi-5)
- Implies that using Tor makes a free-tier query fully private — Tor hides IP; the query architecture is separate
- Claims "no logs" without the structural qualifier — the correct form is "no query logs exist. Not because we chose not to keep them. Because the architecture makes them impossible to create."
- Forecasts how long a fee spike will last — comparable-spike history is history, not a prediction
- Asserts that a mempool event is an attack — cause is provenance Legend cannot establish from the chain alone

### What Legend copy always does

- States the honest scope first, the feature claim second
- Uses "private" not "anonymous"
- Uses "we can't see" with its structural qualifier — not as a policy promise
- Distinguishes IP privacy (Tor) from query privacy (architecture) when both are relevant
- Refers to itself in the third person only when necessary — "Legend" not "the platform" or "the tool"
- Uses sats not BTC as the default denomination label (toggle available)
- Gives the reader the next step when something has gone wrong

### Bereavement register

**Locked: Multi-12 · 23 Aug 2026**
**Context:** UC-1 — The Bradford Inheritance. Distress Mode. Mobile. First query. Single address.

This register applies when the inferred context is bereavement or estate lookup. It is not a named mode. The copy adapts to the detected context (mobile, first visit, single address) without naming the adaptation.

*Note: bereavement is one of two Distress-context registers. The other is the Affordability register (UC-7). Distress Mode is not inherently bereavement — the two registers sit alongside each other under the same inferred mode.*

**Register rules:**

- Lead with the fact, not the instruction. The user has arrived in distress. They need the answer first, the explanation second.
- GBP equivalent appears without being asked. In the bereavement context, the user thinks in pounds. Sats are the correct denomination; GBP is the correct first denomination in this mode only.
- Plain-language movement statement before any transaction detail. "These funds have not moved" or "Funds were moved on [date]" before any technical data.
- No "here's how Legend works." The onboarding modal is suppressed. They came to check, not to be taught.
- No denomination toggle in primary display. One number, legible, accurate.
- Document CTA is present from the first result. `Save a copy for your records →` is not a secondary action.

**What bereavement register copy never does:**
- Uses "wallet" as the primary noun — use "funds" or "these holdings"
- Uses technical abbreviations in primary display — no TXID, no UTXO, no vB
- Expresses alarm about funds that have not moved — absence of movement is the correct and reassuring answer
- Underplays funds that have moved — plain language, no euphemism: "Funds were moved on [date]"

**Denomination rule for bereavement / Distress Mode:**
GBP-first for holdings. Sats visible but secondary. The user is in Bradford. She thinks in pounds.
This overrides the sats-default rule for this mode only. The denomination rule is contextual, not global.
Network-cost figures (if the Fee Context Layer is active) are USD-first regardless — see §4.

### Stewardship register

**Locked: UC-2 Opus · 23 Aug 2026**
**Context:** UC-2 — The Grandparent's Ledger. Stewardship Mode. Desktop. Unhurried. 1–2 addresses.

This register applies when the inferred context is deliberate long-term holding review. Arthur on a Sunday afternoon. Not frightened. Not in distress. Preparing something for people he loves.

**Register rules:**

- Lead with stillness. The most important sentence for Arthur is that nothing has moved. That comes first.
- Sats-first denomination. Arthur is a returning owner. He thinks in sats, or at least is comfortable with them. The denomination rule is sats-first for Stewardship Mode. GBP and BTC are visible but secondary.
- Full addresses visible. Not truncated. Arthur is preparing a document for a solicitor. The full address is the estate record.
- Verification anchor present and prominent. The block height and timestamp are the line Arthur reads to his solicitor.
- Print is a first-class affordance, not an afterthought. The interface is designed to produce a document.
- No jargon in primary display. "Unspent amount" not "UTXO." "Verified at block [height]" not "chain height."

**What stewardship register copy never does:**
- Uses alarm language for a stable result — calm is correct
- Truncates addresses in the primary display — full addresses are the estate record
- Buries the print affordance — it is first-class
- Uses GBP as the primary denomination — sats-first in this mode (Arthur is the owner, not the frightened newcomer)
- Uses "account" or "wallet" — the neutral term is "address" or "holdings"

**Denomination rule for stewardship / Stewardship Mode:**
Sats-first. GBP and BTC visible but secondary. Arthur is the returning owner.
This is the default denomination rule applied correctly to the stewardship context.
The distinction from Distress Mode is deliberate: same data, different denomination hierarchy,
different user state. Both are correct. `legend-copy-index.md` §8 states the rule explicitly:
denomination hierarchy is contextual, not global.

### Verification / disclosure register

**Locked: UC-3 Opus · 23 Aug 2026**
**Context:** UC-3 — The Bitcoin-Backed Loan. Verification Mode (generation) and
Recipient View / Lender View (consumption). The one moment Legend helps a user
*disclose*, not conceal.

This register is a sibling to the bereavement and stewardship registers. It is
structurally different from both: those registers govern *privacy-preserving*
moments. This one governs a *voluntary disclosure* to a counterparty. The register
exists because the honest failure mode is inverted — not leaking what was hidden,
but letting a user believe a thing they chose to show is still private from the
recipient.

**Register rules:**

- Lead with what the verification *does not* prove before stating what it proves.
  The absence (control, future state, lien) is where users make risky assumptions.
- State the disclosure plainly in the generation modal. The borrower has chosen
  to reveal their address. The copy confirms this without alarm, without apology,
  and without softening it. It is a choice, stated as a fact.
- GBP-first in the Recipient View. The counterparty — lender, solicitor, council
  officer — thinks in pounds. Denomination follows the reader's relationship to
  the holding.
- The honest-scope footer is not small print. It is part of the document.
  Do not hide it, collapse it, or render it in `--fg-subtle` only.
- The verification anchor (block height + date) is the line the recipient quotes
  to their compliance team. It must be findable in two seconds on a large desktop.
- Never imply the verification is a lien, an escrow, or a custody arrangement.
  Never imply it prevents future movement. State the limit once, plainly.

**What verification/disclosure register copy never does:**
- Implies that a v1 shareable link proves control — it proves existence and stillness only
- Implies that sharing a verification keeps the address private — the borrower has chosen to reveal it
- Implies Legend can recall or revoke a verification once shared
- Uses "guarantee," "locked," or "secured" in relation to the collateral state
- Calls the proof-of-control step "optional" at v2 — it is available; whether to use it is the borrower's choice, stated as such

**Denomination rule for Recipient View / verification context:**
GBP-first. The professional counterparty reads in fiat. Sats visible, secondary.

**Unifying denomination principle (stated once, governs all three contextual rules):**
Denomination follows the reader's relationship to the holding.
- Owner, returning and deliberate → sats (Stewardship Mode)
- Non-owner, frightened → GBP (Distress Mode)
- Non-owner, professional counterparty → GBP (Recipient / Verification context)

The chain data is identical in all three cases. The denomination hierarchy reflects
the reader, not the holding.

### Institutional register — Treasury Watch (locked UC-4 Opus)

**Context:** UC-4 — The Council and the Whale. Watch Recipient View. Institutional
counterparty. Desktop. Ongoing covenant oversight.

Applied on the Watch Recipient View and any institutional/oversight surface. The
plain-language principle holds; the vocabulary shifts to the professional
counterparty's frame.

| General term | Institutional term |
|---|---|
| wallet / address | collateral address (or: address) |
| balance / UTXO set | verified balance |
| transaction detected / movement alert | movement status / movement recorded |

`alert` is reserved strictly for v3 blind-channel push notification. At v1/v2 the
word is `status`. Never call a watch `secure`; it is live, not protective. Never
imply a watch prevents, freezes, or holds funds.

**Denomination rule for Watch Recipient View:**
GBP-first. Institutional counterparty thinks in pounds. Sats visible, secondary.
Extends the unifying principle: denomination follows the reader's relationship to
the holding. The institutional counterparty is a non-owner professional — fiat-first.

### Civic transparency register — locked UC-5 Opus · Multi-14

**Context:** UC-5 — The Florentine District. Civic Treasury View and (v2) Civic Treasury
Mode. Voluntary public institutional disclosure to an unbounded audience, indefinitely.

Sibling to the verification/disclosure and institutional registers. The failure mode
differs from both: not "user believes a shown thing is private," and not "reader treats
a pull link as a push alarm" — but **"observer reads a declared, voluntary, partial
disclosure as a complete institutional audit"** and **"observer reads Legend's rendering
as an endorsement of the publisher's identity."** Both failure modes must be guarded
against before anything else appears on the screen.

**Register rules:**

- State the identity limit first and plainly. Legend renders a name as provided;
  it verifies chain data, not the institution behind the name. This sentence is the
  first thing the reader sees after the header.
- State that the declared set is not exhaustive before the reader assumes it is.
  The footer carries this; the interface does not contradict it anywhere above the fold.
- Render the Silent Payments absence as *design*, precisely: the outflow amounts and
  cadence are visible; the recipients are unlinkable. Never describe SP outflows as
  "invisible" — that overclaims. Never describe the absence as a gap or limitation.
- BTC-first denomination (see fourth contextual rule below). The institution declares
  a Bitcoin reserve; the display honours that.

**What civic transparency register copy never does:**
- Implies Legend verifies who a treasury belongs to
- Implies a declared set is the complete on-chain footprint of the institution
- Describes Silent Payments outflows as invisible (only recipients are unlinkable)
- Uses "audit", "certified", "official", or "verified organisation" of a self-declared view
- Uses "anonymous" — the correct word is "unlinkable" for SP recipients
- Implies the view is permanent or exhaustive — it renders what was declared at the
  addresses provided, live against the current chain

**Denomination rule for Civic Treasury View:**
BTC-first. This is the fourth contextual denomination rule (locked UC-5 Opus). A declared
institutional reserve states itself in BTC; leading in fiat undercuts the declaration the
institution is making. Sats visible, secondary. Fiat-at-time available via toggle.

### Provenance-agnosticism rule — locked UC-6 Opus · Multi-14

**Context:** UC-6 — The Hanseatic Federation. Generalises the batch-result decision
("Activity detected", never "Compromised") to all heuristic display enhancements.

Legend states on-chain facts and observable patterns. It never asserts an interpretation
of provenance or cause for an address the querying party has not declared.

"Regular settlement cadence detected on this address" is a descriptive statement of an
observable on-chain pattern — permitted. "This UTXO represents aggregated Fedimint
federation settlement" is a provenance assertion — forbidden unless the address is
*declared* by the party it describes.

When an address is *declared* by a known federation (Civic Treasury View / Federation
Settlement View, declared mode), Legend faithfully renders that declaration assertively:
the federation has named itself; Legend is rendering a declaration, not inferring.
When no declaration exists, Legend is descriptive and conditional only.

This rule also keeps settlement-pattern detection on the right side of the
third-party-analytics line: descriptive cadence display applied to an address the user
themselves queries is not surveillance. Offering the same as an API for classifying
*other people's* addresses is — and remains permanently out of scope.

**Temporal extension (locked UC-7 Opus):** The provenance-agnosticism rule applies to
time as well as cause. Legend may state the historical duration of comparable fee spikes
as fact. It may not forecast when the current spike will end, nor assert a causal
explanation ("nation-state attack", "miner coordination") from on-chain data alone.
Cause-detection at v3 produces descriptive anomaly heuristics only — never causal accusation.

### Affordability register — locked UC-7 Opus · Multi-15

**Context:** UC-7 — The Block War. Fee-shock distress. Distress Mode fires (mobile, first
visit, single address), but the distress is economic — a fee spike that may price the
user out of moving their own funds — not bereavement.

*Distress Mode is not inherently bereavement.* The bereavement and affordability registers
are two distinct registers that both activate under Distress Mode inference. A Lagos
smallholder during a 800 sat/vB spike is in distress. The copy must meet her where she is,
without the bereavement framing, and without catastrophising a fee she may choose to wait out.

**Register rules:**

- State the personal cost plainly and calmly. "Moving these funds now costs about [X]"
  is a fact. It is delivered without softening and without alarm. Neither minimise the cost
  nor dramatise it.
- "You may not be able to afford to spend this output right now" is an honest fact, not a
  failure to protect the user. State it where it is true (per-UTXO economic-dust flag).
- Never forecast relief. Comparable-spike history is history, never a prediction.
  "Comparable spikes lasted N–M hours" is a fact. "This one will clear soon" is not.
- Never recommend wait or act. Show both costs — waiting and acting — and advise neither.
  The decision is the user's. Legend hands her the arithmetic.
- Network-cost figures are USD-first. USD is the global network-cost reference; GBP is
  a UK-parochial default that fails a Lagos user. Holding figures in Distress Mode remain
  GBP-first for the bereavement context; the fee cost is a separate number with a separate
  denomination rule. See §4.
- No fee chart. A plain-language percentile sentence is the correct form. Every other
  explorer gives a fee chart built for a trader. Legend gives a sentence built for a
  person deciding whether she can afford to act.
- Do not assert a cause for the spike. Describe the pattern; refuse the diagnosis.

**What affordability register copy never does:**
- Minimises the cost — the number is the number, stated plainly
- Recommends an action (wait or act)
- Forecasts the spike clearing
- Asserts the cause is a nation-state attack or miner coordination
- Uses a fee chart, sat/vB table, or mempool-goo visualisation in primary display
- Uses technical fee jargon (sat/vB, feerate, RBF) in the headline cost figure

**Denomination rule for affordability / network-cost figures:**
USD-first. Network-cost figures are denominated in USD regardless of the active holding
denomination. USD is the near-universal global reference for Bitcoin network costs. This
does not contradict the unifying holding-denomination principle: a *cost* is not a
*holding*; the reader's relationship to a fee is as a cost-payer, and the global reference
for that cost is USD. GBP is not the default for a global product. See §4 for the full rule.

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
Legend costs roughly €673 a month to run — five dedicated servers across five jurisdictions and three continents, chosen for their legal geography, not their price. One Enterprise contract covers that cost many times over. Your free queries are not the product. They are the proof that the architecture works.

No account. No payment. No rate limit. No friction at the moment you need it most.

---

## Section 3 — Query flow copy

All strings are exact. Nothing in this section is approximate. Build sessions pull directly from this table.

### Idle state

Input placeholder: `Address, transaction ID, or block height`

Tertiary line beneath input: `Private query. No logs. No tracking.`

### Progress states (query in flight)

Four strings, exact, displayed in sequence. IBM Plex Mono. No animation beyond the string change.

```
Querying node 1 of 4…
Querying node 2 of 4…
Querying node 3 of 4…
Querying node 4 of 4…
Assembling result.
```

The final string ("Assembling result.") does not include an ellipsis. The query is complete; the assembly is local.

### Result states

**Not found — address:**
Primary: `No activity found for this address.`
Secondary: `This address has not appeared on-chain. If you're expecting a payment, it may not have confirmed yet.`

**Not found — Silent Payments:**
Primary: `This is a Silent Payments static address (BIP-352).`
Secondary: `Outputs derived from this address do not appear on-chain as this address. Legend scans every block for derived outputs.`
Tertiary: `No derived outputs found yet.`

**Error states:**
Generic: `Something went wrong retrieving that result. Try again, or check that the address or transaction ID is correct.`
Timeout: `The query took too long to complete. The nodes may be under load. Try again in a moment.`
Invalid input: `That doesn't look like a Bitcoin address, transaction ID, or block height.`

### Result display

**Primary balance line (IBM Plex Mono):**
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

Four states, session-persisted, reset on new session:

`sats` · `BTC` · `USD at time of transaction` · `GBP at time of transaction`

Toggle label (accessible): `Display denomination`

The third and fourth options are exact — "USD at time of transaction" / "GBP at time of transaction" not "USD" / "GBP" — because the fiat value at the time of a historical transaction is not the current fiat value and the distinction matters to an accountant.

**Contextual denomination rule (locked UC-2 Opus · extended UC-3/UC-4/UC-5/UC-7 Opus):**

Denomination hierarchy is contextual, not global. The toggle persists within a session, but the default denomination displayed varies by inferred context:

- **Stewardship Mode (returning owner, desktop, 1–2 addresses):** Sats-first. Arthur is the owner. He thinks in sats. GBP and BTC are visible but secondary.
- **Distress Mode (mobile, first visit, single address) — holdings:** GBP-first (bereavement register). The user is in distress. They think in pounds. Sats remain visible but secondary.
- **Distress Mode — network-cost figures (Fee Context Layer):** USD-first, regardless of the active holding denomination. A fee is a cost, not a holding. USD is the global network-cost reference. GBP is a UK-parochial default that fails a Lagos user as surely as it fails anyone outside the UK. This is the fifth contextual denomination rule (locked UC-7 Opus).
- **Recipient View / Verification context (UC-3):** GBP-first. The professional counterparty — lender, solicitor — thinks in pounds.
- **Watch Recipient View (UC-4):** GBP-first. The institutional counterparty — council officer — thinks in pounds.
- **Civic Treasury View / Federation Settlement View (UC-5/UC-6):** BTC-first. A declared institutional Bitcoin reserve states itself in BTC. Leading in fiat undercuts the declaration the institution is making. Sats secondary; fiat-at-time available via toggle. This is the fourth contextual rule — it *extends* the unifying principle rather than applying it: the institution's relationship to its own reserve is in BTC, and the public observer reads it the same way.
- **Standard / all other contexts:** Sats-first per the global default.

All hierarchies are correct for their context. The rule is not "show the denomination the user finds comforting" — it is "show the denomination that matches how this user thinks about this holding or cost in this moment." The chain data is identical in all cases.

**Unifying principle (locked UC-3 Opus · extended UC-5/UC-7 Opus):** Denomination follows the reader's relationship to the holding or cost. The owner reads holdings in sats. The non-owner professional counterparty reads holdings in fiat. The declared institutional reserve reads in BTC. Network costs read in USD — the reader's relationship to a fee is as a cost-payer, and the global reference for that cost is USD, not any single national currency.

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

*"Nothing stops this train."*
