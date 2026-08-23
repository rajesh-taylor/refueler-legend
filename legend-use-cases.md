# legend-use-cases.md — Legend Use Case Library
> **Version:** 1.2 | **Created:** Multi-9 · 11 Aug 2026 | **Updated:** UC-2 Opus · 23 Aug 2026
> Civilisational use cases for Legend. Each scenario is a design brief, an article
> candidate, and a potential UI mode. Scenarios are explored in dedicated Opus sessions —
> one per scenario, weekly or fortnightly cadence.
> Load alongside `CLAUDE.md`, `SESSIONS.md`, `MASTER.md` in any Opus use-case session.
> Companion: `legend-scope.md`, `legend-design-spec.md`.

---

## Purpose of this document

Legend is described as a privacy-first Bitcoin block explorer. That description is
accurate and undersells it completely.

What Legend actually is: infrastructure for how value gets *read and trusted* at a
human scale — from a family in Bradford to a municipal treasury in Florence. The
scenarios in this document are not marketing exercises. They are design briefs.
Each one surfaces real architectural requirements, real interface decisions, and real
copy challenges that feed directly into build sessions.

**The governing design principle across all scenarios:**

> Design backwards from the human moment. Not forwards from the technology.

Every Opus use-case session takes one scenario, one named human, one specific moment,
and asks: what does this person see? What do they need to trust? What would make them
feel confident enough to act?

---

## Cross-scenario design principles

Five principles emerged from the founding session (Multi-9). They apply to every
scenario below and every interface decision that follows from them.

**1. Multiple principals, same data.**
The whale and the council look at the same address. Legend serves both without
conflating them. The interface knows who's asking by context, not by login.
No accounts. No tracking. Context inferred from query shape, not user identity.

**2. Verification as a first-class output.**
Not just display — exportable, independently verifiable artefacts. PDFs, shareable
links, Merkle proofs. Legend produces *documents*, not just pages. A third party
receiving a Legend verification link does not need to trust Legend — they can
verify the Merkle proof themselves. "Don't trust us, verify" extends to everything
Legend produces.

**3. Time and history matter as much as current state.**
The chain is a 40,000+ block permanent record. Most explorers show you *now*.
Legend shows *then, now, and pattern*. Fee spike history. Adoption curves.
Settlement rhythms. The chain as a civilisational ledger, not a live ticker.

**4. Plain language as a technical choice, not a dumbing-down.**
Every scenario involves a person who is not a developer. The interface speaks
their language without losing precision. "UTXO" → "unspent amount." "Block height"
→ "verified at block [number] · [date]." Precision and legibility are not in
tension. They require more craft, not less.

**5. Silence is information.**
An address that hasn't moved in five years is *saying something*. Legend surfaces
that — not as an alert, as a quiet affirmation. "Held since block 840,000.
No movement detected." That is estate planning data. Loan collateral data.
Generational wealth data. Most explorers show nothing when nothing has happened.
Legend tells you that nothing has happened, and means it.

---

## Scenario index

| # | Name | Human moment | UI mode | Article candidate | Opus session |
|---|---|---|---|---|---|
| 1 | The Bradford Inheritance | Estate lookup, phone, distress | Stewardship / Distress | No | UC-1 |
| 2 | The Grandparent's Ledger | Estate planning, laptop, unhurried | Stewardship / Legacy print | No | UC-2 |
| 3 | The Bitcoin-Backed Loan | Loan verification, tablet | Verification export | No | UC-3 |
| 4 | The Council and the Whale | Treasury oversight, desktop | Treasury watch | No | UC-4 |
| 5 | The Florentine District | Civic treasury + talent payment | Civic transparency | **Yes** | UC-5 |
| 6 | The Hanseatic Federation | Merchant federation settlement | Federation settlement | **Yes** | UC-6 |
| 7 | The Block War | Nation-state fee spike, global south | Chain context / historical | **Yes** | UC-7 |
| 8 | The UTXO Lottery | Provable movement, trustless draw | Lottery eligibility | **Yes** | UC-8 |
| 9 | The Recovery Coordination Layer | Mass-compromise, 2am distress / sovereign treasury | Distress → Recovery / Watch + Verification | **Yes** | UC-9 |

---

## Scenario 1 — The Bradford Inheritance

**UC-1. Opus session: Multi-12 · 23 Aug 2026. Full spec below.**

### The moment

It is 2031. A family in Bradford has held Bitcoin since 2026. The father has died
suddenly. The youngest daughter — 24, not technical, frightened — has a piece of
paper with a hardware wallet address on it. She opens Legend on her phone. She has
never used a block explorer before. She types in the address.

What does she see?

### What Legend must do in this moment

She is in distress. She cannot afford friction. She cannot afford jargon. She needs
one answer in the first five seconds: *are the funds still there?*

Everything else is secondary. The balance. In sats, with GBP equivalent visible
without asking. A clear statement that the funds have not moved. The date of the
last transaction. Nothing technical until she asks for it.

