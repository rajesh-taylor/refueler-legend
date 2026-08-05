# legend-scope.md — refueler-multi-core
> **Version:** 1.1 | **Created:** Multi-5 · 3 Aug 2026 | **Updated:** Ad-hoc · 5 Aug 2026
> Locked product scope document for Legend. Defines what is in scope, out of scope,
> and deferred by version (v1, v2, v3) across chains, protocols, query types,
> privacy primitives, and professional use cases.
> Load in every build session. Nothing builds without a version reference here.
> Companion documents: `legend-design-spec.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Scope lock principles

**One question governs every decision in this document:**
Does this feature serve a real user at the distress moment, or a professional user
doing their job privately — without telling us what they're looking at?

If yes and it can ship without undermining the privacy architecture: consider.
If it requires compromising ephemeral sessions, storing query data, or trusting a
third party with query content: defer or drop.

**Sequence logic:**
v1 ships a working, honest, private explorer. It does not ship everything.
v2 adds the cryptographic rigour that makes the privacy claims mathematically exact.
v3 adds the ecosystem contributions and federated infrastructure that make Legend
the category-defining tool.

Nothing in v2 or v3 is permitted to break the v1 architecture. All versions share
the same query interface, design tokens, and privacy guarantees.

---

## What Legend is (and is not)

**Is:** A privacy-first Bitcoin mainchain block explorer with a progressively expanding privacy and professional analysis layer. Every query is private by default. The free tier is unlimited. The Enterprise tier is the institutional wrapper around open source.

**Is not:**
- A general-purpose multi-chain explorer (Ethereum, Solana, etc. — never in scope)
- A surveillance tool or chain analytics service (UTXO provenance scoring is
  for the holder querying their own holdings, not for third-party analytics)
- A custodian or wallet (no private key handling, no transaction signing)
- A Nostr relay or social product (Nostr integration is ecosystem contribution only, not a core Legend feature)
- A clone of Mempool.space with a privacy badge — that is the starting point,
  not the destination

---

## Version 1 — Working private explorer (post-B9, target: December 2026)

### Chains and networks

| Protocol | In scope | Notes |
|---|---|---|
| Bitcoin mainchain | **Yes** | Full UTXO set, transaction history, block data |
| Bitcoin testnet | No | Not a priority. Self-hosters can configure if they want. |
| Lightning Network | **Partial** — on-chain only | Channel opens and closes visible on-chain. No off-chain channel state. |
| Liquid | No | Deferred to Phase 4 (v2 adjacent) |
| CoinJoin | **Partial** — detection only | Flag transactions matching CoinJoin heuristics. No de-mixing. |
| Silent Payments (BIP-352) | **Yes** | Full: static address display, per-block scanning, derived output listing |
| Cashu | **Yes** — as infrastructure | Query credentials only. Cashu tokens are not chain-queryable in v1. |

### Query types in scope (v1)

- Bitcoin address → UTXOs, balance, transaction history
- Transaction ID → inputs, outputs, fee, confirmations, block
- Block height or hash → block metadata, transaction list
- Silent Payments static address → derived outputs from block scanning
- Batch address query (up to 50 addresses) — breach scenario support

### Query types out of scope (v1)

- Raw script / scriptPubKey queries — deferred
- xpub / HD wallet scanning — deferred (significant privacy complexity)
- Lightning node pubkey → channel history — deferred to Phase 4
- Liquid confidential transaction queries — deferred to Phase 4
- Mempool fee estimation (full) — basic fee rate display only in v1

### Privacy architecture in scope (v1)

| Primitive | v1 status | Notes |
|---|---|---|
| Ephemeral sessions | **Yes** — locked | No cookie, no session token, no cross-query linkage |
| Cashu query credentials | **Yes** — infrastructure present | Free tier: unlimited. Enterprise: unlimited. No credential gate at v1 launch. |
| PIR-inspired sharding | **Yes** — v1 approximation | 3–5 Hetzner nodes, query split at API gateway. Not full Spiral PIR — that is v2. |
| Tor-native API | **Yes** — Enterprise only | Hidden service for Enterprise endpoint. Not exposed to free tier in v1. |
| Proof-of-query receipts | **Partial** — v1 stub | Blind receipt issued. Verification tooling is v2. |
| No server-side query logs | **Yes** — locked | Structural, not policy. Ephemeral sessions make reconstruction impossible. |
| IP privacy | **Partial** | Tor for Enterprise. Free tier: standard HTTPS, IP visible to server. Documented honestly. |

### Professional use cases in scope (v1)

**Breach response (primary launch use case):**
Batch query up to 50 addresses privately. Results: `Intact` or `Activity detected`.
No account. No friction. Targets Coldcard/Coinkite breach scenario and any future
hardware wallet incident. This is the distress moment feature. It must be zero friction.

**Self-custody monitoring:**
Private address balance and UTXO check. No account. No logs. Replaces Mempool.space
for individual holders checking their own addresses.

**Sparrow Wallet / custom Esplora endpoint:**
Legend is API-compatible with Esplora from electrs lineage. Sparrow users paste
Legend's endpoint URL into Server preferences. No plugin, no custom code.
Instruction article ships with the Phase 1 build. Craig Raw approach deferred to post-B9.

**UTXO consolidation advisor (qualitative):**
Single informational flag when an address has 5+ UTXOs:
`Consolidating these UTXOs will permanently record co-ownership on-chain.`
Plain statement. No score. No recommendation. Honest scope — the flag is qualitative in v1.

**Denomination toggle:**
Sats / BTC / USD (at time of transaction). Session-persisted. Resets on new session.
No localStorage. Covers basic professional use where fiat equivalence is needed.

### Professional use cases out of scope (v1)

- UTXO provenance scoring (quantitative) — deferred to v2
- ZK balance proofs — deferred to v2
- Compliance report generation — deferred to v3
- Family office credential management — deferred to v3
- Insurance pricing layer — v3 or beyond
- Estate / solicitor verification tooling — v3 or beyond
- Bitcoin-backed lending integration (Ledn, Unchained, Lava) — v3 or beyond

### What ships at v1 launch alongside the explorer

- Article 14: `what-your-block-explorer-knows-about-you`
- Eleventy static landing page at `refueler.io/legend`
- Onboarding modal (first visit)
- Privacy explainer modal (per result)
- Batch query flow (breach scenario)
- Credential status icon (confirms: private queries active)
- Paper/Carbon theme toggle (rs-theme cookie)
- Open source repo with MIT licence

---

## Version 2 — Cryptographic rigour (Phase 2–3, estimated 2027)

v2 makes the privacy claims mathematically exact. v1 claims are honest but
architecture-level. v2 makes them formally verifiable.

### Cryptographic upgrades (v2)

| Primitive | v2 status | Notes |
|---|---|---|
| Spiral PIR (MIT 2022) | **Yes** | Replaces v1 sharding approximation. Single-server PIR for UTXO lookups. |
| Path ORAM-compatible indexing | **Yes** | Access pattern hiding. Prevents server from inferring query from access pattern. |
| ZK balance proofs | **Yes** | Groth16 or PLONK on bellman or arkworks. Pedersen commitments underneath. |
| Proof-of-query receipt verification | **Yes** | Full client-side verification tooling. Completes the v1 stub. |
| Homomorphic aggregate queries | **Experimental** | Batch query aggregation without revealing individual addresses. If viable. |

### Protocol expansions (v2)

| Protocol | v2 status | Notes |
|---|---|---|
| Lightning channel state (partial) | **Yes** | On-chain correlation with own node. Not off-chain routing. |
| UTXO provenance scoring | **Yes** | Quantitative. For the holder querying their own UTXOs. Not third-party analytics. |
| Stablesats / DLC recognition | **Yes** | Identify DLC-settled outputs and stablesats-related patterns on-chain. |
| CoinJoin (extended) | **Yes** | Wasabi, JoinMarket, Whirlpool pattern recognition. No de-mixing. |
| xpub / HD wallet scanning | **Evaluate** | Privacy complexity is significant. Evaluate during Phase 2 planning. |
| Lightning node pubkey → channel correlation | **Yes** | Holder's own node only. Private. Nothing leaves the session. No other explorer supports this. |
| Cashu mint health → reserve verification | **Yes** | On-chain Lightning liquidity footprint only. Mint blindness preserved. Mint does not know it was checked. |

### Professional use cases (v2)

**ZK balance proof generation:**
A holder generates cryptographic proof they control ≥ X BTC without revealing addresses.
Use cases: Bitcoin-backed lending (Ledn, Unchained, Lava), estate verification for solicitors,
compliance demonstration to legal team. No addresses exchanged. Verifier checks the proof.

**UTXO provenance scoring (quantitative):**
Holder queries their own UTXOs and receives a scored assessment: mixing history,
coinbase distance, known-service interaction. For compliance professionals receiving
BTC who need to understand what they're holding. Credential-gated. Never revealed
to operator. Never used for third-party surveillance.

**Lightning channel correlation:**
Private correlation of on-chain channel opens/closes with own node's channel history.
Requires the holder to provide their own node pubkey. Nothing leaves the session.

**Lightning node pubkey — private channel correlation:**
Holder provides own node pubkey. Legend correlates on-chain channel opens/closes
with node footprint privately. Nothing leaves the session. No other explorer
supports this at all, let alone privately.

**Cashu mint health verification:**
Private on-chain verification of a Cashu mint's Lightning liquidity footprint.
Query input: mint public key + block height. Output: channel open/close history,
reserve consistency check. Does not query token state — mint blindness is preserved.
The mint does not know it was checked. Positioned alongside the Cashu ecosystem
as a trust and health signal, not as surveillance.

**Article pipeline unlocks at v2:**
- Article 19: `zk-balance-proofs-bitcoin`
- Article 20: `spiral-pir-bitcoin-legend`

---

## Version 3 — Category definition (Phase 7–8, estimated 2028–2029)

v3 is the tool that gets cited in regulatory working groups. It requires everything
in v1 and v2 to be stable, audited, and trusted before a line of v3 code is written.

### Protocol expansions (v3)

| Protocol | v3 status | Notes |
|---|---|---|
| Liquid Network | **Yes** | Confidential transactions, L-BTC, Liquid assets. Requires separate indexer. |
| Silent Payments BIP-352 contribution | **Yes** | Contribute Legend's batch scanning optimisation upstream to the BIP. |
| Federated chain analytics | **Yes** | Multiple Legend nodes pooling analytics without sharing raw query data. |

### Cryptographic architecture (v3)

| Primitive | v3 status | Notes |
|---|---|---|
| Camenisch-Lysyanskaya credentials | **Yes** | Enterprise credential attributes — role, clearance, access scope — without revealing identity. |
| Federated Cashu mint coordination | **Evaluate** | Multiple mints issuing Legend credentials. Evaluate post-v2 audit. |

### Professional use cases (v3)

**Compliance report generation:**
Credential-gated, client-side generated. PDF report of address history, provenance scores,
and balance proofs for a given date range. The operator never sees the content.
For solicitors, accountants, family office trustees, compliance officers.
This is the tool that PI insurance accepts.

**Family office credential management:**
Enterprise tier with named credential sets, access controls by role, audit trail
the family office owns and controls. Not logged by Legend. Documented honestly.

**Insurance pricing layer:**
API for insurers pricing BTC holdings. UTXO provenance scores and mixing history
provided without the insurer knowing which client's addresses they're pricing.
ZK balance proof integration.

**Estate / solicitor verification tooling:**
Formal integration pathway for UK solicitors probating BTC estates. Verification
of holdings at a specific block height. ZK balance proof as court-admissible evidence.
Depends on legal framework development outside Legend's control — scope is the
technical tooling, not the legal acceptance.

**Self-custody estate planning:**
Time-locked balance verification at a specific block height for probate purposes.
A solicitor needs to know what the deceased held at date of death — a specific block. Legend returns a ZK-backed statement of holdings at that block without revealing which addresses were checked. Inheritance script monitoring for Miniscript/Liana-style time-locks: holder or heir pastes the script, Legend watches privately. Multi-sig quorum verification in plain language for solicitors. Per-report pricing model — £50–150 per verified estate report. UK solicitors are the primary audience; the Solicitors Regulation Authority has mandated crypto asset accounting in estates with no tooling provided. Legend is that tooling.

**/legend/verify endpoint:**
Dedicated ZK balance proof verification flow. Separate URL, separate interaction —
not a modification to the main explorer input. Input: proof + claimed amount.
Output: verified or not verified against chain at specified block height.
Used by lenders, solicitors, and counterparties to verify a holder's proof
without seeing addresses. Article 23 CTA links here.

**Bitcoin-backed lending (formal integrations):**
Ledn, Unchained, Lava — ZK balance proof as collateral verification. No addresses
exchanged with the lender. Lender verifies the proof against the chain.
Formal partnership approach begins post-v2 audit.

**Academic and regulatory contribution:**
External cryptographic audit of PIR and ZK implementations. Co-authored academic
paper on privacy-preserving chain analytics. Engagement with regulatory working
groups on Bitcoin privacy standards. London BitDevs as the primary network.

---

## Permanent out of scope (any version)

These are never in scope. Not deferred. Not reconsidered without a full session.

| Item | Reason |
|---|---|
| Ethereum, Solana, or any non-Bitcoin chain | Legend is Bitcoin infrastructure. Adding alt-chains dilutes the positioning and multiplies infrastructure cost. The category is defined by depth, not breadth. |
| Third-party chain analytics surveillance | UTXO provenance scoring is for the holder. It is never exposed as an API for tracking other people's addresses. This is the line that separates Legend from Chainalysis. |
| Private key handling or transaction signing | Legend does not touch private keys. Not a wallet. Not a signing device. Never. |
| Server-side query storage | Not even for debugging. Ephemeral sessions are non-negotiable. If a debug requirement arises, it is satisfied client-side. |
| External video or analytics platforms | No YouTube, Vimeo, Google Analytics, Mixpanel, or equivalent. Applies to the Legend surface and all article pages. |
| Advertising or data monetisation | Legend's business model is Enterprise contracts. Not data. Not advertising. The data is not ours to monetise — structurally impossible by design. |
| Account creation for free tier | No account. No email. No identity. Free tier is anonymous by default. This is the only honest position for a privacy product. |
| Claiming "anonymous" for Lightning payments | Lightning payments are pseudonymous. Blink internal correlation possible. Documented honestly. Never claimed as anonymous. |

---

## Scope creep rules

**Before adding anything to any version:**
1. Does it serve the distress moment user or the professional user?
2. Does it require storing or correlating query data?
3. Does it require trusting a third party with query content?
4. Does it introduce a cross-product dependency that creates friction?

If 1 is no, or if 2–4 are yes: the answer is no. Bring it to a planning session if it needs revisiting. Do not build first and rationalise second.

**The Share lesson:** The Share-upload-to-Legend-queries link was a good idea on paper and was dropped because it introduced cross-product friction. That is the correct outcome. Apply the same discipline here.

---

## Version summary

| Area | v1 | v2 | v3 |
|---|---|---|---|
| Bitcoin mainchain | Full | Full + provenance | Full + federated |
| Lightning | On-chain only | Channel correlation | — |
| Liquid | — | — | Full |
| Silent Payments | Full scan | — | BIP contribution |
| CoinJoin | Detection flag | Extended recognition | — |
| PIR | Sharding approximation | Spiral PIR | — |
| ZK proofs | — | Balance proofs | Credential architecture |
| Cashu credentials | Infrastructure | — | Federated mints |
| Breach response | Yes | Yes | Yes |
| UTXO advisor | Qualitative flag | Quantitative score | — |
| Compliance reporting | — | — | Yes |
| ZK balance proofs | — | Yes | Lending/estate integration |
| Family office tooling | — | — | Full |
| Academic contribution | — | Audit input | Co-authored paper |
| Lightning node correlation | — | Own node, private | — |
| Cashu mint health | — | Reserve check, blind | — |
| Estate planning tooling | — | — | Probate verification, time-lock monitoring |
| /legend/verify endpoint | — | ZK proof stub | Full verification flow |

---

## Carry-forward

`legend-scope.md` is the authority document for every build session.
If a feature is not listed here, it does not exist yet. Add it to a planning session first.

**Pre-build harness sessions (before Multi-8 / Legend-0 build opens):**
- Node infrastructure costing session — produces `legend-node-plan.md`
- Enterprise pricing session — produces `legend-enterprise-pricing.md`
- UX language and information hierarchy session — produces `legend-ux-language.md`
- Node status page spec session — adds section to `legend-design-spec.md`
- Silent Payments plain-language copy session — adds to `legend-ux-language.md`
- Estate planning feature spec session — adds detail to v3 section above

**Session naming:** Multi-[n] continues through Multi-8 (first query flow).
From Legend-0 onwards, sessions are prefixed `Legend-[n]`.

---

*"Nothing stops this train."*
