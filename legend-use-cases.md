# legend-use-cases.md — Legend Use Case Library
> **Version:** 1.1 | **Created:** Multi-9 · 11 Aug 2026 | **Updated:** UC-9 Opus · 23 Aug 2026
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

**UC-1. Opus session: one scenario, one human moment.**

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

### Design implications

- **Distress mode** — a distinct visual state triggered by the query context (first
  visit, single address, mobile). Reduced chrome. Maximum signal. No onboarding.
  No "here's how Legend works" modal.
- **Batch escalation** — if she queries 3+ addresses from the same session, Legend
  recognises a breach/estate pattern and shifts register further. Faster. Quieter.
  More focused.
- **Plain-language movement history** — no txids in primary display. "Received
  ₿X on [date]" and "Sent ₿X on [date]" in chronological order, most recent first.
  Technical detail available on tap, never default.
- **The print / share moment** — she will want to show this to a solicitor.
  A single tap produces a clean, dated, Merkle-verified summary. Not a screenshot.
  A document.

### Age gap and device gap

The daughter is on a phone. Her grandfather — the person who set up the wallet —
would have used a laptop. The same address, viewed by two different people in two
different states of mind, should produce two different information hierarchies from
the same underlying data. Legend detects device. It does not detect age. But
device is a reasonable proxy for context.

### Version assignment (preliminary — confirm in UC-1 Opus session)

- v1: distress mode, plain-language history, single-address mobile view
- v2: Merkle-verified PDF export, solicitor-legible format
- v3: `/legend/verify` estate integration, IHT methodology

---

## Scenario 2 — The Grandparent's Ledger

**UC-2. Opus session: one scenario, one human moment.**

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

### Design implications

- **Legacy view** — desktop, full width used for *explanation* not for more data.
  Each technical term gets a quiet inline definition. "UTXO" never appears.
  "Unspent amount" does.
- **Stewardship summary** — not a transaction list. A summary: total held across
  all entered addresses, date of last movement, verification timestamp, chain height.
  Then the detail, below the fold, for those who want it.
- **Print layout** — a distinct CSS print stylesheet. Clean, dated, no navigation,
  no Legend chrome. Looks like a formal document because it *is* one.
- **Silence as affirmation** — "Held since block 840,000. No movement detected in
  5 years." This is the most important sentence on Arthur's screen. Most explorers
  show nothing when nothing has happened. Legend says it clearly.

### Version assignment (preliminary)

- v1: legacy view, stewardship summary, print layout
- v2: PDF export with Merkle verification, solicitor-legible format
- v3: estate solicitor integration, `/legend/verify`, IHT calculation aid

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
behaviour — Lightning routing and Fedimint privacy are working as intended."

That sentence is Legend doing something no other explorer does: *explaining what
you're not seeing, and why that's correct behaviour, not a gap.*

### Design implications

- **Federation settlement view** — a display mode for UTXOs that appear to
  represent aggregated off-chain activity. Heuristic detection (large, regular,
  round-number inputs from known Fedimint patterns). Display: aggregate amount,
  estimated federation size (if detectable), settlement rhythm.
- **The "absence is correct" principle** — a design pattern that applies across
  Legend wherever privacy-preserving technology produces invisible-but-real
  activity. Display the signal. Explain the silence.

### Article: "The Hanseatic Protocol"

*Opening:* The Hanseatic League ran without a central bank for 300 years.
Here is what its Bitcoin successor looks like, and how Legend reads its ledger.

*Argument:* Fedimint + Lightning recreates the Hanseatic mutual credit model
at global scale. The on-chain UTXO is the bill of lading — proof of settlement,
not proof of every individual trade. Legend's role is the port ledger: what
arrived, what departed, what the aggregate represents. Not surveillance — literacy.

*Closing:* The League fell to nation-states. The protocol doesn't.

### Version assignment (preliminary)

