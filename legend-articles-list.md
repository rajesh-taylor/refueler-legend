# legend-articles-list.md — refueler-multi-core /notes/ pipeline
> **Version:** 1.4 | **Created:** Multi-3 · 3 Aug 2026 | **Updated:** UC-9 Opus · 23 Aug 2026
> Editorial planning document. Lives in `refueler-multi-core/` alongside CLAUDE.md and SESSIONS.md.
> Load when in an editorial planning or article build session. Not by default.
> Publishing platform: `refueler.io/notes/` (main domain, not subdomain).
> Built articles (HTML/NJK/styling) live in `refueler-io`. This file tracks scope only.
> Mirrors the pattern established by `notes-articles-list.md` in `refueler-share/`.

---

## Editorial conventions

**Byline:** Rajesh Taylor (personal, named). Not "Refueler" or "The Refueler Team."
**Voice:** Notes = Refueler brand voice with personal register bleeding through. Editorial = pure brand voice. The distinction matters.
**Register:** Authoritative but readable. Not academic. Not marketing. A named founder who knows what they're talking about and isn't pretending to be a committee.
**Typography:** Source Serif 4 body. IBM Plex Mono for data, tables, and inline technical strings. Paper/Carbon tokens throughout — same design system as Share.
**Table styling:** All table columns carry the same font weight and colour. No lighter secondary-text treatment on any column. If it's in the table, it has the same authority as the rest of the table.
**CTAs:** Each article earns its own ending. No standard template. No hard sell.
**Eleventy structure:** One `src/notes/[slug]/index.njk` per article. New card added by hand to `src/notes/index.njk`. No collection, no config changes.
**honest_metadata.json:** Lives in `src/_data/honest_metadata.json` in `refueler-io` repo.

---

## What this file is not

Not a content calendar. Not a publishing schedule. No artificial deadlines.
Articles publish when they're ready and when there's something honest to say.
Legend articles unlock post-B9 when infrastructure exists to back the claims.
Do not publish Legend articles before the explorer is live — claims must be demonstrable.

---

## Article pipeline

---

### Article A — What querying Mempool.space tells the server about your clients

**Slug:** `what-mempool-tells-the-server`
**Status:** Queued for drafting. No publish date. 2–3 weeks redraft time after draft produced.
**Audience:** UK solicitors, accountants, compliance professionals handling client Bitcoin.
**Dependency:** None — can draft before Legend is live. Publish after Legend live.

**The argument:**
Every time a solicitor or accountant queries Mempool.space to check a client's
Bitcoin holdings, they are telling Mempool's server exactly which address they
checked, at what time, from which IP (typically a corporate or home IP linked to a
regulated professional). Mempool.space is US-incorporated — subpoenable under MLAT.
A data request naming a specific firm returns every client address that firm ever
checked. This is a compliance exposure the SRA and ICAEW have not addressed. Legend
is the correct endpoint for any professional handling client Bitcoin.

**Beats:**
- What Mempool's server logs actually contain from a professional query
- The corporate IP problem: regulated entities are identifiable
- Sequence-of-queries: multiple clients in one session are linked in the log
- MLAT reach: US incorporation, UK professional, client data
- What the SRA's crypto asset guidance actually says
- Legend as the professional endpoint: query metadata is architecturally absent
- The honest scope: Legend protects query metadata, not on-chain history

**CTA:** Contact Refueler for Enterprise access.
**Links:** Earns professional services inbound links. UK compliance angle positions
Refueler as serious infrastructure for regulated professionals.

---

### Article B — Four numbers that tell you where UK Bitcoin adoption actually is

**Slug:** `four-numbers-uk-bitcoin-adoption`
**Status:** Queued for drafting. Quarterly cadence — update figures each quarter.
**Audience:** UK investors, financial press, anyone asking "how big is Bitcoin in the UK actually?"
**Dependency:** None. Pure research article. No Legend dependency.

**The four numbers:**
1. HMRC CGT receipts attributable to cryptoassets (published annually)
2. FCA registered cryptoasset businesses (published and updated by FCA)
3. UK Bitcoin/crypto ETP AUM (ETF Stream, Bloomberg data)
4. Coinbase and Kraken UK entity revenues from Companies House filings

**The argument:**
The noise around UK Bitcoin adoption is enormous. The signal is in four publicly
available numbers that almost nobody reads together. Quarterly cadence makes this a
reliable reference piece. Each update is a reason to publish. Positions Refueler as
the serious infrastructure brand that does the homework.