If the funds *have* moved — if someone swept the wallet already — she needs to know
that clearly, without euphemism, and she needs to see *when* and *where* the
movement was, in plain language. "Funds were moved on [date] to an address we
cannot identify" is honest. "Transaction detected: output to 1A3B…" is not a
human sentence.

### Distress Mode — full anatomy (locked Multi-12)

**Trigger conditions (all three required):**
- Mobile viewport
- First query of session
- Single address input

Never named or selected by the user. Inferred from query shape. If any of the three
conditions is absent, Distress Mode does not engage. This is not a setting.

**Onboarding modal behaviour:** Suppressed in Distress Mode. She came to check, not
to be onboarded. The modal fires on the second visit.

**Information hierarchy — top to bottom:**

1. **Balance** — large, prominent, sats value
2. **GBP equivalent** — surfaced without tap; overrides sats default for Distress Mode only. She is in Bradford. She thinks in pounds.
3. **Held / moved statement** — one sentence, plain language:
   - Funds intact: `These funds have not moved. Held since [date].`
   - Funds moved: `Funds were moved on [date]. See below.`
4. **Plain-language history** — collapsed by default, revealed on tap. "Received [amount] on [date]." "Sent [amount] on [date]." No txids in primary view.
5. **Technical detail** — second tap. TXID, block height, confirmations. For the solicitor, not for her.
6. **Document CTA** — single tap to generate a dated summary. Appears below the history, always visible. `Save a copy for your records →`

**Reduced chrome:** No credential status icon. No denomination toggle. No batch icon. No navigation links beyond the Refueler wordmark. The page exists to answer one question.

### Batch escalation (estate pattern)

If she queries 3 or more addresses within the same session, Legend infers an estate
or breach pattern and escalates to batch results view with Distress Mode tone.
This reuses the breach architecture wholesale — no new component.

Batch results in estate context: same `Intact` / `Activity detected` values.
GBP equivalent appears in the balance column. No modification to the underlying
batch query flow — the register change is copy and visual hierarchy only.

### Age gap and device gap

| User | Device | State of mind | Mode | Primary denomination |
|------|--------|--------------|------|---------------------|
| Daughter, 24 | Mobile | Distress | Distress Mode | GBP first |
| Father, 58 (alive) | Laptop | Stewardship | Stewardship Mode | Sats first |
| Grandfather | Laptop | Curiosity | Standard view | Sats first |

Legend detects device. It does not detect age. Device is a reasonable proxy for context. The same address returns the same chain data; the information hierarchy differs by detected context.

### Solicitor document output