- v1: federation settlement heuristic, "absence is correct" display pattern
- v2: Fedimint health check integration (reserve check, blind)
- v3: cross-federation aggregation, Liquid settlement detection

---

## Scenario 7 — The Block War

**UC-7. Opus session: one scenario, one human moment.**
**Article candidate.**

### The moment

2028. A nation-state — or a well-resourced actor operating at state scale —
deliberately floods the mempool. Fees spike to 800 sat/vbyte. A small business
owner in Lagos needs to send ₿0.00005 — roughly £4 — to pay a supplier.
The transaction fee is £12. She cannot afford to transact on-chain.

Bitcoin's promise to the global south — financial inclusion without permission —
is under active attack. Not a hack. Not a technical failure. A deliberate
economic exclusion, conducted by filling blocks with dust.

### What Legend must do

**Show the human cost, not just the technical metric.**

Not "current fee rate: 800 sat/vbyte." Instead:

*"At current fees, sending ₿0.00005 (≈ £4) costs £12 in fees — three times the
transaction value. Small transactions are currently uneconomical on-chain.
Lightning channels remain available."*

That is a moral statement rendered as data. Not editorialising. Not political.
Just showing what is true, in human terms.

**Show historical pattern.**

The chain is a permanent record of every fee environment Bitcoin has ever
experienced. Legend provides context:

*"The last time fees reached this level was [date]. Over the following 30 days,
[X]% of transactions migrated to Lightning. Channel opens spiked [Y]% in the
first week."*

Not prediction. Historical pattern, clearly labelled. One-line contextual
note beneath the fee display. Expands on tap. Never pushed.

**Show Lightning correlation — from on-chain signals only.**

Legend has no Lightning node. But channel open/close activity is on-chain.
Fee spikes correlate with channel open surges — people fleeing to Lightning.
Legend shows this correlation without requiring off-chain data.

"Lightning network activity indicator — derived from on-chain channel signals."
Honest about the methodology. Useful despite the limitation.

### Design implications

- **Human cost calculator** — a persistent element in the mempool/fee section.
  Shows the fee cost of sending a small, medium, and large transaction in human
  terms (GBP and local currency if detectable). Updates in real time.
- **Historical fee context** — a quiet contextual note. Not a chart. A sentence
  with an expand option. Pattern recognition, not prediction.
- **Lightning correlation panel** — on-chain derived. Honest methodology label.
  Shows channel activity rhythm relative to fee environment.
- **The global south frame** — Legend's fee display is designed for the person
  who cannot afford to get this wrong, not for the node operator who is
  curious about market conditions.

### Article: "Who Gets Priced Out"

*Opening:* When fees hit 800 sat/vbyte, Bitcoin works fine for whales.
Here is what it looks like for everyone else.

*Argument:* Block space is a commons. When it is weaponised — deliberately or
incidentally — the smallest users pay the highest relative cost. Legend shows
this not as a political statement but as arithmetic. The chain doesn't lie.

*Closing:* Financial inclusion without permission is the promise. The mempool
is where that promise is either kept or broken.

### Version assignment (preliminary)

- v1: human cost calculator, fee display in plain language
- v2: historical fee context layer, Lightning correlation panel
- v3: multi-jurisdiction fee impact (currency-adjusted human cost)

---

## Scenario 8 — The UTXO Lottery

**UC-8. Opus session: one scenario, one cryptographic design session.**
**Article candidate.**

### The concept

For Bitcoin to survive and grow it must be used as money. Velocity matters.
A UTXO that has moved many times is demonstrating Bitcoin's monetary properties
in action — it is being used, not hoarded.

The UTXO Lottery is a trustless, on-chain lottery that rewards movement.
No operator. No custodian. No trust required beyond the chain itself.

### The mechanism (sketch — to be developed in UC-8 Opus session)

1. **Eligibility criterion** — a UTXO qualifies for the lottery if it has moved
   at least N times (e.g. 10) within a defined block range. Movement history is
   provable via Merkle inclusion — the chain records every input/output.