**Beats:**
- Each of the four numbers with source, methodology note, and context
- What they tell you together that none tells you alone
- One paragraph of honest interpretation — not cheerleading, not doom
- "We publish this quarterly because we think infrastructure companies should know
  the market they're building for."

**CTA:** Subscribe for quarterly updates. (No email capture — link to notes page.)
**Note:** Quarterly cadence means this becomes a reference piece that compounds in
search value over time. Brand play as much as content play.

---

### Article C — What BIP-110 means for wallets, Taproot, and second-layer protocols

**Slug:** `bip-110-taproot-second-layer`
**Status:** Queued for drafting. Timely if BIP-110 activates; evergreen if not.
**Audience:** Bitcoin developers, wallet teams, second-layer protocol builders,
technically curious Bitcoiners following protocol development.
**Dependency:** None. Can draft now. Timely hook if BIP-110 gets traction.

**The argument:**
BIP-110 proposes restricting arbitrary data embedding via a 55% UASF threshold,
targeting inscription/ordinal use of Taproot. The downstream implications for
legitimate Taproot-dependent protocols — Ark, Spark, certain Miniscript constructions,
covenants-based approaches — are underexplored in public discourse. This article
maps the implications honestly: what BIP-110 would and wouldn't affect, which
second-layer protocols have exposure, and what wallet teams and protocol developers
need to know.

**Beats:**
- What BIP-110 actually proposes (precise, not sensationalised)
- The UASF threshold: 55%, what that means historically
- Taproot dependency map: which protocols rely on which Taproot features
- Ark and Spark exposure — honest assessment, not alarmist
- Covenant-based protocols: what's at risk and what isn't
- What wallet teams should be watching
- The Legend angle (one paragraph, earned not shoehorned): Legend tracks Ark and
  Spark address support as v2/v3 scope items — BIP-110 is a variable in that plan

**CTA:** Follow Refueler notes for protocol coverage. Link to Legend scope.
**Note:** Professional services implications angle goes one level deeper than a
layman guide. Aimed at people who build things, not people who hold things.

---

### Article 14 — The metadata leak nobody talks about

**Slug:** `what-your-block-explorer-knows-about-you`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Every Bitcoiner. Coldcard users specifically. Family offices. Anyone who has ever pasted an address into Mempool.space.
**Dependency:** Legend explorer live at `refueler.io/legend`.

**Opening line (locked):**
"Every time you look up a Bitcoin address on a public block explorer, you're telling that server exactly what you own and what you're watching."

**The argument:**
Public block explorers are surveillance infrastructure dressed as public utilities.
Mempool.space and Blockstream.info log every query. They know which addresses you
watch, when you check them, and from which IP. When a hardware wallet supplier is
breached and users rush to check whether their addresses have been swept, they compound
the breach — they've now told the explorer exactly which addresses they're worried about,
at the precise moment they have reason to believe those addresses are under threat.
The Coldcard Mk3 event (August 2026) demonstrated this at scale: 1,200 BTC swept in
a coordinated no-dust attack, with victims then querying public explorers to find out
what happened — handing a second data point to anyone watching Mempool's logs.
Legend is architected so this is structurally impossible. Not a policy promise.
The maths won't allow it.

**Beats:**
- The Coldcard/hardware wallet breach as the concrete example
- What Mempool.space's server logs actually contain
- Why Tor helps with IP but not with query content
- What Legend does differently (PIR, ephemeral sessions, Cashu credentials)
- The one honest sentence: "We can't see what you asked. Not because we promised.
  Because the maths won't let us."

**CTA:** Use Legend. Link to `refueler.io/legend`.

---

### Article 15 — From Chaum to Satoshi to Legend

**Slug:** `chaum-satoshi-legend-44-years-of-financial-privacy`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Technically curious Bitcoiners. Cashu users. Privacy researchers. Anyone who wants to understand why Legend is the logical next step in a 44-year arc.
**Dependency:** Legend live. Article 14 published first.

**The argument:**
Three people, four decades, one unfinished problem. David Chaum identified in 1982
that electronic payments could be made untraceable using blind signatures. DigiCash
implemented it in 1989 with a centralised mint and failed. Satoshi removed the mint
in 2009 and solved double-spend — but left the privacy layer unbuilt. Cashu restored
Chaumian blind signatures on Lightning in 2022. Legend applies the same blind
signature construction to chain queries in 2026. The trilogy is complete.

