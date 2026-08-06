# legend-competitor-research.md

> **Type:** Research input only. Contains no decisions. Do not treat any figure or citation
> here as confirmed until checked against the primary source named in the relevant
> *Needs verification* subsection.
>
> **Provenance:** Compiled from training knowledge, cutoff approximately January 2026.
> No live web research was performed in the session that produced this file. Competitor
> UIs, pricing, API surfaces and feature sets change frequently and may already be stale.
> Academic citations are recalled from memory and may be misattributed or conflated —
> confirm every author/year/venue tuple before use.
>
> **Confidence key used throughout:** `[confirmed]` = stable, load-bearing fact unlikely
> to be wrong. `[directional]` = correct in shape, specific numbers suspect. `[speculative]`
> = plausible but unverified. `[flag]` = a claim seen elsewhere (including in Legend's own
> planning notes) that is overstated or misattributed as written.

---

## Area 1 — Competitor landscape and gaps

### Method note

"Block explorer" spans three distinct product shapes that are worth separating, because
Legend competes with only one of them directly and is *defined against* another:

1. **Full explorers** — address/tx/block lookup with an indexer behind them (Mempool,
   Esplora, btc-rpc-explorer, Blockchain.com, Blockchair).
2. **Dashboards / stat sites** — aggregate on-chain metrics, not primarily address lookup
   (Bitbo, Clark Moody dashboard).
3. **Chain-analytics platforms** — the surveillance layer Legend is positioned against
   (Chainalysis, Elliptic, TRM). Not explorers, but they consume the same data and,
   critically, can consume *the queries made to explorers*.

### 1.1 Mempool.space

- **(a) UI / tabs** `[confirmed]` Blocks and mempool visualisation is the signature view
  (animated projected blocks, mempool goggles). Tabs typically: Dashboard, Blocks, Mining
  (pools, hashrate, difficulty adjustment), Lightning (network explorer), and per-network
  switching (Mainnet / Testnet / Signet / Liquid). Address and tx pages show fee, RBF
  status, CPFP, and a fee/effective-fee breakdown.
- **(b) Data / indexing** `[confirmed]` Own backend (`mempool/mempool`, a Node/TypeScript
  service) sitting on top of a Bitcoin full node plus an electrs-family indexer for
  address/tx history. Self-hostable as a full stack; also ships as an Umbrel/Start9 app.
- **(c) Privacy posture** `[directional]` Offers a `.onion` service. Documentation
  encourages self-hosting for privacy. No claim that the *hosted* instance is private —
  the honest reading is that the hosted site sees what you look up.
- **(d) What the hosted version observes** `[confirmed]` Querying IP + the address/tx/block
  you requested + timing + referrer. Standard web-server observability; no special privacy
  engineering on the hosted path.
- **(e) Lightning / Silent Payments** `[confirmed]` Mature Lightning *network* explorer
  (nodes, channels, capacity). **No Silent Payments support** as of cutoff `[directional]`.
- **(f) Mobile UX** `[confirmed]` Strong. Responsive, and a dedicated mobile app exists.
  Probably the best mobile experience in the category.
- **(g) Commercial model** `[directional]` Open source (mixed licensing — check per-repo).
  Commercial angle is the **Mempool Accelerator** (paid transaction acceleration via mining
  pool partnerships) and an enterprise/API offering. This is a revenue model built on
  *transactions*, not on privacy.

### 1.2 Blockstream Esplora (blockstream.info)

- **(a) UI / tabs** `[confirmed]` Cleaner, more utilitarian than Mempool. Address/tx/block
  lookup, mempool, and a Liquid network toggle. This is the canonical "reference" explorer
  look.
- **(b) Data / indexing** `[confirmed]` `Blockstream/esplora` frontend over the
  `blockstream/electrs` fork (the Esplora API). **This is your upstream.** The Esplora HTTP
  API is the de facto standard other tools (including Sparrow) target.
- **(c) Privacy posture** `[confirmed]` `.onion` available. Self-host encouraged. No privacy
  claim for the hosted instance.
- **(d) What the hosted version observes** `[confirmed]` Same as any explorer: IP + query +
  timing. Blockstream's stated position historically emphasises running your own.
- **(e) Lightning / Silent Payments** `[directional]` Not a Lightning explorer in the
  Mempool sense. No Silent Payments support at cutoff.
- **(f) Mobile UX** `[directional]` Functional, less polished than Mempool.
- **(g) Commercial model** `[confirmed]` The explorer is a shop window; Blockstream's
  commercial products are elsewhere (Liquid, Green, satellite, enterprise infra). The
  **Esplora API is used commercially by third parties**, which is the relevant competitive
  fact for Legend's Enterprise tier.

### 1.3 Bitbo.io