2. **Draw mechanism** — the block hash at a predetermined future block height
   is the entropy source. Nobody controls it. Miners could theoretically
   manipulate it but the cost of doing so for a lottery prize would typically
   exceed the prize itself (merge-mining economics argument — to be stress-tested
   in Opus session).

3. **Winner determination** — the eligible UTXO whose hash, combined with the
   draw block hash via a deterministic function, produces the lowest (or highest)
   value wins. Fully verifiable by anyone with access to the chain.

4. **Prize** — could be a pre-funded prize UTXO, locked with a script that
   releases to the winner address on proof of eligibility. Or simply social
   recognition — the chain as the scoreboard.

5. **Legend's role** — display. Legend shows a UTXO's lottery eligibility status:
   how many times it has moved, current standing, draw block height, odds
   (estimated from eligible UTXO count). Legend is the verification layer,
   not the lottery operator. It cannot manipulate the draw — the draw is the
   chain.

### Cryptographic sketch (to be developed)

- **Movement proof** — Merkle path from each spend transaction to its block header,
  chained across N hops. Proves the UTXO lineage without requiring Legend to store
  any data beyond what the chain provides.
- **Entropy source** — block hash. NIST randomness beacon could supplement if
  chain-only entropy is insufficient (to be assessed in Opus session).
- **Winner verification** — deterministic function (e.g. BLAKE3 of UTXO outpoint
  concatenated with draw block hash). Lowest/highest output wins. Any node can
  verify. Legend displays the verification, doesn't perform it.

### Design implications

- **Lottery eligibility view** — a distinct display element on any UTXO detail page.
  Movement count, eligibility status, draw block countdown, estimated odds.
  Quiet by default. Prominent if eligible.
- **Leaderboard** — a public, chain-derived display of current top-movement UTXOs.
  No user data. Pure chain data. Sorted by movement count. Legend's version of a
  scoreboard that requires no operator.

### Why this matters beyond the lottery

The UTXO Lottery demonstrates something important about Legend's architecture:
Legend can compute non-trivial chain analytics — movement history, Merkle lineage,
eligibility scoring — without storing query data or knowing who is asking.
The lottery is a proof of concept for provable chain-derived facts that serve
human purposes (incentivising velocity) using only what the chain already contains.

### Article: "The UTXO Lottery"

*Opening:* Bitcoin's longest debate is whether it is money or gold. Here is a
lottery that only works if it is money.

*Argument:* Movement velocity is a monetary property. A UTXO that has moved
ten times in a year is being used as money. One that has sat since 2015 is being
stored. Both are legitimate. The lottery doesn't judge — it rewards movement,
because movement is the thing worth rewarding if Bitcoin is to be used, not
just held.

*Closing:* The draw block is coming. The chain picks the winner.
Nobody else does.

### Version assignment (preliminary)

- v1: movement count display on UTXO detail
- v2: eligibility scoring, draw block display, Merkle lineage proof
- v3: lottery verification endpoint, leaderboard, prize script integration

---

## Scenario 9 — The Recovery Coordination Layer

**UC-9. Opus session: two parallel human tracks, one architecture.**
**Article candidate — Article 24.**

---

### Why this scenario exists

On a Tuesday morning in August 2026, approximately 1,200 Bitcoin were swept from
Coldcard Mk3 users in a coordinated, no-dust attack. The vulnerable UTXO set was
identified not by probing individual addresses but by computing the predictable output
of a flawed RNG across the affected production batch — a calculation performed by a
powerful language model five years after the devices shipped. There were no test
transactions. No dust amounts. Funds moved directly to fresh addresses in a single
coordinated sweep window.

The victims had no warning before the sweep. Most learned of the loss through a
disclosure alert, a news article, or by checking a public explorer.

Every tool they needed had existed in cryptographic literature for years.
None had been built for a human in distress.