**Beats:**
- Chaum 1982: blind signatures, the construction, what it proved
- "Security Without Identification" (1985): the political framing, Big Brother, individual sovereignty
- DigiCash: why it failed (centralised mint, not the cryptography)
- Bitcoin: what Satoshi solved and what he didn't
- Cashu: Chaumian mint returns, trustless by construction, on Lightning
- Legend query credentials: the same blind signature applied to blockchain queries
- Pedersen commitments (1991) and ZK balance proofs: the parallel thread
- The closing line: "Bitcoin settled the payment. Cashu blinded the token. Legend blinds the query."

**CTA:** Read the Chaum 1982 paper (link). Use Legend.

---

### Article 16 — Why Mempool over Tor isn't enough

**Slug:** `mempool-tor-not-enough`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Privacy-conscious Bitcoiners already using Tor. Sparrow Wallet users. Anyone who thinks they've solved the explorer privacy problem.
**Dependency:** Legend live.

**The argument:**
Tor hides your IP from Mempool. It does not hide your query from Mempool. Mempool's
server still knows exactly which address you searched — it just doesn't know which IP
searched it. Query unlinkability is a different problem from IP privacy, and Tor doesn't
touch it. Legend solves the second problem. You need both. We give you the second one.

**Beats:**
- What Tor actually does (IP privacy) and what it doesn't (query privacy)
- What Mempool's logs contain even from a Tor exit node
- Session correlation: reusing a Tor circuit links queries across a session
- Cashu credentials: structurally cannot link query 1 to query 2, same session or different
- Spiral PIR: the server returns an answer without knowing what it retrieved
- The wedge statement: "Tor hides who you are. Legend hides what you asked."
- Sparrow Wallet users: paste Legend's URL into Server preferences, done

**CTA:** Set Legend as your Esplora endpoint in Sparrow. Link to instructions.

---

### Article 17 — The UTXO consolidation problem

**Slug:** `utxo-consolidation-privacy-cost`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Bitcoiners managing their own custody. Anyone consolidating UTXOs before a fee spike. Sparrow Wallet users.
**Dependency:** Legend UTXO advisor live (v1 qualitative flag sufficient for this article).

**The argument:**
Consolidating UTXOs saves fees. It also permanently records on-chain that those UTXOs
were controlled by the same wallet. Most people making this decision are optimising for
fees without knowing the privacy cost. Legend's consolidation advisor shows both numbers
— fee cost precisely, privacy cost honestly. Not a score. A true statement: "This
transaction will permanently reveal these UTXOs are co-owned."

**Beats:**
- What UTXO consolidation is and why people do it
- The on-chain privacy cost: co-ownership heuristic, permanent record
- Why timing matters: fee spike windows vs privacy exposure
- What Legend's advisor shows and how to use it
- The honest scope: v1 is a qualitative flag, not a score — and why that's more honest than a number

**CTA:** Check your UTXOs on Legend before you consolidate.

---

### Article 18 — Silent Payments and why no explorer supports them correctly

**Slug:** `silent-payments-bip352-explorer-problem`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** BIP-352 implementers, privacy-focused Bitcoiners, wallet developers, anyone using or building Silent Payments support.
**Dependency:** Legend SP scanning live (v1).

**The argument:**
Silent Payments (BIP-352) allow a recipient to publish a static address that generates
a unique on-chain address for every sender — unlinkable by design. No explorer currently
supports them correctly because none has the precomputed tweak index required to scan
efficiently. Without it, a full-range SP scan takes hours per address. Legend's
precomputed tweak index reduces this to ~8 minutes on 8 cores. Legend is the first
production explorer to show Silent Payments correctly — and to explain what "correctly"
means to users who have never heard of BIP-352.

**Beats:**
- What Silent Payments are and why they matter
- The scanning problem: why explorers haven't done this
- The tweak index: what it is, why Legend built it, the honest performance figures
- What Legend shows for an SP-eligible transaction (one sentence in plain English)
- The honest scope: what SP protects and what it doesn't

**CTA:** Try a Silent Payments scan on Legend.

---

### Article 19 — ZK balance proofs for Bitcoin holders

**Slug:** `zk-balance-proofs-bitcoin`
**Status:** Scoped. Not drafted. Unlocks post-v2 (ZK balance proofs live).
**Audience:** Bitcoin holders needing to prove reserves. Lenders. Solicitors. Family offices.
**Dependency:** Legend v2 live. ZK balance proofs implemented.