This is the document the daughter takes to a probate solicitor. It must hold up
in a professional context without Arthur (or the daughter's father) being present
to explain it.

| Version | Output | Cost | Who commissions | Notes |
|---------|--------|------|-----------------|-------|
| v1 | Dated summary — address, balance, last movement, verification block height, Legend query timestamp | Free | Daughter, in browser | Browser-generated. No paywall. No Merkle proof. Adequate for initial solicitor conversation. |
| v2 | Merkle-anchored + FROST-signed verified estate report | £50 | Solicitor commissions directly | Independently verifiable without Legend's involvement. Solicitor can check the Merkle proof. |
| v3 | £150 report with contextual metrics + IHT methodology aid | £150 | Solicitor commissions | Power law context, supply metrics, EO 6102 note, IHT methodology citation. |

**Bridge-document requirement (inherited from UC-2):** The v1 summary must carry full
untruncated addresses. A solicitor receiving the v1 printout may need to commission
a v2 report without the daughter present. The full address is the key. Truncation
at v1 breaks the chain of custody from v1 to v2.

### Version assignment (locked Multi-12)

- **v1:** Distress Mode, plain-language history, single-address mobile view, browser-generated dated summary with full addresses
- **v2:** Merkle-verified PDF export, FROST-signed, solicitor-legible format, solicitor commission flow
- **v3:** `/legend/verify` estate integration, IHT methodology, contextual metrics

---

## Scenario 2 — The Grandparent's Ledger

**UC-2. Opus session: UC-2 Opus · 23 Aug 2026. Full spec below.**

### The moment

Arthur is 74. He has held Bitcoin since 2020. He has two hardware wallets and a
paper backup in a solicitor's safe. He opens Legend on his laptop, unhurried,
on a Sunday afternoon, to check that everything is as he left it and to prepare
a summary for his solicitor.

He will read the same screen three times. He may print it. He wants to get it right
because this is for people he loves.

### What Legend must do in this moment

Arthur is not in distress. He is in *stewardship mode* — methodical, careful,
intending to act on what he finds. He wants:

- Confirmation that the addresses hold what he thinks they hold
- A clean summary he can print or email to his solicitor
- No jargon he has to look up
- Confidence that what he's reading is accurate and verifiable

### Stewardship Mode — full anatomy (locked UC-2 Opus)

**Trigger conditions (all required):**
- Desktop viewport
- 1–2 addresses queried in session (not the 3+ batch escalation pattern)
- No outbound activity in recent history — stillness is the signal

Never named or selected by the user. Inferred from query shape. The 3+ address
pattern triggers batch/estate escalation regardless of desktop context.

**Three-reads stillness constraint:**
Arthur will read this screen three times. The interface must accommodate that.

- No motion. No collapsing state. No timed elements.
- No auto-refresh that changes numbers between his second and third read.
- No loading animation that suggests the data might change.
- The result is stable. The chain is stable. The display is stable.

This is not an accessibility rule — it is a design principle for the stewardship
context. An interface that moves is an interface Arthur does not trust.

**Information hierarchy — top to bottom:**

1. **Stillness affirmation** — the first thing Arthur reads. Prominent. Calm.
   `Held since block [n]. No movement detected in [X] years.`
   This is the most important sentence on his screen. Most explorers show nothing
   when nothing has happened. Legend says it clearly.

2. **Aggregate summary** — total held across all entered addresses. Sats primary,
   with GBP and BTC visible without toggle. One line per denomination.
   Reuses the batch-summing logic from the breach architecture — no new component.

3. **Verification anchor** — one line. Monospace. Calm.
   `Verified at block [height] · [date] · [time]`
   This is the line Arthur quotes to his solicitor.

4. **Per-address breakdown** — one row per address. Address (full, not truncated —
   see bridge-document requirement below). Balance. Last activity date.
   The table Arthur will print. IBM Plex Mono throughout. No colour coding.

5. **Transaction history** — below the fold. Collapsed by default. Revealed on
   click. Inbound and outbound with plain-language labels. Technical detail
   (TXID, block height) available on second click.

6. **Tor notice** — below the per-address table, tertiary text.
   `This check travelled over your normal internet connection.`
   Honest. Present. Not alarming. The architecture note for users who care.

7. **Print affordance** — first-class. Single button. Top-right of summary panel.
   `Print this summary →`
   Not buried. Not secondary. Arthur came here to print something for his solicitor.
   Print is the output.

**Reuse notes:**
- Per-address balance display reuses the UC-1 history component.
- Aggregate total reuses batch-summing logic from the breach scenario architecture.
- No new components required for Stewardship Mode. The mode is register and hierarchy, not new machinery.

### Legacy print layout (locked UC-2 Opus)

A distinct CSS `@media print` stylesheet. Paper theme only — Carbon users are
silently rendered to the Paper palette for print. Carbon tokens do not print well
and the document must be legible on white paper. No user-facing notice of this;
it is a background rendering decision.

**What the stylesheet strips:**
- Refueler nav and footer
- Theme toggle
- Credential status icon
- Batch icon
- Privacy explainer trigger links
- Tor notice (present in screen view; omitted from print — the print is the output, not an explanation of how it was produced)
- Any SPA chrome

**What the stylesheet keeps:**
- Legend wordmark (small, top-left)
- Document header string (exact — see below)
- Per-address table with full untruncated addresses
- Aggregate summary
- Verification anchor (block height, date, time)
- Privacy footer string (exact — see below)
- Page break rules: one address set per page for long address lists

**Full-address requirement:**
Every Bitcoin address in the print layout renders in full, untruncated. No `bc1q...3f7a` abbreviation. The complete string. This is a chain-of-custody requirement, not a design preference. A solicitor receiving Arthur's printout may need to commission a v2 Merkle-verified report without Arthur present. The full address is the only key they have. Truncation at v1 breaks the bridge from v1 to v2.

**Document header string (exact):**
`Bitcoin holdings summary · Prepared with Legend · [date] · Block [height]`

**Privacy footer string (exact):**
`Prepared for your records. Legend keeps no copy.`

**v1 / v2 visual distinction:**
- v1 print looks like a careful personal record: clean, dated, undecorated. A document Arthur printed himself.
- v2 print looks like a commissioned verification: Merkle proof appendix, FROST signature block, verifier instructions. A document a solicitor commissioned.
- The distinction is deliberate. The v1 document is honest about what it is. It does not pretend to be a v2 document.

### Solicitor output table

The solicitor Arthur is preparing this for will receive one of three documents,
depending on which version of Legend is running and what Arthur commissions.

| Version | Document type | Cost | Who commissions | What the solicitor receives |
|---------|--------------|------|-----------------|----------------------------|
| v1 | Personal summary — browser-generated, no cryptographic verification | Free | Arthur, in browser | Dated summary with full addresses, aggregate balance, verification block height. Starting point for probate file. Not independently verifiable without re-querying Legend. |
| v2 | Merkle-anchored + FROST-signed verified estate report | £50 | Solicitor commissions directly | Independently verifiable Merkle proof. FROST signature. Can be checked against the chain without trusting Legend. Arthur does not need to be present. |
| v3 | Full verified report with contextual metrics + IHT methodology aid | £150 | Solicitor commissions | v2 report plus: power law position, supply audit, 4/8/12-year return windows, EO 6102 contextual note, IHT methodology citation. |

**Direction of commissioning note:**
v1 is commissioned by Arthur (or his daughter, or any family member with access
to the address). It flows *from* the holder *to* the solicitor as a starting
document.

v2 and v3 are commissioned *by* the solicitor — directly, from Legend's paid
report flow. This is deliberate. The solicitor is the professional who needs the
independently verifiable artefact. They commission it, they pay for it, they own
it. Arthur does not need to understand Merkle proofs.

**Bridge-document requirement:**
The v1 print must carry full address strings so that a solicitor can commission v2
without Arthur present. This is the load-bearing requirement for full addresses
in the Legacy print layout. The v1 document is the bridge from Arthur's living
instructions to the solicitor's independent verification. If the address is
truncated in v1, the bridge breaks.

### Version assignment (locked UC-2 Opus)

- **v1:** Legacy view (desktop), Stewardship Mode, silence-as-affirmation display, print layout with full addresses, browser-generated dated summary
- **v2:** Merkle-anchored + FROST-signed PDF export, solicitor commission flow, `/legend/verify` stub
- **v3:** Estate solicitor integration, IHT methodology aid, contextual metrics, `/legend/verify` full endpoint

---

## Scenario 3 — The Bitcoin-Backed Loan

**UC-3. Opus session: one scenario, one human moment.**

### The moment

James and Priya have 2.4 BTC. They want to use it as collateral for a £180,000
loan to extend their house. The lender — a Bitcoin-native lending firm — needs to
verify that the address holds what they claim, that the funds haven't moved in 90
days, and that the address is genuinely theirs (or at least that they can sign a
message from it). James opens Legend on a tablet. The lender has their own Legend
link open on a desktop across town.

### What Legend must do in this moment

Two principals. One address. Different needs.

James needs to *generate* a verification artefact — a document that proves the
balance, the holding period, and the freshness of the check.

The lender needs to *consume* that artefact independently, without trusting James
or trusting Legend — only the chain and the Merkle proof.

### Design implications

- **Verification export flow** — a distinct user journey. Not "look up an address"
  but "generate a verification." Output: a dated, Merkle-anchored summary with a
  shareable link and a downloadable PDF.
- **Lender view** — the shared link opens in a read-only, lender-legible layout.
  No Legend navigation. No query box. Just the verified data and the proof.
  "This verification was generated at block [height] on [date]. The balance shown
  has not changed since block [height − 90 days equivalent]."
- **Fiat prominence** — in this context, GBP equivalent is the primary figure.
  The denomination toggle defaults to GBP for verification exports. Sats visible
  but secondary. The lender thinks in pounds.
- **Holding period display** — explicit. "No movement in [X] days / [Y] blocks."
  Not buried. Not technical. Front and centre.

*Note: UC-2's solicitor summary and UC-3's lender verification both consume the same
Merkle-anchored v2 export artefact from opposite sides — the solicitor receives it
from the estate; the lender receives it from the borrower. The artefact is the same.
The commissioning direction differs.*

### Version assignment (preliminary)

- v1: verification export flow, shareable link, holding period display
- v2: Merkle proof download, PDF with chain verification instructions
- v3: `/legend/verify` endpoint, lender API, ZK balance proof

---

## Scenario 4 — The Council and the Whale

**UC-4. Opus session: one scenario, one human moment.**

### The moment

A county council in the West Midlands has accepted Bitcoin as collateral security
from a property developer — a Bitcoin whale — for a £40 million housing development.
The whale needs to demonstrate his holdings are unencumbered. The council needs
ongoing assurance that the collateral hasn't been moved. Neither party trusts the
other completely. Both need a trusted third party that isn't a bank.

Legend is that third party. Except Legend doesn't hold anything, store anything,
or know who either party is.

### What Legend must do in this moment

The whale opens Legend. He adds the collateral address to a *watch*. Legend
generates a FROST-signed watch attestation — a statement that at this block height,
this address held this amount, and any movement will be detectable. The watch
link is shared with the council.

The council's officer opens the watch link on a Monday morning. She sees: funds
intact, last verification block height, canary status (is Legend itself operational
and uncompromised), date of next scheduled check.

Neither party has revealed anything about themselves to Legend. The chain is the
witness.

### Design implications

- **Watch mode** — a persistent (session-based, not account-based) monitoring view.
  An address is "watched" for a session. Share the session link. The recipient
  sees the current state without needing a Legend account.
- **Treasury dashboard** — council view. Single address or address set. Balance,
  movement status, canary status, verification timestamp. Clean enough for a council
  officer who has no Bitcoin background. The legend canary status is explained in
  one sentence: "Legend's verification service is operating normally and has not
  been legally compromised."
- **Institutional language** — "collateral address" not "wallet." "Verified balance"
  not "UTXO set." "Movement alert" not "transaction detected."
- **FROST attestation as document** — the watch attestation is a signed statement,
  not a live feed. It can be filed. It can be cited. It has a timestamp and a
  block height and a cryptographic signature.

### Version assignment (preliminary)

- v1: watch mode, shareable link, institutional language layer
- v2: FROST-signed attestation download, automated movement alerts
- v3: council API, multi-signatory watch (council + developer + solicitor)

---

## Scenario 5 — The Florentine District

**UC-5. Opus session: one scenario, one human moment.**
**Dual output: UI mode + Article candidate.**

### The historical resonance

Florence, 1450s. The Medici don't just bank — they *attract*. Brunelleschi,
Michelangelo, Leonardo, Botticelli. The city becomes the most valuable square mile
in Europe not because of its territory but because of who lives there. Value accrues
to the jurisdiction that earns the right people.

2033. A canton in Switzerland, a city-state in the Gulf, a special economic zone
in East Africa each declare Bitcoin treasuries and begin paying residency stipends
in Bitcoin to attract cryptographers, architects, artists, engineers. The Florentine
model, on-chain.

### The moment

A cryptographer in Edinburgh receives an offer from a Swiss canton. A monthly
stipend, paid in Bitcoin, from the canton's declared treasury address, via Silent
Payments. She opens Legend to verify the canton's treasury is real, funded, and
has been paying others consistently.

The canton's treasury officer opens Legend to publish the monthly payroll outflow —
publicly verifiable, individually private.

### What Legend must do

**For the cryptographer:** verify the canton's treasury without revealing her own
address or her interest in the offer. She should be able to see outflow patterns —
"this treasury has made 47 outflows in 12 months, consistent with a stipend
programme" — without seeing who received them. Silent Payments means she cannot.
That is correct behaviour.

**For the canton:** publish a civic treasury view. A public-facing display of
declared inflows (Bitcoin tax receipts, reserve deposits) and outflows (declared
expenditures, stipend programmes). Not every transaction — the declared ones,
voluntarily published, verifiably on-chain.

**For observers:** see the aggregate without seeing the individuals.
"This jurisdiction operates a Bitcoin treasury. Here is its declared activity."

### Design implications

- **Civic treasury mode** — a distinct UI mode. A public, read-only view of a
  declared address set. Canton/council/municipality branding optional (they provide
  a name, Legend verifies nothing about the identity — only the chain data).
  Aggregate inflow/outflow by month. No individual transaction details in primary
  display.
- **Silent Payments integrity** — the display explicitly states: "Outflows use
  Silent Payments. Individual recipients are not visible on-chain by design.
  This is a privacy feature, not a gap."
- **The Florentine UI mode** — named internally as such. The mode that makes
  institutional Bitcoin treasury activity legible without making individual
  participants visible.

### Article: "The Florentine Protocol"

*Opening:* The Medici didn't just hold gold — they attracted the people who made
gold worth holding. Here's what that looks like on a blockchain.

*Argument:* Jurisdictions that can credibly demonstrate a Bitcoin treasury and pay
in Bitcoin will attract talent that compounds in value. Legend is the tool that
makes the treasury verifiable without making the recipients visible. Silent Payments
is the cryptographic equivalent of a Medici stipend envelope — you know money went
out; you don't know who received it.

*Closing:* The city-state is back. It runs on Lightning.

### Version assignment (preliminary)

- v1: civic treasury address view, aggregate display, SP integrity statement
- v2: voluntary treasury publication flow, monthly summary export
- v3: multi-signatory treasury (council + auditor), ZK aggregate proofs

---

## Scenario 6 — The Hanseatic Federation

**UC-6. Opus session: one scenario, one human moment.**
**Dual output: UI mode + Article candidate.**

### The historical resonance

The Hanseatic League, 1358–1669. A network of merchant cities — Hamburg, Lübeck,
Bruges, London, Riga, Tallinn — operating without a central authority, bound by
mutual credit and shared commercial law. A merchant in Hamburg ships wool to Bruges.
He carries a letter of credit, not gold. The Bruges merchant honours it because
they share membership of something larger than either of them.

The League ran for 300 years. It collapsed when nation-states got powerful enough
to override it. The Bitcoin version is resistant to that — because the settlement
is on-chain and the routing is Lightning.

### The moment

2031. A network of Bitcoin-native merchants across European cities run Fedimints.
A leather goods maker in Hamburg, a linen merchant in Bruges, a spice importer in
Valletta. They accept Lightning. They trust each other's Fedimint attestations.
A customer in Riga buys from Hamburg. The payment routes through Fedimints,
settles on-chain once a week in a single aggregated UTXO.

Legend shows the settlement UTXO. But with context — because context is everything.

### What Legend must do

Show the on-chain UTXO and explain what it represents — not who made the
individual payments, but what kind of activity this UTXO aggregates.

"This UTXO represents aggregated weekly settlement across a merchant federation.
Individual transactions are not visible on-chain by design. This is correct
behaviour — not a gap in the data."

The federation treasurer opens Legend to verify settlement. She sees the UTXO,
the block height, the date, the amount. She does not see the individual
transactions that contributed to it. She does not need to. The Fedimint handled
that. Legend shows the on-chain truth.

### Design implications

- **Federation settlement view** — a distinct display mode for UTXOs that match
  federation settlement patterns (high value, low frequency, Fedimint provenance
  heuristic). Not a new query type — a display enhancement on the standard UTXO
  result.
- **Absence explanation** — the display explicitly states why individual
  transactions are not visible. This is not a bug. The copy explains it plainly.
- **Historical pattern display** — weekly settlement rhythm visible in transaction
  history. "Settlement: [amount] on [date]. Settlement: [amount] on [date]."
  Pattern is the information. Single transactions are not.

### Article: "The Hanseatic Protocol"

*Opening:* The Hanseatic League ran without a central bank, a central government,
or a shared currency for 300 years. Here's what the 2031 version looks like.

*Argument:* Merchant federations that settle in Bitcoin are rediscovering the
Hanseatic model — mutual credit, shared commercial law, no sovereign anchor.
Legend shows the on-chain settlement without revealing the individual trades.
The absence of individual transaction visibility is the feature, not the bug.

*Closing:* The League collapsed when nation-states got strong enough to override it.
The Bitcoin version doesn't have that problem.

### Version assignment (preliminary)

- v1: federation UTXO display, absence explanation, settlement pattern display
- v2: Fedimint attestation integration, weekly summary export
- v3: multi-federation aggregation, cross-Fedimint provenance chain

---

## Scenario 7 — The Block War

**UC-7. Opus session: one scenario, one human moment.**
**Article candidate.**

### The moment

2029. A nation-state actor — unknown jurisdiction, suspected coordination with
a major mining pool — begins systematically flooding the mempool during business
hours in targeted time zones. Fee rates spike to 800 sat/vB for twelve hours.
A smallholder in Lagos has 0.003 BTC in a non-custodial wallet. She needs to move
funds. The fee to consolidate her UTXOs exceeds the value of her smallest one.

She opens Legend. She needs to understand what is happening — not technically,
not politically, but practically. Is this normal? Has it happened before? How long
does it last?

### What Legend must do

Show her what the chain says about this fee spike — historically, contextually,
and practically.

- Current fee rate vs 30-day median vs 12-month distribution
- Duration of past comparable spikes
- "At the current fee rate, a simple transaction costs approximately [X] sats
  ([Y] GBP). Your smallest unspent amount is [Z] sats."
- Honest human cost: if she waits, the spike may clear. If she acts, it costs her.
  Legend shows both. It does not recommend.

### Design implications

- **Fee context layer** — a secondary display mode that activates when fee rates
  are significantly above baseline. Historical fee distribution in plain text, not
  charting. "The current fee rate is higher than 94% of days in the past 12 months."
- **Human cost calculator** — sats and local fiat equivalent. Honest. No
  recommendation. The human decides.
- **Lightning correlation panel** — does she have Lightning options? If she's
  querying an on-chain address, she may not. But the display notes whether the
  address has had Lightning channel activity.
- **Historical fee context** — block list with fee rate annotation for the past
  1,000 blocks. Plain text. No animation.

### Article: "Who Gets Priced Out"

*Opening:* The Bitcoin network has no concept of fairness. The mempool doesn't
know where you live or how much you have. Here's what a fee spike looks like from
Lagos.

*Argument:* High fee events are not neutral. They price out small holders in
high-inflation, low-income contexts first. Legend shows the human cost — not the
technical cost. The fee rate is a number. The human cost is a fraction of someone's
savings.

*Closing:* The chain doesn't care who gets priced out. The tools we build around
it can.

### Version assignment (preliminary)

- v1: fee context layer, human cost calculator, historical fee data display
- v2: Lightning correlation, UTXO consolidation cost projection
- v3: mempool fee attack detection heuristics, historical comparison tooling

---

## Scenario 8 — The UTXO Lottery

**UC-8. Opus session: one scenario, one human moment.**
**Hybrid: UX design session + cryptographic design session. Article candidate.**

### The moment

A community fund in a small town in El Salvador runs a monthly UTXO lottery.
Every sat that moved through the town's Bitcoin circular economy in the past month
is eligible. The winner is determined by a deterministic function applied to the
current block hash — provably random, publicly verifiable, impossible to rig.

The lottery organiser opens Legend. She needs to prove to every participant that
the draw was fair.

### What Legend must do

Provide a trustless, independently verifiable draw function. No trusted third party.
No organiser can influence the result. Anyone with Legend can verify it.

- Block hash as entropy source — the lottery closes at a specified block, the
  winning output is determined by `SHA256(block_hash || lottery_seed)` modulo
  the eligible UTXO count. Organiser cannot predict the block hash.
- Eligible UTXO set — verifiable by anyone. The set is defined by the organiser
  (address set + date range) and independently reconstructable from the chain.
- Merkle lineage proof — the winning UTXO's inclusion in the eligible set is
  Merkle-provable. The proof is downloadable.
- Miner manipulation economics — the prize must be small enough that no rational
  miner would withhold a block to influence the result. Legend calculates and
  displays this threshold. Honest scope.

### Design implications

- **Lottery eligibility view** — a new query type: "eligible UTXOs at block [N]
  within address set [S] and date range [D]." Returns the eligible set and the
  draw result.
- **Verifier interface** — anyone can paste the lottery parameters and verify the
  draw independently. Legend is not the authority. The chain is.
- **Miner manipulation threshold** — plain-language display. "At current block
  rewards, a miner would need to withhold [N] blocks to guarantee a win. The
  expected lottery prize must exceed [X] BTC for this to be rational. The current
  prize is [Y] BTC."

### Article: "The UTXO Lottery"

*Opening:* The oldest objection to Bitcoin lotteries is that the organiser could
cheat. Here's how the chain makes that impossible.

*Argument:* A deterministic function applied to a future block hash, applied to
a publicly verifiable UTXO set, produces a lottery that no participant or
organiser can influence. Legend is the tool that makes this verifiable to anyone.
The catch: the prize must be small enough that no miner would bother.

*Closing:* A lottery that anyone can verify is not a lottery. It's an institution.

### Version assignment (preliminary)

- v1: eligible UTXO set query, deterministic draw function, block hash entropy
- v2: Merkle lineage proof download, verifier interface
- v3: miner manipulation threshold calculator, multi-round lottery coordination

*Note: Samourai Whirlpool flagged for UC-8 session — 5,651 BTC unspent capacity,
86,844 UTXOs.*

---

## Scenario 9 — The Recovery Coordination Layer

**UC-9. Opus session: UC-9 Opus · 23 Aug 2026. Full spec below.**

### Two tracks, one architecture

**Marco** — individual victim. A mass-compromise event (hardware wallet batch
vulnerability, AI-computed RNG pattern, coordinated sweep). 2am. Savings gone
or at risk. Distress Mode as entry point.

**Elena** — sovereign treasury officer, El Salvador national Bitcoin reserve.
A fiscal event — national treasury address under state-level threat. Watch and
Verification mode as entry point. Never enters Recovery Mode.

These are not the same scenario at two scales. Marco's emotional and legal position
is entirely different from Elena's institutional one. The primitives generalise;
the interface does not.

### The Coldcard Mk3 attacker model (August 2026)

~1,200 BTC swept in a coordinated no-dust attack. Vulnerable UTXO set identified
by AI-computed RNG pattern across the production batch — five years after device
shipping. No test transactions. No dust. Single coordinated sweep window.

Design consequence: the "dust then sweep" early-warning model is not the primary
attacker profile for UC-9. Pass notification may arrive *after* the sweep, not
before. The copy must be honest about this.

### FROST decomposition (three-layer model)

Recovery does not mean "victims co-sign." The honest architecture has three layers:

1. **Blind membership credential** (Pass Access-class) — asynchronous, anonymous.
   Does not encode the group ID in a redemption-linkable way. Attests: "entitled
   to participate in a Legend-coordinated recovery." Binding to specific recovery
   happens via the sealed component, submitted separately.

2. **Sealed individual component** — each victim's claim encrypted to the trustee
   quorum; submitted via Share; no victim sees another's data.

3. **Mixed attestation quorum** — Legend + trustee + legal representative, 3-of-4
   FROST. Signs the aggregate envelope only. No single party can forge or block.
   Addresses the single-operator IPA compulsion gap: a UK order on Legend's
   operator does not lapse the quorum.

### UI mode summary

| Mode | Trigger | Track |
|---|---|---|
| Distress Mode | Single address query, mobile, first visit; or 3+ addresses in session | Both (entry point) |
| Recovery Mode | Explicit opt-in after Distress Mode answer resolves; disclosed batch detected | Marco only |
| Watch/Verification | Declared address set, institutional context, desktop | Elena |

Recovery Mode is never triggered automatically.
Distress Mode is read-only. Recovery Mode is a legal and social action.
The mode boundary is the consent moment.
Elena never enters Recovery Mode.

### Pass — coordination credential architecture

Pass holds the coordination credential as an Access-class token (non-monetary,
closed-loop). Fresh independent mint instance per the standard rotation rule.
NUT-12 DLEQ mandatory (detects mint tagging attacks — standard requirement).

**Hard constraint:** the credential must not encode the group ID in a
redemption-linkable way. Attests: "entitled to participate in a
Legend-coordinated recovery." The binding to a specific recovery happens
via the sealed component, submitted separately. Group membership and amount
remain invisible on redemption.

Pass also carries the early-warning job from the existing beats:
quiet dust-transaction alert, no address, no amount, no corporate-IT log
signal. In the no-dust attacker model (Coldcard Mk3 pattern), this alert
may arrive after the sweep rather than before it. The copy must be honest
about this: Pass tells you what happened as soon as the chain records it.
It cannot warn you before a coordinated sweep that leaves no test signal.

---

### Lawyer sequencing — honest timeline

No lawyers required before v2. No lawyers required before v3 ships.

Before v3 launch:
- One forensic Bitcoin specialist solicitor reviews the sealed-component and
  attestation format — confirming it is receivable in a UK or EU insolvency
  context. One session, one memo. This extends the existing Opus-C gate.
  Firms: Mishcon de Reya (digital assets team), AWO (privacy-adjacent,
  already on the shortlist), Pinsent Masons (Cryptopia/Bittylicious creditor
  experience).
- One civil-law jurisdiction equivalent (most likely German or French insolvency
  practitioner, covering EU civil law). Two relationships, not fifty.
- Format published openly before either relationship is confirmed — the format
  is the durable artefact.

Legend's copy at v3 launch names the class of eligible trustee
(UK-registered insolvency practitioner, or equivalent) — not a specific firm.
The victim engages their own counsel. Legend provides the machinery.

---

### Version assignment

**v1 (prerequisite — must ship and prove before any recovery claim is honest):**
Legend's own FROST 3-of-4 for canary and mint key management. Blind-credential
issue/redeem path (query credentials). Distress Mode (UC-1 core). The chain trace
foundation. Without these in production, the Recovery Coordination Layer cannot
be built honestly on top.

**v2:**
Chain Trace Report — Merkle-verified, solicitor-legible PDF export.
Share encrypted transport in production.
Pass Access credential in production.
Address Watch + Pass notification (with honest copy re: no-dust attacker model).
Article 24 phase-one beats publish at v2 (Marco/Distress/ChainTrace/Share beats).

**v3 honest minimum — three gates, not one:**
1. Sealed-component format built and tested (crypto — the easy part).
2. Mixed attestation quorum operational (Legend + trustee + legal representative,
   3-of-4 FROST). Requires the two-operator milestone and Opus-C solicitor
   sign-off — these are the load-bearing gates, not the cryptography.
3. At least one real trustee/jurisdiction relationship confirmed. Without a
   real trustee anchor, offering "collective recovery" cannot be delivered.

**Honest copy at v3 launch before gate 3 clears:**
*"Coordination format published. Operational trustee relationships pending.
Check refueler.io/legend/recovery for current status."*

Article 24 sovereign/Recovery Coordination Layer beats publish at v3 unlock.

---

*"Chainalysis works for the observer. Legend works for the owner."*

---

## Opus session structure

Each use case runs as a dedicated Opus session. One session per scenario.
Weekly or fortnightly cadence. Each session takes the scenario brief above
as its primary input alongside `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`.

### Session format

**Load:** `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`
**Scope:** one scenario, fully interrogated.

**Questions each Opus session must answer:**

1. What does the named human see in the first five seconds?
2. What information hierarchy serves their moment (distress / stewardship /
   verification / oversight)?
3. What does Legend need to show that no other explorer shows?
4. What does Legend need to *not* show — and how does it explain the absence?
5. What copy principles from `legend-ux-language.md` apply here, and which
   need extending?
6. What are the version assignments — what ships in v1, what waits for v2/v3?
7. Does this scenario generate a new UI mode? If so, name it and spec its
   trigger conditions.
8. Is this an article? If so, what is the opening line?

### Output from each Opus session

- Interface spec for the scenario's UI mode (feeds `legend-design-spec.md`)
- Copy additions for the scenario's language register (feeds `legend-ux-language.md`)
- Version assignments (feeds `legend-scope.md`)
- Article outline if applicable (feeds `legend-articles-list.md`)
- Carry-forward items for `SESSIONS.md`

---

## The B9 gate — lifted

**Decision logged Multi-9 (11 Aug 2026):** The B9 gate (Share Lightning node live)
is no longer a prerequisite for Legend build sessions. Legend will run its own
independent Lightning node when needed. Legend v1 has no Lightning dependency.
The Cashu mint for query credentials is non-monetary and requires no Lightning node.

Build sequence from Multi-9:

1. **Adversarial Opus session** — v1 transport and query architecture threat model.
   What can an unlimited-budget attacker extract from the v1 design? Do any current
   decisions foreclose better v2 transport options? Output: `legend-threat-model.md`
   or equivalent. May generate a hardening work block.

2. **Legend-6 planning Opus** — Eleventy shell, SPA architecture, Silent Payments
   display planning. Feeds directly into Legend-6 build.

3. **Legend-6 build** — Eleventy shell live at `refueler.io/legend`. SPA mount,
   Paper/Carbon theme, query input idle state.

4. **Article drafting block** — no publish pressure. 2–3 weeks to draft Articles
   A, B, C (queued in `legend-articles-list.md`) plus Article 14 and 15 outlines.

5. **Use-case Opus sessions** — UC-1 through UC-9, weekly/fortnightly cadence,
   running in parallel with build sessions.

---

## On the scope of Legend

A block explorer was the starting description. It remains accurate.

It undersells what Legend actually is: infrastructure for how value gets read and
trusted across every scale of human economic life — from a daughter in Bradford
checking her father's wallet on a phone, to a canton in Switzerland publishing a
civic treasury, to a merchant federation settling a week of Lightning payments in
a single on-chain UTXO, to a national treasury officer in San Salvador generating
a court-ready loss attestation at 3am.

The chain doesn't know any of these humans. Legend doesn't either.
That is the point.

*"Chainalysis works for the observer. Legend works for the owner."*

---

*"Nothing stops this train."*