UC-9 is the design brief for building those tools.

---

### The governing principle for this scenario

The Bradford daughter (UC-1) showed us what Distress Mode must do for an individual
in a calm loss event — the death of a father, a piece of paper with an address.
UC-9 is Distress Mode under adversarial conditions: a coordinated attack, a disclosed
batch, 2am, funds possibly already gone, and — if they are gone — the question of
whether anything can be recovered through collective legal action.

The governing design principle does not change:

> Design backwards from the human moment. Not forwards from the technology.

What changes is that there are now two distinct human moments, at different scales
of the same architecture, and they must be developed as parallel tracks — not merged.

---

### The attacker model for this scenario (UC-9-specific)

The no-dust sweep is the load-bearing assumption. Do not design UC-9 around the
"dust then sweep" model — that model assumes an attacker who probes before acting.
A sophisticated attacker with AI-assisted victim enumeration does not probe.
They compute the complete victim list from the flaw, then sweep all affected addresses
in a coordinated window.

**Design consequence:** Pass notification of a dust transaction is not Marco's first
warning in this scenario. It may be his only warning *after* the fact. The first
five seconds must work under both conditions:

- Funds intact (he is on the list but not yet swept)
- Funds moved (he was swept, silently, without a prior dust signal)

Both must be answered in plain language, without euphemism, in the first five seconds.
The rest of the interface follows from which answer he gets.

---

### Track A — Marco. Individual victim. Mass-compromise event.

#### The moment

It is some future year. A hardware wallet manufacturer has disclosed a supply chain
compromise: the entropy used to generate seed phrases in a specific production batch
was predictable. Marco's batch number is on the list. He does not know if his specific
wallet was affected. He does not know if his funds have already moved.

It is 2am. He opens Legend on his phone.

#### First five seconds

One answer. No jargon. No onboarding. No modal.

Either:

*"Funds intact. Last moved [date] — [X] days ago. No movement since."*

Or:

*"Funds were moved on [date] to an address we cannot identify. We can show you
the full chain trace."*

Both are honest. Neither contains a technical term. Both resolve his first question
before anything else loads.

If his funds are intact, the next sentence — offered quietly, not pushed — is:
*"Your batch number appears in the disclosed list. Your funds are unaffected so far."*

If his funds have moved, the next sentence is:
*"This appears to be part of the disclosed batch compromise. Here is exactly what
the chain recorded."*

#### The ten-minute arc

**Minutes 0–2:** Distress Mode answer for each address he holds (3+ address pattern
triggers escalation from UC-1 — batch/breach recognition, quieter chrome,
faster focus).

**Minutes 2–5:** Chain Trace display. Every hop, in plain language, most recent first.
"Received [amount] on [date]. Sent to unknown address on [date]."
Technical detail (txid, block height) available on tap, never default.

**Minutes 5–7:** "Send this to your solicitor privately" — Share button, encrypted
link, 72-hour expiry, recipient needs no account. The Chain Trace Report is the
document: Merkle-verified, block-height-stamped, solicitor-legible.

**Minutes 7–10:** Recovery Mode door — offered once, quietly, after the distress
answer and the trace, never before.

*"This appears to relate to a disclosed batch compromise. If you want to add your
claim to a shared legal filing — where others cannot see your amount and you cannot
see theirs — here is how that works."*

That sentence contains no cryptographic vocabulary. The explanation that follows
uses two plain sentences:

*"Each person's claim is sealed separately. A court-appointed trustee can open each
one only through proper legal process. Nobody else can — not other claimants, not us."*

The door. Not a push. Not a modal. One tap to learn more, one tap to proceed,
one tap to decline. Marco is in distress. He must be able to say no and have that
respected immediately.

#### Recovery Mode (distinct opt-in — not a Distress Mode variant)

