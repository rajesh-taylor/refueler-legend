# legend-articles-list.md — refueler-legend /notes/ pipeline
> **Version:** 1.0 | **Created:** Multi-3 · 3 Aug 2026
> Editorial planning document. Lives in `refueler-legend/` alongside CLAUDE.md and SESSIONS.md.
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

### Article 14 — The metadata leak nobody talks about

**Slug:** `what-your-block-explorer-knows-about-you`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Every Bitcoiner. Coldcard users specifically. Family offices. Anyone who has ever pasted an address into Mempool.space.
**Dependency:** Legend explorer live at `refueler.io/legend`.

**Opening line (locked):**
"Every time you look up a Bitcoin address on a public block explorer, you're telling that server exactly what you own and what you're watching."

**The argument:**
Public block explorers are surveillance infrastructure dressed as public utilities. Mempool.space and Blockstream.info log every query. They know which addresses you watch, when you check them, and from which IP. When a hardware wallet supplier is breached and users rush to check whether their addresses have been swept, they compound the breach — they've now told the explorer exactly which addresses they're worried about, at the precise moment they have reason to believe those addresses are under threat. Legend is architected so this is structurally impossible. Not a policy promise. The maths won't allow it.

**Beats:**
- The Coldcard/Coinkite breach as the concrete example
- What Mempool.space's server logs actually contain
- Why Tor helps with IP but not with query content
- What Legend does differently (PIR, ephemeral sessions, Cashu credentials)
- The one honest sentence: "We can't see what you asked. Not because we promised. Because the maths won't let us."

**CTA:** Use Legend. Link to `refueler.io/legend`.

---

### Article 15 — From Chaum to Satoshi to Legend

**Slug:** `chaum-satoshi-legend-44-years-of-financial-privacy`
**Status:** Scoped. Not drafted. Unlocks post-B9.
**Audience:** Technically curious Bitcoiners. Cashu users. Privacy researchers. Anyone who wants to understand why Legend is the logical next step in a 44-year arc.
**Dependency:** Legend live. Article 14 published first.

**The argument:**
Three people, four decades, one unfinished problem. David Chaum identified in 1982 that electronic payments could be made untraceable using blind signatures. DigiCash implemented it in 1989 with a centralised mint and failed. Satoshi removed the mint in 2009 and solved double-spend — but left the privacy layer unbuilt. Cashu restored Chaumian blind signatures on Lightning in 2022. Legend applies the same blind signature construction to chain queries in 2026. The trilogy is complete.

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
Tor hides your IP from Mempool. It does not hide your query from Mempool. Mempool's server still knows exactly which address you searched — it just doesn't know which IP searched it. Query unlinkability is a different problem from IP privacy, and Tor doesn't touch it. Legend solves the second problem. You need both. We give you the second one.

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
Consolidating UTXOs saves fees. It also permanently records on-chain that those UTXOs were controlled by the same wallet. Most people making this decision are optimising for fees without knowing the privacy cost. Legend's consolidation advisor shows both numbers — fee cost precisely, privacy cost honestly. Not a score. A true statement: "This transaction will permanently reveal these UTXOs are co-owned."

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
**Audience:** Silent Payments users. Privacy-focused Bitcoiners. Wallet developers.
**Dependency:** Legend Silent Payments scanning live.

**The argument:**
BIP-352 Silent Payments allow a sender to pay a static address without that address appearing on-chain. Every payment goes to a unique derived address. This is excellent for privacy. It is also completely unsupported by every public block explorer — because correctly identifying Silent Payments outputs requires scanning every block, which public explorers don't do. Legend scans every block. It is the first explorer to display Silent Payments static addresses and their derived outputs correctly.

**Beats:**
- What Silent Payments are and why they exist
- The scanning problem: why public explorers can't support them
- What Legend does differently
- The batch scanning optimisation: EC point aggregation, ~256x speedup over naive
- Practical use: checking whether a Silent Payment arrived, privately

**CTA:** Use a Silent Payments-compatible wallet. Use Legend to verify receipt.

---

### Article 19 — ZK balance proofs for Bitcoin holders

**Slug:** `zk-balance-proofs-bitcoin`
**Status:** Scoped. Not drafted. Unlocks post-v2.
**Audience:** Family offices. Bitcoin-backed lenders and borrowers. Compliance professionals. Lawyers verifying client holdings.
**Dependency:** Legend ZK balance proof feature live (v2).

**The argument:**
If you want to prove to a lender, solicitor, or counterparty that you control a certain amount of Bitcoin, you currently have two options: show them your addresses (permanent privacy loss) or use a custodian (defeats self-custody). ZK balance proofs give you a third option: cryptographic proof that you control at least X BTC, without revealing which addresses. Legend generates the proof. The verifier checks it. No addresses exchanged.

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
Private Information Retrieval is a branch of cryptography that asks: can a server answer a query without learning what was asked? Previous PIR schemes required multiple non-colluding servers. Spiral PIR (MIT, 2022) achieves this with a single server and is fast enough for production use. Legend implements Spiral PIR for UTXO lookups. The server returns your answer. Its logs reveal nothing about what you asked. This is a world first for a production Bitcoin block explorer.

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

## Publishing sequence

Articles publish in this order, each unlocking when its dependency is met:

| Order | Article | Dependency |
|-------|---------|------------|
| 14 | The metadata leak nobody talks about | Legend live |
| 15 | From Chaum to Satoshi to Legend | Legend live, Article 14 published |
| 16 | Why Mempool over Tor isn't enough | Legend live |
| 17 | The UTXO consolidation problem | Legend v1 UTXO advisor live |
| 18 | Silent Payments and why no explorer supports them | Legend SP scanning live |
| 19 | ZK balance proofs for Bitcoin holders | Legend v2 live |
| 20 | Spiral PIR and Bitcoin privacy | Legend v2 PIR live |

Articles 14, 15, and 16 can publish in close succession at Legend launch.
Articles 17 and 18 follow as features confirm stable.
Articles 19 and 20 are v2 unlocks — no timeline pressure.

---

*Next article session: post-B9, once Legend infrastructure exists to back the claims.*
*"Nothing stops this train."*
