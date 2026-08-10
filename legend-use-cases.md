# legend-use-cases.md — Legend Use Case Library
> **Version:** 1.0 | **Created:** Multi-9 · 11 Aug 2026
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

5. **Use-case Opus sessions** — UC-1 through UC-8, weekly/fortnightly cadence,
   running in parallel with build sessions.

---

## On the scope of Legend

A block explorer was the starting description. It remains accurate.

It undersells what Legend actually is: infrastructure for how value gets read and
trusted across every scale of human economic life — from a daughter in Bradford
checking her father's wallet on a phone, to a canton in Switzerland publishing a
civic treasury, to a merchant federation settling a week of Lightning payments in
a single on-chain UTXO.

The chain doesn't know any of these humans. Legend doesn't either.
That is the point.

*"Chainalysis works for the observer. Legend works for the owner."*

---

*"Nothing stops this train."*