Distress Mode is read-only: here is your situation, here is what the chain recorded.
Recovery Mode is a legal and social action: I am choosing to join a collective filing.
These are categorically different. The mode boundary is the consent moment.

Recovery Mode is never triggered automatically. Marco opts in after the distress
answer has resolved. If he opts in while his funds are intact, the framing shifts:
*"You can register now. If your address is swept before the claim window closes,
your registration is already in place."*

**What Recovery Mode actually does:**

1. Marco provides his address set (he has already entered these in Distress Mode —
   they carry forward, with his confirmation).

2. Legend generates a sealed component: Marco's addresses + the chain-verified amount
   that moved (or the verified amount currently held, for intact-but-at-risk claims) +
   a Merkle proof anchoring both to a specific block height. This component is
   encrypted to the trustee's public key. Legend never sees the plaintext again.
   Marco receives a local copy.

3. Marco receives a Pass Access-class credential: a blind-signed token that attests
   "entitled to participate in this recovery" — nothing more. The credential does not
   encode the group ID in a redemption-linkable way. The binding to this specific
   recovery happens via the sealed component, submitted separately. No amount.
   No address. No group membership visible on redemption.

4. Marco submits the sealed component via Share. Share moves encrypted ciphertext
   and a routing token. Share sees: a blob, a timestamp. Not the group structure,
   not any amount, not the aggregate.

5. Done. Marco receives a receipt: a timestamp and a reference number (opaque —
   not linked to his identity or amount). He can return with his credential to
   check filing status.

**What the trustee receives:**

A set of sealed components. Each is a self-contained, Merkle-verified claim.
The trustee cannot open them without legal process (the components are encrypted
to a threshold key held jointly — see attestation quorum below). Once opened,
each component is independently verifiable against the chain — no trust in Legend
required for verification.

