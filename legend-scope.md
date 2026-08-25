# legend-scope.md — refueler-legend
> **Version:** 1.8 | **Created:** Multi-5 · 3 Aug 2026 | **Updated:** Multi-[n] restructure · 25 Aug 2026
> Locked product scope document for Legend. Defines what is in scope, out of scope,
> and deferred by version (v1, v2, v3) across chains, protocols, query types,
> privacy primitives, and professional use cases.
> Load in every build session. Nothing builds without a version reference here.
> Companion documents: `legend-design-spec.md`, `CLAUDE.md`, `SESSIONS.md`.
> Note: carry-forward log removed in Multi-[n] restructure — session history lives in `SESSIONS.md`.

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

### Silent Payments — v1 build requirement

**Precomputed per-block tweak index is a prerequisite for Silent Payments, not an optimisation.**
A naive per-query full-range ECDH scan over one year of blocks takes 1.5–3 core-hours — not shippable. The tweak index precomputes one public tweak per eligible transaction at block ingestion, shared across all future scans. Without it, Silent Payments scanning does not ship.

With the tweak index:
- Per-scan compute: ~8 minutes parallelised on 8 cores (vs 1.5–3 core-hours naive)
- Storage overhead: ~30–60 GB per node — within the 2 TB spec
- Concurrent full-range scans sustainable per node: 5–10

The tweak index build must be complete before Silent Payments is declared v1-ready. Bounded-range scans (user specifies start block) are the default UX; full-range scan is available but documented as compute-intensive.

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
| PIR-inspired sharding | **Yes** — v1 approximation | Five dedicated nodes across five jurisdictions; four active query nodes (A, B, C, D); query split at API gateway. Not full Spiral PIR — that is v2. |
| Tor-native API | **Yes** — Enterprise only | Hidden service for Enterprise endpoint. Not exposed to free tier in v1. |
| Proof-of-query receipts | **Partial** — v1 stub | Blind receipt issued. Verification tooling is v2. |
| No server-side query logs | **Yes** — locked | Structural, not policy. Ephemeral sessions make reconstruction impossible. |
| IP privacy | **Partial** | Tor for Enterprise. Free tier: standard HTTPS, IP visible to server. Documented honestly. |

### Node topology (v1, locked Legend-7)

Five nodes. Four full participants (A, B, C, D) plus one cold standby (E).

| Node | Role | Jurisdiction | Provider |
|---|---|---|---|
| A | Full participant | Germany (EU) | Hetzner, Falkenstein |
| B | Full participant | Luxembourg (EU) | Frantech/BuyVM |
| C | Full participant | Iceland (EEA, non-EU) | FlokiNET, Reykjavik |
| D | Full participant + warm standby | Canada | OVHcloud NA |
| E | Chain-only cold standby | Switzerland | Infomaniak |

**Node D** is a full FROST 3-of-4 participant, fully synced chain and Esplora index, manifest-registered, live from day one. When any of Nodes A–C is lost, D promotes immediately with no sync delay.

**Node E** holds a continuously synced Bitcoin Knots chain only — no Esplora index, no FROST share. Its sole purpose is to be ready to build its Esplora index and join the manifest as the new D when D is consumed. Activation trigger: sub-quorum state (fewer than three full-participant nodes operational) or D consumed.

**FROST threshold:** 3-of-4 across Nodes A, B, C, D. Any three of four shares required to sign. Node E holds no share. Sub-quorum requires three simultaneous full-participant failures — materially less likely than the two-failure sub-quorum floor under the original three-node 2-of-3 scheme.

**Provider selection criteria:** dedicated hardware (no VPS); 8+ cores x86_64, 64 GB RAM, 2 TB NVMe, 1 Gbps unmetered; no shared parent company with any other node's provider; jurisdiction preserves no-single-order-reaches-two-nodes property. US providers excluded (CLOUD Act 2018).

### Cashu NUT status