- **(a)–(g)** `[flag]` Bitbo is primarily a **Bitcoin dashboard / stats aggregator**
  (halving countdown, treasuries, on-chain and market metrics) rather than a
  lookup-first block explorer. It does surface a block explorer view, but treating it as a
  head-to-head explorer competitor overstates the overlap. Its competitive relevance to
  Legend is low; its relevance as a *distribution/attention* comparison (the "one page that
  tells you the state of Bitcoin" genre) is moderate. **Confirm current feature set** — this
  product has changed shape over time.

### 1.4 Others worth naming

- **btc-rpc-explorer (Jameson Lopp / Dan Janosik)** `[confirmed]` The privacy-adjacent
  incumbent. Talks to *your own* node, minimises external calls, self-host by default. This
  is the closest thing to an existing "privacy-respecting explorer," but it solves privacy by
  **making it your problem** (run your own). Legend's wedge is privacy *as a hosted service* —
  a different proposition. Worth studying its UX for the self-hoster overlap with your ARM
  target.
- **Blockchair** `[directional]` Multi-chain, feature-rich, offers analytics, "privacy-o-meter"
  scoring, and a paid API/data-dumps business. Explicitly a data-selling business model —
  useful as the anti-pattern.
- **Blockchain.com explorer / BTC.com** `[directional]` Legacy, high-traffic, ad/product
  funnel driven. Weak privacy posture, strong SEO.
- **OXT.me and KYCP.org (Samourai lineage)** `[flag]` These were the notable
  *privacy-analysis* tools (OXT for chain analytics, KYCP "know your coin privacy" for
  scoring an output's privacy). Following the Samourai Wallet founders' April 2024 arrests,
  their status is uncertain and likely offline/abandoned. **Do not cite as live.** Their
  historical existence is relevant: they proved demand for privacy-literate on-chain tooling.
- **mempool-space forks / self-host community** — the self-hosting explorer scene overlaps
  heavily with your ARM/Umbrel/StartOS indexer audience.

### 1.5 What Chainalysis / Elliptic / TRM advertise

Framing: these firms sell **inference over public chain data**, and their capability
increases sharply when they also control **network-level vantage** (their own nodes logging
broadcast IPs) or **querying IPs** (knowing who asked about what).

**Commonly advertised on-chain inference** `[directional]` — confirm against current
marketing:
- Address clustering via the **common-input-ownership heuristic** and change-output
  detection.
- **Entity identification / attribution** (exchange, mixer, darknet market, sanctioned
  entity) via labelled datasets built from KYC leaks, undercover buys, and partnerships.
- **Transaction tracing** across peel chains, and through some CoinJoin/mixer patterns
  (they market "de-mixing" or exposure-through-mixers capabilities — the honest technical
  reality is probabilistic, not certain).
- **Risk scoring / exposure analysis** ("this address has X% indirect exposure to
  sanctioned entity Y").
- **Travel Rule / VASP compliance** tooling (this is TRM and Elliptic's bread and butter).

**Network-level and query-level vantage** `[flag — verify carefully]`:
- There is public reporting and at least one **leaked Chainalysis presentation** describing
  infrastructure (a system name circulated as "Rumker") for **running Bitcoin nodes to
  capture the IP address a transaction was first broadcast from**. This is widely cited but
  rests substantially on a *leaked document*, not on standing public marketing. **Treat as
  reported-but-not-vendor-confirmed.** Do not state it as an advertised feature.
- The stronger, cleaner point for Legend copy: **any explorer you query learns the mapping
  {your IP → the address you care about}**, and if that explorer operator is (or cooperates
  with) an analytics firm, that mapping is far more valuable than anything on-chain. This is
  the metadata-leak thesis in your Article 14, and it stands on its own logic without needing
  the leaked-doc claims.

**Honest limits of these firms** (useful for the "what they *cannot* do" side of Enterprise
conversations) `[directional]`:
- They cannot break ECDLP or read private keys.
- Clustering heuristics produce **false positives**; attribution is probabilistic and has
  been challenged in court.
- Silent Payments, well-used CoinJoin, and good coin-control degrade their heuristics.
- On-chain data alone rarely gives them your IP — **that gap is exactly the surface
  Legend closes on the query side.**

### 1.6 Top five gaps a privacy-first explorer could own

1. **Query privacy on the hosted path.** Every incumbent's answer to "is my lookup private?"
   is "self-host." Nobody offers *hosted* query privacy. This is the category-defining gap.
2. **Correct, native Silent Payments display and scanning.** No mainstream explorer does
   BIP-352 properly at cutoff. First-mover is available. (See Area 2 for how hard "correct"
   actually is.)
3. **Honest, structural privacy language + warrant canary + status transparency.** The
   incumbents either make no privacy claim or make vague ones. A product whose privacy claims
   are *precise and slightly understated* is differentiated on trust alone.
4. **Estate / probate / point-in-time valuation with an audit trail.** No explorer treats
   "what was this holding worth in GBP on this date, with a citable source" as a first-class
   feature. (See Area 4.)
5. **Privacy-native Enterprise/API.** Blockstream and Mempool sell API access with standard
   observability. An API where the *operator's* ability to profile the client is
   architecturally constrained (Tor endpoint, credential unlinkability, no logs) is unclaimed
   ground — assuming the claims survive scrutiny (Area 3).

### Needs verification

- Every "no Silent Payments support" claim for Mempool and Esplora — confirm none has
  shipped BIP-352 since cutoff.
- Mempool Accelerator mechanics, pricing, and mining-pool partners (current).
- Bitbo's current product scope — dashboard vs full explorer.
- OXT.me / KYCP.org live-or-dead status; Samourai case status and its downstream effects.
- **The Chainalysis IP-capture ("Rumker") claim** — source is a leaked presentation, not
  standing marketing. Verify provenance and wording before any use; prefer the
  self-standing metadata-leak argument in copy.
- Current advertised capabilities of Chainalysis Reactor/KYT, Elliptic, TRM — pull from
  their live product pages, not memory.
- Licensing per Mempool sub-repo (mixed).
- Court challenges to clustering/attribution — get named cases if you want to cite the
  "probabilistic, challenged in court" point.

---

## Area 2 — Silent Payments (BIP-352) technical depth

### Mechanic recap (implementer framing)

A Silent Payment address (`sp1…`) encodes two public keys: a **scan** key `B_scan` and a
**spend** key `B_spend`. The sender does an ECDH between the private keys of their *eligible
inputs* and `B_scan`, derives a shared secret, and produces a Taproot (P2TR) output tweaked
to the recipient. The recipient re-derives the same shared secret using their **scan private
key** and the **sum of the sender's input public keys**, then checks whether any P2TR output
in the transaction matches.

The consequence that dominates everything below: **detection requires the scan private key
and per-transaction elliptic-curve work.** There is no address to index and grep for.

### (a) What a precomputed per-block tweak index must contain

For each *candidate* transaction (one with at least one eligible input type and at least one
P2TR output), the index stores enough to let a scanner skip re-deriving from raw inputs:

- The **tweak point** — an EC point, 33 bytes compressed — equal to
  `input_hash · Σ(P_i)`, where `Σ(P_i)` is the sum of the eligible input public keys and
  `input_hash` is the BIP-352 hash over the sorted outpoints and that sum. `[directional —
  confirm exact definition against BIP-352]`
- Enough to locate outputs: the **txid** and the set of **P2TR output pubkeys** (x-only, 32
  bytes each) in that transaction, plus block height/position for reorg handling.
- Optionally, spent-status / cut-through info so a light client can be told an output was
  already spent.

Reference implementations to check the exact shape against: the BIP-352 test vectors, and
the **BlindBit** oracle/client design, which is the most concrete published "serve tweaks +
filters to light clients" architecture I'm aware of `[directional]`.

### (b) Compute cost — with and without the tweak index

- **Without a tweak index** (client reconstructs everything): per candidate tx the client
  must fetch the full tx, identify eligible inputs, fetch/parse their prevout scripts to
  recover input pubkeys, sum them, compute `input_hash`, then do the ECDH. Dominated by
  bandwidth and parsing, plus one scalar-mult-scale operation per tx. Impractical for a
  light client over a wide range.
- **With a tweak index**: per candidate tx the client does essentially **one ECDH (one EC
  scalar multiplication) against the served tweak point**, derives the candidate output key,
  and compares. `[confirmed in shape]`
- **Honest figures** `[speculative — must benchmark]`: order-of-magnitude, a modern CPU does
  tens of thousands of secp256k1 scalar mults per second single-threaded. Candidate
  transactions per block are a *subset* of the ~1,500–3,500 txs/block (only those with
  eligible inputs and Taproot outputs). So per-block scan cost is plausibly **single-digit
  to low-tens of milliseconds of EC work per block** on a server, plus I/O — but the real
  cost is **linear in block count** (see (c)) and in the tweak-download bandwidth. **Do not
  put a specific ms or tx/s number in client materials without benchmarking on your BLAKE3
  electrs fork.** Note BLAKE3 helps the *hashing* around the scan (input_hash, index keys),
  not the EC scalar mult itself, which is the actual bottleneck.

### (c) "Bounded-range scan" in practice, and the UX it implies

Scanning is **linear and forward-only over a block range** — there is no random access, no
"jump to my balance." To check receipts you scan every block from a start height to a tip.

- "Received in the last **3 months**" ≈ ~13,000 blocks → fast, feasible client-side or as a
  quick server-assisted scan.
- "Received **2 years** ago" ≈ ~105,000 blocks → materially heavier; realistically a
  server-assisted or backgrounded scan.

**UX consequence:** the honest interface is **time-bounded scanning with a progress model**,
not an instant balance. Recent = interactive; deep history = a job that runs. This is a
genuine product-shape constraint, not a temporary limitation, and copy should frame it as
"scan a range" rather than implying instant SP balances.

### (d) Displaying an SP output to a user who does NOT hold the scan key

This is the crisp one. **Without the scan key, a Silent Payment output is
indistinguishable from any other Taproot key-path output.**

The explorer *can* show:
- That the output exists, its value, that it is P2TR, and its spend status.

The explorer *cannot* show (without the scan key):
- That the output is a Silent Payment at all.
- Which `sp1…` address it pays.
- Any linkage between the output and a recipient's static address.

Only the scan-key holder can confirm receipt (and only the spend-key holder can spend). So
the "Silent Payments view" for a third party is necessarily **null by design** — which is
itself a privacy feature to state plainly, and a hard limit on what any explorer, including
Legend, can render for a non-owner.

### (e) Fuzzy Message Detection (Beck et al. 2021) as an alternative

- **What it is** `[directional — verify authors/venue]`: FMD lets a recipient hand a server
  a *detection key* so the server flags messages "possibly for you" with a **tunable
  false-positive rate**, without learning for certain which are truly yours. Recalled as
  Beck, Len, Miers, Green, **CCS 2021** — **confirm before citing.**
- **What it offers Legend:** a way to let a node help a client narrow which blocks/txs to
  fully scan **without revealing the scan key**, buying recipient ambiguity.
- **False-positive reality at block volume** `[directional]`: the FP rate is a parameter
  (e.g. `2^-p`). Higher FP = more privacy (bigger ambiguity set) but more junk candidates
  the client must then ECDH-check; lower FP = efficient but leaks. At production block
  volumes the client's extra work scales with `FP_rate × candidate_count`, so there's a
  direct privacy/compute dial.
- **The important caveat** `[flag]`: subsequent analysis (recalled as **Seres, Pejó,
  Burcsi**) argues FMD's practical privacy is **weaker than first hoped**, because an
  observer correlating flagged sets *over time* can narrow recipients statistically. So FMD
  is not a clean win — it's a trade with a known critique. **Confirm the critique citation**
  and treat FMD as "candidate, contested," not "solution."

### (f) Open questions an explorer must resolve before shipping BIP-352

- **Eligible input set:** BIP-352 restricts which input types contribute to the sender's key
  sum (recalled: P2TR key-path, P2WPKH, P2SH-P2WPKH, P2PKH; script-path and unknown types
  excluded). Get this exactly right — it defines which txs are candidates. `[verify against
  BIP-352]`
- **Labels:** BIP-352 labels let one address derive many outputs; the scanner must handle the
  label set.
- **Reorg handling** for the tweak index (invalidate and rebuild affected heights).
- **Serve tweaks vs serve filters:** tweaks are public (no privacy cost to serving them), but
  *which ranges a client requests* can leak intent — argues for range-batching / BIP-158-style
  filters / serving generously.
- **Light-client bandwidth budget** for deep scans, and where the scan runs (client vs
  server-assisted) — ties directly to Legend's no-scan-key-revelation stance.
- **DoS surface** on the scanning/index endpoints.

### Needs verification

- Exact `input_hash` and tweak definitions, eligible-input list, and label handling —
  against the **BIP-352 spec and test vectors** (primary source).
- Any concrete ms/tx-per-second figures in (b) — **must** come from benchmarks on your fork,
  not from this document.
- BlindBit as the reference light-client architecture — confirm it's current and matches
  your intended design.
- **FMD citation** (Beck/Len/Miers/Green, venue, year) and the **Seres/Pejó/Burcsi
  critique** — confirm both before either appears anywhere. These are exactly the kind of
  citation I can get subtly wrong.
- Whether any explorer has shipped SP display/scanning since cutoff (first-mover claim
  depends on this).

---

## Area 3 — Privacy architecture: honest assessment

Assessed claim (from `CLAUDE.md` / `legend-node-plan.md`):
**"collusion-resistant query splitting with blind credential unlinkability."** Your own node
plan already states the honest ceiling; this section stress-tests it as an outside reviewer
would.

### (a) "Collusion-resistant" — precisely

- **What it defeats:** any adversary who controls **fewer than the two nodes involved in a
  given query's role split**, plus anyone acting *after the fact*. A single node — whether
  compromised, subpoenaed, or malicious — sees only its fragment (index node: a script-hash
  *prefix* → a k-anonymity set; data node: opaque row IDs, not the address). Post-hoc
  collusion fails because the linking blinding factors lived only in the browser and were
  discarded.
- **What it does NOT defeat:**
  - **Both nodes in a specific query colluding in real time** (or one entity controlling
    both). Your plan states this explicitly as the v1 ceiling — good, keep it explicit.
  - **A node that logs in real time despite the design.** Splitting constrains what a
    *compliant* node sees; it cannot force compliance.
  - **Global timing/traffic correlation**, especially since all three nodes are operated by
    one person — an adversary observing all three can correlate stage 1 and stage 2 by
    timing regardless of blinding. **This is the sharpest gap and should be named.**
  - **The operator personally.** Per your own node plan: IPA 2016 can compel the operator.
    Geographic node spread protects against orders on *hosting providers*, not against orders
    on *the person*. This is a legal/operational limit, not a cryptographic one, and it caps
    the whole claim.

### (b) "Blind credential unlinkability" — precisely

- **Guarantees:** the Cashu NUT-00 (BDHKE) blind signature means the mint cannot link an
  issued credential to its later use, and no node can link two queries to the same session
  via the credential. Nested re-blinding stops the data node linking stage 2 to stage 1 even
  in collusion with the mint.
- **What one node's logs can still infer** (unlinkability is not invisibility):
  - The **fragment** that node saw (prefix or row IDs), its **timing**, and the client's
    **IP** — unless the client used Tor (free tier is HTTPS by default; IP is visible).
  - That *a* valid credential was presented — just not *whose*.
- **What they cannot infer:** user identity, the full query, or a link to that user's other
  queries — from the credential alone. The re-linking risk is **IP + timing**, not the
  credential. Copy must keep "credential unlinkability" and "IP privacy" as separate claims
  (your copy register already mandates this).

### (c) The Checklist PIR reference — and a citation flag

`[flag — important]` Your planning notes cite **"Checklist PIR (Henzinger et al. 2023)."**
This looks like a **conflation of two different papers**:

- **Checklist** (private blocklist lookups, sublinear online phase, **two-server** model) is,
  to my recollection, **Kogan & Corrigan-Gibbs, USENIX Security 2021** — *not* Henzinger.
- **Henzinger et al., USENIX Security 2023** is, to my recollection, **"One Server for the
  Price of Two" (SimplePIR / DoublePIR)** — **single-server**, LWE-based.

These have **opposite server models** (two-server vs single-server), so the attribution
matters. **Verify both before either appears in any material.** I may be wrong on one, but
they are not the same paper.

On the substance (assuming Checklist = two-server, sublinear-online PIR):
- **What it offers:** an offline/online split giving a **sublinear online phase**, attractive
  for repeated lookups against a large database (its motivating case was Safe-Browsing-style
  blocklists).
- **Mapping two-server security onto three nodes:** two-server PIR assumes **exactly two
  servers that do not collude**. A three-node topology with per-query role rotation is **not
  the same model** and does not automatically inherit the two-server guarantee. Your
  SESSIONS.md already flags the **"controls 4 of 5 nodes"** framing as overstated — that
  instinct is correct. Do not claim Checklist's guarantee for the three-node split without a
  precise mapping, which does not trivially exist.

### (d) The gap between v1 and true PIR — stated honestly

- **True PIR** (single- or multi-server) guarantees the server(s) learn **nothing** about
  which record was retrieved — provably, even in real time.
- **Legend v1 is not PIR.** The index node learns a **prefix / k-anonymity set** (a bounded
  leak, not zero), and the data node learns **row IDs**. That is *partial information*, by
  design, in exchange for being cheap and shippable in vanilla JS.
- **To make a PIR claim** you would need to replace the role-split with an actual PIR scheme
  (your plan names **Spiral PIR, MIT 2022** for v2; SimplePIR/DoublePIR are alternatives).
  The costs to eat: client **hint download**, per-query **linear server work**, and
  **bandwidth** — the "client hint payload size" your own CryptoRoadmap-1 correctly names as
  the real adoption risk. Until then, **"PIR-inspired"** or **"PIR-adjacent"** is the honest
  register; "PIR" without qualifier is not (and your copy register already bans that).

### (e) Is the v1 claim defensible to a sophisticated Enterprise compliance team?

**Yes — if stated precisely and not inflated.** The claim survives scrutiny; overstatement is
the only thing that would sink it. Anticipated questions and the honest answers:

- *"What does each node see per query?"* → Index node: a prefix (k-anonymity set). Data node:
  opaque row IDs. Neither sees the full address; linkage lived only in the browser.
- *"Who operates the nodes?"* → One operator, London. Three providers across DE/FI/IS. **State
  this.** The single-operator fact is the most material disclosure.
- *"Can you be compelled to log or to keep publishing a canary?"* → Providers can be served;
  IPA 2016 can compel the operator. The canary is a strong signal about **provider** orders,
  a **limited** signal about **operator** compulsion. (Straight from your node plan — this
  honesty is an asset.)
- *"What's the collusion threshold?"* → Two specific nodes in one query's split, in real
  time. That is the ceiling; v2 Spiral PIR removes it.
- *"Is this PIR / zero-knowledge?"* → No. It is collusion-resistant query splitting with
  blind-credential unlinkability. PIR is the v2 roadmap.
- *"What happens in degraded mode?"* → With one node down: reduced splitting. With two down:
  single node sees the full query, shown as a plain, non-hidden notice. **"Reduced, not
  secure"** — never call a degraded mode secure.
- *"IP privacy?"* → Free tier is HTTPS; IP visible to contacted nodes; Tor is the user's
  mitigation and documented as such. Enterprise gets a Tor endpoint.
- *"Jurisdiction / data retention?"* → No server-side query logs (architectural, not a policy
  promise). Node jurisdictions named.

The one thing that would **fail** a compliance review: any document that says "PIR,"
"zero-knowledge," "anonymous," or "no one can ever see your query" without the structural
qualifiers. The defensible product is the understated one.

### Needs verification

- **Checklist vs SimplePIR/DoublePIR attribution** (Kogan & Corrigan-Gibbs 2021 vs
  Henzinger et al. 2023) — resolve the conflation in your notes before it propagates.
- **Spiral PIR** (MIT 2022) authorship, exact guarantees, and current client-hint payload
  figures — the adoption-risk number must come from the paper/benchmarks, not memory.
- The precise **k-anonymity set size** implied by your default prefix length — this is a
  concrete, checkable privacy parameter and should be stated with a real number once chosen.
- Whether **timing-correlation across one operator's three nodes** is addressed anywhere in
  the threat model; if not, it should be named as a known v1 limit.
- IPA 2016 specifics (technical capability notices, canary compulsion) — get a UK lawyer's
  read before Enterprise contract language leans on any of it.

---

## Area 4 — Denomination history and estate use cases

Toggle under assessment: **sats · BTC · USD at time of transaction · GBP at time of
transaction.** The interesting engineering + legal question is the last two: historical
fiat valuation with an **auditable source**.

### (a) Historical BTC/USD and BTC/GBP price APIs

`[directional — confirm current terms, all of these change]`

| Source | Coverage | Cost | Fit for legal/estate |
|---|---|---|---|
| **CoinGecko API** | Daily history back to ~2013; granular history gated | Free tier (rate-limited) + paid | Reasonable for daily; check licensing for redistribution |
| **CoinMarketCap API** | Long daily history | Mostly paid for historical | Redistribution/licensing needs checking |
| **CryptoCID / CryptoCompare** | Daily + hourly, long history | Free-ish + paid tiers | Good granularity; verify provenance of early data |
| **Kaiko** | Institutional tick/trade data, deep history | Paid (institutional) | **Best for defensible, exchange-sourced valuation** |
| **Coin Metrics** | Reference rates, community + pro | Free community + paid | Reference-rate methodology is a plus for legal use |
| **Bitstamp API** | Real BTC/USD trades back to ~2011 | Free | **Actual exchange prints** — strong provenance |
| **Blockchain.com charts** | Market price series | Free | Convenience-grade, not clearly citable |
| **Nasdaq Data Link (ex-Quandl)** | Curated crypto datasets | Free + paid | Depends on dataset |
| **Yahoo Finance (BTC-USD, BTC-GBP)** | Daily from ~2014 | Free (unofficial) | Convenient, **not** a defensible legal source |

**BTC/GBP specifically** `[flag]`: direct, deep BTC/GBP history is **thinner** than BTC/USD.
Most sources derive GBP as **BTC/USD × USD/GBP FX**. For estate work this is fine **only if
the derivation and both sources are documented** (which exchange print for BTC/USD, which FX
fixing for USD/GBP). LocalBitcoins had GBP history but is defunct. **Do not present a derived
GBP figure as a native exchange price.**

The honest design implication: for the two fiat columns, Legend should **record and display
the source and method**, not just a number — because that provenance is the entire value in
an estate context.

### (b) Appropriate price precision for estate/probate valuation

`[directional — UK-specific, verify with HMRC guidance + a probate solicitor]`

- UK **IHT** values assets at **open market value at the date of death** (IHTA 1984 s.160).
- For a volatile asset, the defensible approach is a **documented value on the date of
  death from a named, reputable source, using a consistent methodology** — not a claim of
  perfect precision.
- **Daily close is generally sufficient**, provided the source and time basis are recorded.
  Sub-daily granularity is **not** generally required for probate; it can even invite
  argument ("why 14:03 and not 14:04?"). The winning property is **consistency + citable
  source + reproducibility**, not maximal granularity.
- Where a range matters (large estates, disputed valuations), showing **the day's
  high/low/close from a named exchange** is more defensible than a single opaque number.

**Product implication:** for probate mode, "daily close from source X, GBP derived via FX
fixing Y" with an exportable audit line beats a slick single figure.

### (c) UK legal / HMRC standards for point-in-time crypto valuation

`[directional — confirm against the live HMRC Cryptoassets Manual and current guidance]`

- HMRC publishes the **Cryptoassets Manual** (the **CRYPTO…** series). It addresses valuation,
  pooling, and treatment for CGT and IHT. `[verify current section references — I recall a
  CRYPTO22xxx range for valuation but do not trust the exact number from memory.]`
- Recalled principles (**confirm each**): value in **GBP**; use a **consistent, reasonable
  methodology** and a reputable source; for CGT, **s.104 pooling** with **same-day** and
  **30-day ("bed and breakfasting")** matching rules apply to crypto; for IHT, **open market
  value at date of death**.
- There is, to my knowledge, **no single prescribed price feed or fixing** mandated by HMRC —
  which is precisely why a tool that makes the methodology **explicit and exportable** is
  valuable.

**Do not state any specific HMRC manual section number, or any "HMRC requires X" sentence, in
product or client copy without pulling the live guidance.** This is high-liability copy.

### (d) Existing tools and the gap Legend could fill

- **Crypto tax tools** — Koinly, Recap (UK-focused), CoinTracking, Accointing (status?),
  CoinTracker: these do **portfolio-level, transaction-history tax accounting** (gains,
  pooling, reports). They are built around *your own* imported holdings, not around
  *inspecting an arbitrary address at a historical date*.
- **Exchange statements** — good provenance, but only for assets held *on that exchange*.
- **Probate-specific crypto valuation** — thin to nonexistent as a dedicated product.

**The gap:** none of these gives you **address- or UTXO-level, point-in-time GBP valuation
with a citable source and an exportable audit trail**, usable by a solicitor or executor who
does **not** control the wallet. That is a real, narrow, defensible wedge for an explorer with
a historical denomination display — and it dovetails with the "works for the owner"
positioning (here: for the owner's *estate*). It also pairs naturally with the ZK
balance-proof / `/legend/verify` use case in your CryptoRoadmap-3 scope, though **that is a
decision for those sessions, not this document.**

### Needs verification

- Current terms, historical depth, granularity, and **redistribution licensing** for every
  API in the table — especially whether you may **display/derive** their prices in a
  commercial product.
- Whether native BTC/GBP deep history exists anywhere reputable, or whether derivation via
  USD/GBP FX is unavoidable (and which FX fixing to standardise on).
- **HMRC Cryptoassets Manual** — exact current section references for IHT/CGT valuation;
  do not cite section numbers from this document.
- Whether any UK probate court has set an expectation on crypto valuation granularity
  (case law) — a solicitor's input.
- Whether Recap/Koinly or others have shipped an estate/probate-specific valuation feature
  since cutoff (moving target).

---

## Area 5 — Ristretto255 vs secp256k1 in Legend's credential and privacy stack

### Premise correction (load-bearing)

The framing "Ristretto255 eliminates cofactor edge cases and simplifies ZK" is true, but the
**comparison target** matters and is easy to get wrong in copy. Ristretto255 eliminates the
**cofactor-8** edge cases of **Curve25519 / edwards25519**. **secp256k1 is already a
prime-order group (cofactor 1)** — it has no cofactor edge cases to eliminate. So on the
cofactor axis, Ristretto255 brings Curve25519 **up to** the cleanliness secp256k1 already
has; it does **not** surpass secp256k1 there. This is the same correction your SESSIONS.md
already applied to the "secp256k1 not a natural fit for ZK" claim — extend it to the cofactor
claim too. The genuine differences below are about **tooling and bug-class elimination**, not
a cofactor advantage over secp256k1.

### (a) What Ristretto255 concretely offers over secp256k1 — blind sigs + DLEQ

Both are ~128-bit prime-order groups; **BDHKE blind signatures and Chaum–Pedersen DLEQ work
over either with equivalent security**. What actually differs:

- **Encoding canonicity.** Ristretto defines a unique canonical encoding with well-defined
  equality — no "is this in the prime-order subgroup / is this a valid encoding" checks.
  secp256k1's point validation (on-curve check) is also straightforward, so this edge is
  **modest** and mostly meaningful relative to raw 25519. `[confirmed]`
- **Tooling / audit surface — the real difference.** The dalek ecosystem
  (`curve25519-dalek`, `bulletproofs`, `merlin`, `zkp`) is **Ristretto-native, constant-time,
  heavily audited, and batteries-included** for exactly the DLEQ/ZK constructions Legend
  needs. Over secp256k1 you assemble from `k256` / `rust-secp256k1` **plus hand-rolled proof
  code**. The honest "becomes easier" is: **fewer lines of bespoke cryptography to audit**,
  not a stronger primitive. `[directional]`
- **Hash-to-group.** Ristretto has a clean Elligator-based map; Cashu's secp256k1 BDHKE uses
  a specific domain-separated `hash_to_curve`. Neither is broken; the Ristretto path has less
  historical foot-gun surface. `[directional]`

Precise version of the cryptographic difference: **implementation ergonomics and
bug-class elimination, not security level or fundamental capability.** "It's cleaner" unpacks
to "fewer point-validation/encoding bug classes, and a mature ZK library instead of
hand-rolled proofs."

### (b) Ecosystem compatibility cost

- Cashu NUTs (NUT-00 BDHKE and the rest) are **specified over secp256k1**. Existing mints,
  wallets (`cashu-ts`, Nutshell, CDK) and tokens are secp256k1. Diverging to Ristretto makes
  Legend's credentials **non-interoperable** with the Cashu monetary ecosystem — no token
  movement, no shared wallet libraries. `[confirmed in shape — verify no curve-agnostic NUT
  has landed]`
- **The load-bearing fact (from your node plan):** Legend's mint is **non-monetary and
  closed-loop** — query-authorisation tokens only, no ecash value, no intended interop with
  other mints. So the **interop cost is largely theoretical for Legend**: there is nothing
  external to interoperate with. The credentials never leave Legend's own client/mint loop.
- **The real cost of diverging is internal, not ecosystem.** Share's Cashu stack is
  secp256k1. Two curves means **two blinding/DLEQ implementations** to build, audit and
  maintain across Share and Legend, with no code reuse between them. That is the genuine
  trade: **Share-consistency and audit-surface economy vs. tooling quality for Legend's
  future ZK layer.**
- `[flag]` Any claim that "Legend must stay on secp256k1 for Cashu interop" is **overstated**
  for a non-monetary mint. The honest reasons to stay are Share-consistency and one audit
  surface — not interop.

### (c) WebCrypto / Safari — is the acceleration real or theoretical?

- **Neither curve is accelerated by WebCrypto for the operations Legend needs.**
  `SubtleCrypto` exposes fixed algorithms (NIST P-curves, RSA, AES, and in newer browsers
  Ed25519/X25519 sign/DH). It does **not** expose raw group operations — scalar
  multiplication, point addition, blinding — on **either** secp256k1 or Ristretto. Blind
  signatures and DLEQ need those raw ops, so they run in **WASM or JS regardless of curve.**
  `[confirmed — but recheck current SubtleCrypto surface]`
- **Safari specifically** added Ed25519 to WebCrypto relatively recently, but that gives
  Ed25519 **sign/verify**, not the raw edwards25519/Ristretto **group operations** blind
  signatures require. So Safari's Ed25519 support **does not accelerate Ristretto blind
  signatures.** `[directional]`
- **Practical reality:** client-side, Legend uses a JS/WASM library **either way** —
  `@noble/curves` (audited, pure JS, supports both secp256k1 and ristretto255, runs
  identically across browsers including Safari) or a WASM build of `curve25519-dalek` / `k256`.
- **Verdict:** the "WebCrypto / Safari hardware acceleration for Ristretto" benefit is
  **theoretical for this use case — flag it as such.** If a real Safari performance or memory
  delta exists, it is a **library-implementation** difference (noble ristretto vs noble
  secp256k1, or dalek-WASM vs k256-WASM), measurable **only by benchmark** — not native
  acceleration. This is the "Safari memory profile" open item from Legend-5; it stays open
  until benchmarked.

### (d) v1 credential layer, or v2/v3 consideration?

Presented as considerations for the CryptoRoadmap sessions — **not a decision here.**

- **v1 credential layer:** secp256k1 keeps Legend aligned with Share's Cashu stack (shared
  BDHKE/DLEQ code, single audit surface). Ristretto brings **no v1 privacy or security gain**
  — the cofactor benefit is over raw 25519, not secp256k1, and there is no interop to win.
  Forcing Ristretto into v1 buys a **second crypto stack for no v1 benefit.**
- **Where Ristretto earns its place:** the **ZK layer (v2/v3 balance proofs).** Bulletproofs+
  / range proofs and the dalek ZK ecosystem are **Ristretto-native**; that is where "fewer
  bespoke lines to audit" becomes a **material build-risk reduction.** Maps to
  **CryptoRoadmap-3 (ZK architecture).**
- **Mixing is possible but deliberate:** credentials over secp256k1, ZK proofs over Ristretto,
  is feasible — but mixing groups in one system adds its own complexity and should be a
  conscious CryptoRoadmap decision, not a default.
- **Shape for those sessions to decide:** Ristretto reads as a **v2/v3 ZK-layer question, not
  a v1 credential rewrite.**

### (e) Production Rust implementations mature enough to build on now

- **`curve25519-dalek`** — canonical Ristretto255 implementation. Constant-time, widely
  audited, production-mature. Provides `RistrettoPoint` / `Scalar`. **Ready now.** `[confirmed
  — pin version/audit status]`
- **Ristretto-native ZK ecosystem:** `bulletproofs` (dalek), `merlin` (transcripts), `zkp`
  (Schnorr/DLEQ proof macros). Mature. `[directional]`
- **secp256k1 side:** `k256` (RustCrypto, pure Rust) and `rust-secp256k1` (bindings to Bitcoin
  Core's `libsecp256k1` — the most battle-tested secp256k1 in existence). Mature, but the ZK
  tooling around them is **thinner**. `[confirmed in shape]`
- **The honest adoption cost is not curve maturity** — `curve25519-dalek` is more than ready.
  It is that **the Cashu BDHKE primitives do not exist over Ristretto**: you would adapt
  BDHKE + DLEQ to Ristretto yourself. The scheme is group-agnostic so this is
  **straightforward**, but it is **new code needing its own audit**, and that cost lands on
  Legend alone because Share will not share it.

### The "3–5×" figure — do not use

`[flag]` The **"3–5× batch verification latency reduction"** in your Legend-5 note must not
appear anywhere without a benchmark citation. Batch verification is a real technique available
for **both** curves (verifying *n* proofs/signatures together, faster than *n* individual
checks). dalek has **particularly good** batch-verification support — but that is a
**library-quality** point, **not a fundamental Ristretto-over-secp256k1 advantage.** Any
multiplier must come from a benchmark on Legend's **actual** credential/proof workload,
**target hardware** (the x86 nodes vs the browser client), and **browser** (Safari WASM in
particular). No number goes in any material without that benchmark named.

### Needs verification

- Whether current Safari / Chrome `SubtleCrypto` exposes **anything** usable for raw group
  operations on either curve — my read is no, confirm against a current support table before
  stating it.
- `curve25519-dalek` and `@noble/curves` **current audit status and versions** before
  building on either.
- The exact **new-code surface** to port Cashu BDHKE + DLEQ to Ristretto — scope in
  CryptoRoadmap-1 before any build estimate.
- Any real **Safari memory/latency delta** between noble ristretto255 and noble secp256k1 —
  benchmark, do not assert. (The Legend-5 "Safari memory profile" open item.)
- **The "3–5×" multiplier** — replace with a measured figure or delete.
- Confirm **Cashu NUT-00 is still secp256k1-only** and that no NUT has introduced a
  curve-agnostic BDHKE since cutoff.

---

## Global caveats (repeat, because they matter)

- Written from training knowledge, **no live web research** in this session. Everything
  competitor-, price-, and citation-related can be stale or wrong.
- **Academic citations are the highest-risk content here.** I have explicitly flagged at
  least one likely misattribution (Checklist vs Henzinger) that also appears in your own
  planning notes. Confirm every author/year/venue before it reaches copy or a client.
- This document contains **no decisions**. It is input for the relevant build and
  CryptoRoadmap sessions, per the session structure.