**The attestation quorum (mixed — not Legend's internal nodes alone):**

The aggregate attestation — "N sealed components received, total [X BTC] claimed,
each component individually verifiable" — is signed by a mixed quorum:
Legend (one share) + the appointed trustee (one share) + a designated legal
representative (one share). 3-of-4 or similar threshold, using FROST. No single
party, including Legend's operator, can forge or block the filing. This is the same
FROST 3-of-4 already planned for canary and mint — same primitive, different
application.

**UK IPA caveat applies here as everywhere:** A compelled-continuation order served
on Legend's operator does not lapse the quorum — two other parties hold shares.
The mixed quorum design specifically addresses the single-operator IPA exposure
the threat model identified. This is the architectural answer to that gap, not
a policy promise.

#### What Legend does not do

Legend cannot undo what the chain recorded. It cannot recover funds. It cannot
compel a trustee, file on Marco's behalf, provide legal advice, or replace a
solicitor. These are stated on the Recovery Mode entry screen, in plain language,
before Marco opts in:

*"Legend provides the chain evidence and the coordination format. You will need
your own solicitor. Legend cannot represent you or recover your funds."*

---

### Track B — Elena. Sovereign treasury. National fiscal event.

#### The moment

El Salvador's Bitcoin Office holds a reserve built on a declared daily purchase
programme. Their node operator has detected anomalous signing activity. Elena —
the duty treasury officer — is not a cryptographer. She is a treasury professional
who understands fiduciary duty, legal procedure, and political consequences.

She has her own legal team. She does not need to join a victim group.
She needs a verified record of what the chain shows, fast, in a format her
counsel can use.

#### First five seconds — Elena's screen is not Marco's screen

Elena's first screen is Watch/Verification mode (from UC-4), not Distress Mode.
She has a declared reserve address set already known. What she sees:

*"Movement detected on [address] at block [height], [date].*
*Destination: address set we cannot identify.*
*Canary status: Legend operating normally, no legal compromise detected."*

Institutional register throughout. "Reserve address," not "wallet."
"Verified movement," not "transaction detected."
"Canary status" explained in one sentence: *"Legend's verification service is
operating normally and has not been legally compromised."*

#### Elena's ten-minute arc

**Minutes 0–3:** Movement confirmation across all reserve addresses in the declared
set. Aggregate moved. Aggregate remaining. Block height, timestamp. No jargon.

**Minutes 3–7:** Chain trace for the moved funds. Every identifiable hop,
in plain language. "Moved on [date] to address set [X]. Subsequent movement
to [Y] on [date]." Honest about chain hops that cannot be identified
(e.g. CoinJoin outputs, privacy-preserving consolidation).

**Minutes 7–10:** One artefact — a verified-loss attestation:
Merkle-anchored, block-height-stamped, quorum-signed. This document is the
chain's record of what happened, authenticated by Legend's quorum, formatted
for a legal team. It is not a legal opinion. It is not a valuation. It is
chain evidence, made court-readable.

Elena's legal team receives the attestation via Share — same private transport,
same architecture. Elena does not enter Recovery Mode. There is no peer group
to coordinate with. The coordination layer degenerates for a claimant of one.

**What changes architecturally for Elena:** nothing fundamental. She uses
three of the five primitives: Chain Trace verification, sealed attestation
format, and quorum signing. The sealed-component / blind-membership /
peer-coordination layer is structurally absent because she has no peers
to be private from. Same bricks, subset.

#### The sovereign dimension — standard, not dependency

The strongest version of Elena's story is pre-compromise, not post.
A national Bitcoin office that has adopted Legend's attestation format,
established a trustee designation, and has watch addresses active before
an incident will produce a court-ready verified-loss attestation at hour
zero instead of a decade of spreadsheets.

That is not a product sale. It is a standard of care.

The honest framing — and the one that survives a solo operator's limits —
is that Legend defines an open, verifiable format: sealed-component
structure, attestation schema, Merkle anchoring. The format is MIT-licensed,
independently implementable, and verifiable by anyone with chain access.
A treasury adopts a *format that outlives the operator*, not a dependency
on one person's infrastructure.

*"A Bitcoin treasury without verified recovery protocols in place is
operating below the standard of care."*

That sentence is accurate. It is not a dependency claim. It does not require
Legend-the-company to be operational for the statement to be true.

**The boundary — stated on the screen and in the article:**

Legend provides: chain-trace attestations, sealed-component and collective-filing
format and machinery, private transport, quorum-signed verified-loss documents.

Legend points to, does not provide: legal representation, insolvency administration,
diplomatic or IMF engagement, custody, fund recovery, reserve valuation.

*"Legend produces the evidence and the coordination format. The trustee, the court,
and the treasury's own legal and diplomatic teams do everything else."*

---

### The civilisational argument — why this is not scope creep

The chain recorded Marco's loss and Elena's loss in the same ledger, in the same
way, readable by the same five primitives. The Bradford daughter (UC-1) and the
El Salvador treasury officer (Track B) are the ends of the same scale.

Legend's job is to make the chain's record legible and actionable at both ends
using one set of components — sealed attestation, blind membership credential,
quorum signing, private Share transport, Chain Trace verification. Marco uses all
five. Elena uses three. The cathedral and the gatehouse: same bricks.

This is not scope creep. It is the point.

*"Chainalysis works for the observer. Legend works for the owner."*

---

### Share — v3 design constraint (flag for build, not v1)

Share moves encrypted ciphertext plus a routing token and timing metadata.
It does not see group structure, amounts, or aggregate.

**Constraint:** if N victims all submit sealed components to one fixed trustee
endpoint, Share and a network observer can count the group and correlate timing.
Content is hidden; the fact of a drop is not.

**v3 design requirement:** sealed components route to rotating, ephemeral drop
points — not a single fixed endpoint. Timing jitter added by design.

**Copy requirement (from v3 launch):** *"Share hides what you send, not that
you sent something. For the strongest protection, use Tor."*

Flag: design in, not bolt on. Surfaces in Share architecture session before v3.

---

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