| NUT | Status | Reason |
|---|---|---|
| NUT-00 (BDHKE blind signatures) | **v1 — locked** | Core credential primitive. Required for all query credential issuance. |
| NUT-01/02 (mint info / keysets) | **v1 — locked** | Vocabulary adopted. Required for mint operation. |
| NUT-06 (mint info endpoint) | **v1 — locked** | Browser session start: confirms mint is live and which window is active. |
| NUT-07 (token state check) | **v1 — locked** | Credential validity verification without account state. |
| NUT-11 (P2PK spending conditions) | **v1 — locked** | Enterprise credentials bound to client public key. Prevents credential theft and replay. |
| NUT-12 (DLEQ proofs) | **v1 — locked** | Browser-side, mandatory. Proves the mint signed honestly without revealing the message. |
| NUT-19 (idempotency) | **v1 — locked** | Prevents duplicate credential issuance on retry. Required for batch query reliability. |
| NUT-29 (batched minting) | **v1 — locked** | Breach scenario: 47 addresses → 47 credentials in one round-trip. Essential. |
| NUT-17 (WebSocket subscriptions) | **Rejected — v1** | Requires persistent connection — incompatible with ephemeral session model. |
| NUT-21/22 (bolt11/bolt12 melt) | **Rejected** | Monetary melt operations. Legend mint is non-monetary. Structurally absent. |
| NUT-27 (clear text memos) | **Permanently rejected** | Would attach human-readable context to a credential — directly undermines unlinkability. |
| NUT-09 (restore signatures) | **Permanently rejected** | Restore requires the mint to hold issuance state — breaks forward secrecy of rotating mint instances. |
| NUT-13 (deterministic secrets) | **Permanently rejected** | Deterministic secrets would allow a compromised client device to reconstruct credential history — the opposite of ephemeral. |
| NUT-28 (P2BK spending conditions) | **v2** | Hardened successor to NUT-11. Enterprise credential binding upgrade at v2. |
| NUT-24 (HTTP 402 payment required) | **v2+** | May gate credential top-up flows if abuse requires throttling. Not active at v1 launch. |

### Merkle proof scope

| Scope | Version | Notes |
|---|---|---|
| Cross-node header fetch + in-browser SPV verification | **v1** | Locked Legend-0a. Browser fetches block header from a different node than the query, verifies inclusion. No server trust required for proof validity. |
| Proof export artefact (downloadable, machine-verifiable) | **v2** | Required for estate solicitor £50 block-height balance statement. Depends on /legend/verify stub. |
| Estate integration — ZK-backed proof accepted by PI insurer methodology | **v3** | Depends on full /legend/verify endpoint and ZK balance proof architecture. |

### Plain-language script rendering

Displaying raw scriptPubKey hex or ASM is correct for developers and wrong for solicitors, accountants, and distressed holders. Plain-language script rendering (e.g. "2-of-3 multisig — requires any two of these three keys to spend") is a **v2 item**. v1 shows script type label only (P2WPKH, P2WSH, P2TR, etc.) with raw script accessible on expand. Plain-language quorum descriptions for multisig and Miniscript policies ship at v2, timed with the professional market layer.

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
Instruction article ships with the Phase 1 build.

**UTXO consolidation advisor (qualitative):**
Single informational flag when an address has 5+ UTXOs:
`Consolidating these UTXOs will permanently record co-ownership on-chain.`
Plain statement. No score. No recommendation. Honest scope — the flag is qualitative in v1.

**Denomination toggle:**
Sats / BTC / USD (at time of transaction) / GBP (at time of transaction). Session-persisted.
Resets on new session. No localStorage. GBP added UC-2 Opus. Denomination defaults vary by mode
— see `legend-copy-index.md` §8.

**UI modes (locked UC-1/UC-2/UC-3/UC-4 Opus):**
Distress Mode (mobile, first query, single address) — inferred, never named.
Stewardship Mode (desktop, 1–2 addresses, no outbound activity) — inferred, never named.
Verification Mode (explicitly invoked via `Create a verification →` on standard/Stewardship
results) — Legend's first explicitly-invoked mode.
Treasury Watch Mode (explicitly invoked via `Create a watch →`) — Legend's second
explicitly-invoked mode. Never appears in Distress Mode.

**Legacy print layout (locked UC-2 Opus):**
`@media print` stylesheet. Paper theme only on print. Full untruncated addresses.
Solicitor-legible chain-of-custody document format.

**Shareable verification links and Recipient/Lender View (locked UC-3 Opus):**
URL-fragment share links (address + reference block, never transmitted to server).
In-browser cross-node SPV on recipient side. Lender View: read-only, chrome-less,
GBP-first, three-reads stillness constraint.

**Watch links and Watch Recipient View (locked UC-4 Opus):**
Same URL-fragment mechanism as verification links; re-runs SPV on every open.
Watch Recipient View: institutional register, movement status as load-bearing element,
canary summary line, no-alert honest-scope footer.

### Professional use cases out of scope (v1)