**The argument:**
If you want to prove to a lender, solicitor, or counterparty that you control a certain
amount of Bitcoin, you currently have two options: show them your addresses (permanent
privacy loss) or use a custodian (defeats self-custody). ZK balance proofs give you a
third option: cryptographic proof that you control at least X BTC, without revealing
which addresses. Legend generates the proof. The verifier checks it. No addresses exchanged.

**Beats:**
- The problem: proving reserves without disclosure
- Pedersen commitments: the primitive underneath
- What a ZK balance proof looks like in practice
- The lending use case: Ledn, Unchained, Lava — collateral verification without address disclosure
- The compliance use case: family office legal team, demonstrating holdings for estate purposes
- The honest scope: what ZK proves and what it doesn't

**CTA:** Contact Refueler for Enterprise access. Link to Legend.

---

### Article 20 — Spiral PIR and Bitcoin privacy

**Slug:** `spiral-pir-bitcoin-legend`
**Status:** Scoped. Not drafted. Unlocks post-v2 (Spiral PIR live).
**Audience:** Technical Bitcoiners. Privacy researchers. Developers building on Legend's API.
**Dependency:** Spiral PIR implemented in Legend (v2).

**The argument:**
Private Information Retrieval is a branch of cryptography that asks: can a server
answer a query without learning what was asked? Previous PIR schemes required multiple
non-colluding servers. Spiral PIR (MIT, 2022) achieves this with a single server and
is fast enough for production use. Legend implements Spiral PIR for UTXO lookups.
The server returns your answer. Its logs reveal nothing about what you asked.
This is a world first for a production Bitcoin block explorer.

**Beats:**
- What PIR is and why it matters
- Previous schemes: multi-server, impractical
- Spiral PIR: single server, production-viable, the MIT paper
- How Legend implements it for UTXO queries
- The bandwidth tradeoff: client downloads more than it strictly needs (by design)
- The hybrid approach: PIR for UTXO existence, Tor for full history
- What this means in practice: mathematically provable query privacy, not a policy

**CTA:** Read the Spiral paper (link). Use Legend.

---

### Article 21 — The family office problem

**Slug:** `family-office-bitcoin-privacy`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Family offices, private wealth managers, Bitcoin-holding HNWIs, compliance professionals at wealth management firms.
**Dependency:** Legend explorer live.

**The argument:**
Your advisor's query behaviour is your attack surface. A family office checking client
addresses from a corporate IP — logged into Chrome, on a monitored network, with IT
security logging outbound traffic — has donated your holdings map to anyone who can
reach Mempool's logs. The metadata leak isn't yours alone. It belongs to every
professional who touches your addresses. Worse: family offices check multiple clients
in sequence, which links those clients to each other in the explorer's session log.
A data request naming that family office reconstructs every client's holdings they've
ever checked. Legend is the correct Esplora endpoint for any professional handling
client Bitcoin.

**Beats:**
- The corporate IP problem: your advisor's query logs are attached to a regulated entity
- Sequence-of-queries: multiple clients checked in one session are linked in the log
- What a data request to Mempool naming a specific firm would return
- The compounding problem: IT security logging, browser history, compliance obligations
- Legend as the professional endpoint: one Enterprise contract covers all client queries
- The honest scope: Legend doesn't know who you're checking either

**CTA:** Contact Refueler for Enterprise access.

---

### Article 22 — The jurisdiction problem

**Slug:** `bitcoin-jurisdiction-mobility-explorer-logs`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** HNWIs relocating jurisdictions, lawyers advising on asset restructuring, privacy-conscious Bitcoiners in politically unstable environments.
**Dependency:** Legend explorer live.

**The argument:**
If you're moving jurisdictions — relocating, restructuring, or leaving — your
pre-departure query behaviour is a timestamped record of asset attention that
Five Eyes-adjacent legal processes can reach. Mempool.space is US-incorporated.
Blockstream is Canadian. Both are subpoenable under mutual legal assistance treaties.
A British national checking addresses from a London IP the week before departure has
handed a legal record of pre-departure asset attention to any jurisdiction that knows
to ask. Legend retains nothing. Not a policy promise — the architecture doesn't permit
retention. There is nothing to subpoena.

**Beats:**
- What MLAT requests can reach and how quickly
- The timing problem: query logs are most dangerous at exactly the moment people use them
- State-level monitoring: bulk collection doesn't discriminate
- The Five Eyes legal chain: what a request to a US-incorporated entity returns
- Legend's structural answer vs a policy promise
- The honest scope: Legend protects query metadata, not on-chain history

