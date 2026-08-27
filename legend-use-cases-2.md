# legend-use-cases-2.md — Legend Use Case Library (UC-5–9)
> **Version:** 1.0 | **Created:** Multi-[n] restructure · 25 Aug 2026
> **Extracted from:** `legend-use-cases.md` v1.3 (lines 664–1163)
> Scenarios 5–9 (briefs), Opus session structure, B9 gate note, and scope essay.
> UC-1 through UC-4 (full specs) are in `legend-use-cases.md`.
> Load alongside `CLAUDE.md` and `SESSIONS.md` in any UC-5–9 Opus session.
> Companion: `legend-scope.md`, `legend-design-spec.md`, `legend-ui-modes.md`.

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
- **Watch Recipient View reuse** — check UC-4's Watch Recipient View for surface
  reuse before speccing a new mode. The civic treasury public view may be a
  variant of the Watch Recipient View with the no-alert footer adapted for a
  public transparency context rather than a covenant monitoring context.

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

**⚠ RETIRED — Multi-16 · 27 Aug 2026**

UC-8 was scoped in Multi-16 and immediately retired. The scenario was found to be outside
Legend's mission scope on two grounds:

1. **Product clarity.** Legend is a privacy-first block explorer for Bitcoin owners and
   professionals. A lottery-verification surface muddies that positioning and attracts
   a use-case class (on-chain gambling/coordination mechanisms) inconsistent with the
   product's identity.
2. **Gambling-law gate.** The surface could not ship without a UK solicitor review of
   Gambling Act 2005 promotion-of-a-lottery exposure — a gate that confirms the feature
   belongs elsewhere.

The cryptographic kernel (block-hash entropy as a manipulable-but-bounded beacon;
value-weighted Sybil-resistant eligibility; forward-from-declared-receipt lineage) may
surface as a *primitive* discussion in a future CryptoRoadmap session or Article 15,
not as a product feature.

Article UC-8 ("The UTXO Lottery") retired with the use case. No article stub kept.
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
as its primary input alongside `CLAUDE.md`, `SESSIONS.md`.

### Session format

**Load:** `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases.md` (UC-1–4) or `legend-use-cases-2.md` (UC-5–9)
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

- Interface spec for the scenario's UI mode (feeds `legend-ui-modes.md`)
- Copy additions for the scenario's language register (feeds `legend-ux-language.md` / `legend-copy-index.md`)
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