- UTXO provenance scoring (quantitative) — deferred to v2
- ZK balance proofs — deferred to v2
- Compliance report generation — deferred to v3
- Family office credential management — deferred to v3
- Insurance pricing layer — v3 or beyond
- Estate / solicitor verification tooling — v3 or beyond. Exception: the v1 Legacy print layout (UC-2) must render full untruncated addresses to ensure estate chain continuity — a solicitor may commission a v2 Merkle-verified report from nothing but the holder's v1 printout.
- Formal estate/solicitor verification endpoint (`/legend/verify`) — v3 or beyond
- Bitcoin-backed lending formal integrations (Ledn, Unchained, Lava) — v3 or beyond
- Movement alerting (watch push notification) — v3 only, blind channel, see below

### What ships at v1 launch alongside the explorer

- Article 14: `what-your-block-explorer-knows-about-you`
- Eleventy static landing page at `refueler.io/legend`
- Onboarding modal (first visit)
- Privacy explainer modal (per result)
- Batch query flow (breach scenario)
- Distress Mode, Stewardship Mode
- Verification Mode + Recipient/Lender View
- Treasury Watch Mode + Watch Recipient View
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

**Canonical Merkle-anchored, FROST-signed artefact (locked UC-2/UC-3/UC-4 Opus):**
The v2 verified-holdings statement is shared across four consumers — UC-2 (estate),
UC-3 (loan), UC-4 (watch attestation), UC-9 (Chain Trace Report). Single locked field
format; any change must be checked against all four. Watch attestation variant adds
`watch created at block [height]` line. Commissioning direction: subject-present for
UC-3 and UC-4; verifier-commissioned for UC-2; Recovery coordinator for UC-9.

**Proof of control via BIP-322 (locked UC-3/UC-4 Opus):**
BIP-322 signed-message verification. Legend checks the signature; Legend never
handles keys. BIP-322 only — BIP-137 (legacy; P2WPKH-only; not Taproot-compatible;
not multi-sig-compatible) is never used. In UC-3: at verification generation time.
In UC-4: at watch creation time only. A signature proves control at the moment of
signing — not ongoing.

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

**Cashu mint health verification:**
Private on-chain verification of a Cashu mint's Lightning liquidity footprint.
Query input: mint public key + block height. Output: channel open/close history,
reserve consistency check. Does not query token state — mint blindness is preserved.
The mint does not know it was checked.

**Public-body transparency module (v2 — parked):**
A publicly-funded body uses Legend to publish declared holdings and transaction history
to the public. Requires no new query infrastructure; requires a public-disclosure module.
Not a v1 consideration. Scoping session required before any v2 build begins on this feature.

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

**Compliance report generation, family office credential management, insurance pricing layer, estate/solicitor verification tooling, self-custody estate planning:**
See full detail in v3 section — these are credential-gated, client-side generated professional outputs for solicitors, accountants, trustees, and compliance officers.

**Movement alerting — Treasury Watch (locked UC-4 Opus):**
Watch push notification for movement on a watched collateral address. v3 only.
**Architectural constraint (non-negotiable):** alerting is delivered through a blind,
unlinkable channel (Pass Access-class credential, per UC-9 architecture) or it is not
offered. Any non-blind delivery channel (email, webhook with address) would let Legend
join the recipient's identity to the watched address and destroy the core claim.
Alerting is reactive (post-on-chain-confirmation) and never preventive. NUT-12 DLEQ
mandatory. No redemption-linkable address binding.

**Multi-signatory watch (locked UC-4 Opus):**
Council + developer + solicitor multi-party watch. Requires two-operator milestone
and v3 credential architecture. Part of the council API tier.

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
| Public-body transparency module | — | Parked — scoping session required | — |
| /legend/verify endpoint | — | Stub (estate/loan) | Full verification flow |
| Merkle proof | Cross-node SPV v1 | Export artefact (4 consumers: UC-2/3/4/9) | Estate PI methodology |
| Script rendering | Type label only | Plain-language quorum | — |
| SP tweak index | Build prerequisite (v1) | — | — |
| Distress Mode | Yes | Yes | Yes |
| Stewardship Mode + Legacy print | Yes | Yes | Yes |
| Verification Mode + Recipient View | Yes | + Signed artefact + BIP-322 control | + ZK address-hiding |
| Treasury Watch Mode + Watch Recipient View | Yes (live link, canary summary, no-alert footer) | + Signed watch attestation + BIP-322 control | + Blind-channel alerting (Pass) + Council API + Multi-signatory watch |
| Denomination toggle | Sats / BTC / USD / GBP | — | — |
| Proof of control | — | BIP-322, UC-3 and UC-4 | — |

---

*"Nothing stops this train."*