**CTA:** Use Legend. Link to Article 14.

---

### Article 23 — Bitcoin and your estate

**Slug:** `bitcoin-estate-planning-privacy`
**Status:** Scoped. Not drafted. Unlocks post-v2 (estate verification tooling live).
**Audience:** Bitcoin holders with meaningful holdings, UK solicitors, estate planners, family members of Bitcoin holders.
**Dependency:** Legend v2 live. /legend/verify endpoint live.

**The argument:**
Bitcoin is the only major asset class where the wealth permanently disappears if the
owner dies without proper succession planning. Solicitors don't understand it. Banks
won't custody it. The Solicitors Regulation Authority has mandated crypto asset
accounting in estates and provided no tooling. Legend provides: time-locked balance
verification at a specific block height for probate, inheritance script monitoring,
multi-sig quorum verification in plain language, and ZK balance proof output a probate
court can verify. Written entirely in language a solicitor — not a cryptographer —
can act on.

**Beats:**
- The permanent loss problem: no recovery mechanism for lost keys
- What the SRA actually requires and what tooling gap exists
- What "holdings at date of death" means technically and how Legend proves it at a block height
- Time-locked inheritance scripts: Liana wallet, Miniscript, what a solicitor needs to know
- ZK balance proof as evidence: what it proves, what it doesn't, what a court sees
- The honest scope: Legend provides the technical verification; legal acceptance depends on frameworks outside our control

**CTA:** Contact Refueler for Estate Verification access. Link to /legend/verify.

---

### Article 24 — What the Mt. Gox victims needed that didn't exist

**Slug:** `what-mt-gox-victims-needed`
**Status:** Scoped. Not drafted. Two-phase publish.
Phase 1 unlocks post-v2 (Distress Mode v1 live, Chain Trace Report v2 live).
Phase 2 sovereign/Recovery Coordination Layer beats added at v3 unlock.
**Audience:** Distressed Bitcoiners. Anyone who has suffered exchange collapse,
hardware wallet compromise, or mass-breach events. Solicitors advising creditors in
crypto insolvencies. Bitcoin historians. National Bitcoin treasury officers.
**Dependency:** Legend Distress Mode v1. Chain Trace Report v2. Mixed attestation
quorum operational (v3). Full outline produced in UC-9 Opus session (23 Aug 2026).

**The argument:**
When Mt. Gox collapsed in February 2014, approximately 850,000 Bitcoin disappeared.
The creditors spent a decade filing spreadsheets with a Japanese trustee, unable to
verify independently what the chain already recorded. Every tool they needed existed
in cryptographic literature. None had been built for humans in distress.

Legend builds them: plain-language chain tracing from the moment of distress,
Merkle-verified legal documents, private transmission via Share, address watching
with Pass-native alerts, and a coordination layer for mass-compromise events where
victims submit sealed individual claims to a mixed trustee quorum — without revealing
holdings to each other or to Legend, without any victim co-signing anything with any
other victim.

At civilisational scale: a national Bitcoin reserve loss is a fiscal event.
El Salvador's declared purchase programme makes this scenario material, not
theoretical. A Bitcoin treasury without verified recovery protocols in place is
operating below the standard of care — and that standard is an open, verifiable
format, not a dependency on Legend's infrastructure.

**Opening line (locked):**
"When Mt. Gox collapsed, the Bitcoin was gone but the chain still knew where it went.
The victims had no tool to read it. We're building that tool now."

**Phase 1 beats (publish at v2):**
- The Coldcard Mk3 event (August 2026): AI-assisted victim enumeration, no dust,
  coordinated sweep — the attacker model that makes every previous recovery playbook obsolete
- What Mt. Gox victims actually needed in February 2014 — not what they got
- The chain trace: every hop permanent and public, unreadable without tooling
- Distress Mode: the 2am screen — plain language, no jargon, no onboarding, no modal,
  works whether funds are intact or already gone
- The no-dust honest caveat: Pass tells you what happened as soon as the chain records
  it; it cannot warn you before a coordinated sweep that leaves no test signal
- Chain Trace Report: Merkle-verified, solicitor-legible, timestamped at block height
- Why emailing the report compounds the breach — and what Share does instead
- Share integration: sealed transport, "Send privately" button, 72-hour expiry,
  recipient needs no account
- Address Watch: knowing your address is being swept while you still have minutes
  (when the attacker leaves a test signal — honest about when this doesn't apply)
- Pass as notification layer: Cashu watch credential, quiet alert during daily use,
  no address or amount in the notification, no corporate IT log signal

**Phase 2 beats (add at v3 unlock):**
- Recovery Mode: the consent moment — a door, not a push; explicitly not a
  continuation of Distress Mode
- The sealed-component model: each victim's claim encrypted to a trustee quorum,
  submitted via Share, independently verifiable against the chain without trust
  in Legend — no victim co-signs anything with any other victim
- The dead drop analogy: each agent deposits separately; the composite is presented
  to the minister; a compromised agent does not expose the others
- The mixed attestation quorum: Legend + appointed trustee + legal representative,
  3-of-4 FROST — no single party including Legend's operator can forge or block
  the filing; the IPA single-operator gap addressed architecturally
- Pass credential architecture: attests "entitled to participate," not which group
  or what amount; binding to specific recovery via sealed component only
- Share v3 constraint: rotating ephemeral drops, timing jitter — content hidden,
  and the fact of submission made harder to correlate; honest copy: "Share hides
  what you send, not that you sent something"
- The lawyer sequencing: format first, relationships second; two jurisdictions
  (common law + civil law), not fifty; Legend names the class of trustee,
  the victim engages their own counsel
- Elena's arc (sovereign track): same five primitives, three in use — Chain Trace,
  sealed attestation, quorum signing; no peer group; the coordination layer
  degenerates for a claimant of one; ten minutes to a court-ready document
- The proactive standard: a treasury that has the format, the trustee designation,
  and the watch addresses active before an incident produces a court-ready
  attestation at hour zero; a treasury that does not is operating below standard
- The open format argument: Legend defines a verifiable standard — MIT-licensed,
  independently implementable — not a dependency on one operator's uptime
- The honest scope statement: Legend produces the evidence and the coordination
  format; the trustee, the court, and the treasury's legal and diplomatic teams
  do everything else
- The Hoseki comparison: a notary for calm days; Legend is the forensic layer
  for crisis ones
- Closing: the chain recorded Gox, Bradford, and San Salvador in the same ledger.
  Legend reads it — for the owner, not the observer.

**CTA:** Use Legend. If you're in distress right now, start here.
*(When Distress Mode live: direct link, no modal, no onboarding.)*

**Note:** This article is simultaneously the strongest Legend article and the strongest
Share article not yet written. Coordinate publication with Share editorial plan.
Article 17 (Mt. Gox): this is UC-9 output — Opus session 23 Aug 2026.
Do not conflate Article 17 (UTXO consolidation) with Article 24 (Mt. Gox / Recovery).

---

### Article UC-5 — The Florentine Protocol

**Slug:** `the-florentine-protocol`
**Status:** Full outline locked — UC-5 Opus · Multi-14 · 26 Aug 2026.
**Audience:** Bitcoin-native jurisdictions, policy makers, talent-attracting institutions,
cryptographers and engineers evaluating institutional offers, technically curious Bitcoiners.
**Dependency:** Legend Civic Treasury View live (v1). Full Civic Treasury Mode live (v2).
Outline can be drafted now; publish once Civic Treasury View ships.

**Opening line (locked):**
"Florence in 1450 was the most valuable square mile in Europe. It didn't get there by taxing more. It got there by paying better."

**The argument:**
Value accrues to the jurisdiction that earns the right people, not the one that extracts the
most from them. A credibly-demonstrated Bitcoin treasury paying stipends in Bitcoin is the
modern Medici pull — not extraction, attraction. Silent Payments is the cryptographic
equivalent of the Medici stipend envelope: you can see that money went out, and how much,
and how often. You cannot see who received it. Legend makes the treasury verifiable without
making the recipients visible. That is not a limitation. That is the architecture.

**Beats:**
- The Medici model: attraction, not extraction. Brunelleschi, Leonardo, Botticelli — the
  city-state that attracted talent compounded in value faster than any that merely taxed it.
- The 2033 civic-treasury declaration: canton, city-state, special economic zone — whoever
  first credibly demonstrates a Bitcoin reserve paying talent in Bitcoin wins the talent.
- What a treasury should be able to prove publicly: that it exists, that it is funded, that
  it pays out consistently. Not who it pays.
- Silent Payments as the stipend envelope: pattern visible, recipient unlinkable. The
  cryptographer in Edinburgh can verify that 47 outflows left this treasury in 12 months.
  She cannot see that she is one of the prospective recipients — and neither can anyone else.
- What Legend refuses to do: vouch for the identity behind a declared name; imply that a
  declared set is the complete institutional footprint; call a voluntary disclosure an audit.
- The honest scope: declared, not audited; self-verifiable; the institution names itself and
  Legend checks the chain — the reader verifies both independently.
- Why "verifiable but private" is the only civic model that attracts rather than surveils.
  A treasury that reveals its recipients is a surveillance list. A treasury that hides its
  activity is unverifiable. Legend occupies the only honest middle: provable without exposure.

**Closing:**
The city-state is back. It can prove its treasury without exposing its people. The Medici
would have recognised the architecture immediately — even if they'd never heard of a block.

**CTA:** View a civic treasury on Legend. (Link once Civic Treasury View live.)
**Note:** Do not deploy "Chainalysis works for the observer. Legend works for the owner."
here — locked phrase for Articles 14/15 and presentations only.

---

### Article UC-6 — The Hanseatic Protocol

**Slug:** `the-hanseatic-protocol`
**Status:** Full outline locked — UC-6 Opus · Multi-14 · 26 Aug 2026.
**Audience:** Bitcoin merchants, Fedimint operators, Lightning network builders,
economic historians with a sense of humour, anyone who thinks the word "federation"
sounds better than "bank."
**Dependency:** Legend Federation Settlement View live (v1). Declared Fedimint-attestation
rendering (v2). Outline can be drafted now; publish once settlement-pattern display ships.

**Opening line (locked):**
"The Hanseatic League ran for 300 years without a central bank. Here is what its Bitcoin successor looks like, and how Legend reads its ledger."

**The argument:**
Merchant federations settling in Bitcoin are rediscovering the Hanseatic model — mutual
credit, shared commercial law, no sovereign anchor. Legend is the port ledger: what
arrived, what departed, what the aggregate represents. Not surveillance. Literacy.
The absence of individual-transaction visibility inside each settlement is the feature,
not the gap. The League ran on trust and shared record-keeping. The Bitcoin version
runs on cryptography and on-chain settlement. Legend reads the ledger honestly — for
the federation, not the revenue authority.

**Beats:**
- The Hanseatic League: Hamburg to Riga, wool to spice, three centuries without a
  central issuer. What made it work was shared record-keeping and mutual credit — a
  port ledger that every member could read and no single city could falsify.
- The Custom House, Lower Thames Street, London: the most legible port ledger in
  northern Europe, and one of the most surveilled buildings in the City. Customs
  officials were routinely compromised — merchants, smugglers, and foreign intelligence
  services alike occupied the ledger. The record was public; the *reading* of it was
  not safe. Legend's port ledger is readable by anyone and logged by no one. The
  spies who occupied Custom House would have found nothing to sell.
- The 2031 Fedimint version: Hamburg leather to Bruges linen to Valletta spice,
  Lightning routing inside federations, weekly on-chain settlement in a single
  aggregated UTXO. The customer in Riga never appears in the settlement record.
  Neither does the merchant's individual sale.
- What Legend shows: the settlement UTXO — amount, block height, date. The cadence
  that makes the aggregate legible: most recently this amount, on this date, at this
  height. Pattern is the information.
- What Legend cannot tell you from the chain alone: whether this UTXO is a Fedimint
  settlement, a batching service, or something else entirely. Legend describes what
  it can see and nothing more. When a federation has declared its address, Legend
  renders that declaration assertively. When it has not, the conditional absence
  note is all that is honest.
- Why the absence of individual-transaction visibility is correct: the payments are
  inside the Fedimint. The Fedimint handled them. The on-chain UTXO is the settlement,
  not the ledger of every trade. The League's port ledger recorded ships and cargo,
  not every handshake in the counting house.
- Why Bitcoin settlement is resistant to what killed the original League: nation-states
  eventually got powerful enough to override the League's shared commercial law. They
  cannot override a confirmed on-chain UTXO. The settlement is final regardless of
  who objected.

**Closing:**
The League collapsed when nation-states got strong enough to override it. On-chain
settlement doesn't have that problem. And a ledger that reads itself honestly —
open to anyone, logged for no one — doesn't need a Custom House full of compromised
officials to keep it.

**CTA:** View federation settlement on Legend. (Link once Federation Settlement View live.)

---

### Article UC-7 — Who Gets Priced Out

**Slug:** `who-gets-priced-out`
**Status:** Outline in `legend-use-cases.md` UC-7. Full outline to be produced in UC-7 Opus session.
**Audience:** Every Bitcoiner. Global south advocates. Anyone who cares whether Bitcoin's
financial inclusion promise survives fee spikes.
**Dependency:** Legend human cost calculator live (v1). Historical fee context layer (v2).

**The argument:**
When fees hit 800 sat/vbyte, Bitcoin works fine for whales. For everyone else it is
arithmetic: the fee exceeds the transaction value. Block space is a commons. When it
is weaponised — deliberately or incidentally — the smallest users pay the highest
relative cost. Legend shows this not as a political statement but as arithmetic.
The chain doesn't lie.

**Opening line (provisional):**
"When fees hit 800 sat/vbyte, Bitcoin works fine for whales. Here is what it looks like for everyone else."

**CTA:** Check current fee impact on Legend.

---

### Article UC-8 — The UTXO Lottery

**Slug:** `the-utxo-lottery`
**Status:** Outline in `legend-use-cases.md` UC-8. Full cryptographic design in UC-8 Opus session.
**Audience:** Bitcoin developers, cryptography enthusiasts, anyone interested in on-chain
coordination mechanisms that require no operator.
**Dependency:** Legend movement count display live (v1). Full lottery eligibility (v2).

**The argument:**
Bitcoin's longest debate is whether it is money or gold. Here is a lottery that only
works if it is money. Movement velocity is a monetary property. A UTXO that has moved
ten times in a year is being used as money. The lottery rewards that — trustlessly,
with the block hash as the entropy source and no operator able to manipulate the draw.

**Opening line (provisional):**
"Bitcoin's longest debate is whether it is money or gold. Here is a lottery that only works if it is money."

**CTA:** Check your UTXO's lottery eligibility on Legend.

---

## Publishing sequence

Articles publish in this order, each unlocking when its dependency is met:

| Order | Article | Dependency |
|-------|---------|------------|
| A | What querying Mempool tells the server about your clients | None (draft now, publish post-Legend live) |
| B | Four numbers that tell you where UK Bitcoin adoption actually is | None (quarterly cadence) |
| C | What BIP-110 means for wallets, Taproot, second-layer protocols | None (timely if BIP-110 activates) |
| 14 | The metadata leak nobody talks about | Legend live |
| 15 | From Chaum to Satoshi to Legend | Legend live, Article 14 published |
| 16 | Why Mempool over Tor isn't enough | Legend live |
| 17 | The UTXO consolidation problem | Legend v1 UTXO advisor live |
| 18 | Silent Payments and why no explorer supports them | Legend SP scanning live |
| 21 | The family office problem | Legend live |
| 22 | The jurisdiction problem | Legend live |
| UC-5 | The Florentine Protocol | Legend Civic Treasury View live (v1) — outline locked Multi-14 |
| UC-6 | The Hanseatic Protocol | Legend Federation Settlement View live (v1) — outline locked Multi-14 |
| UC-7 | Who Gets Priced Out | Legend human cost calculator live |
| 19 | ZK balance proofs for Bitcoin holders | Legend v2 live |
| 20 | Spiral PIR and Bitcoin privacy | Legend v2 PIR live |
| UC-8 | The UTXO Lottery | Legend movement eligibility display live (v2) |
| 23 | Bitcoin and your estate | Legend v2 live, /legend/verify live |
| 24 | What the Mt. Gox victims needed | Phase 1: Distress Mode v1 + Chain Trace v2. Phase 2: v3 unlock. |

Articles A, B, C can publish before Legend is live — no infrastructure dependency.
Articles 14, 15, 16, 21, 22 publish in close succession at Legend launch.
Articles 17, 18 follow as features confirm stable.
UC-5, UC-6, UC-7 slot in alongside v1/v2 feature rollout.
Articles 19, 20, UC-8, 23 are v2 unlocks — no timeline pressure.
Article 24 publishes in two phases: Phase 1 at Distress Mode + Chain Trace launch;
Phase 2 sovereign/Recovery Coordination Layer beats at v3 unlock.

---

*Next article session: Articles A, B, C draft block. 2–3 weeks redraft time. No publish pressure.*
*Article 24 full outline complete — UC-9 Opus session 23 Aug 2026.*
*Articles UC-5 and UC-6 full outlines complete — UC-5/UC-6 Opus · Multi-14 · 26 Aug 2026.*
*"Nothing stops this train."*
