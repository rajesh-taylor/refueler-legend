# SESSIONS.md — refueler-multi-core
*Rolling log — last 3–4 sessions only. Archive older entries.*

---

## Session 3 — Multi-3 · 3 Aug 2026

**Status:** Strategic planning — Legend economics, cryptographic foundations, architecture

### Completed

- Pleb tier query model resolved: 21 sats for 100 queries/day via Lightning (LNURL/Bolt12)
- Share-upload-to-Legend-queries link removed — was cross-product friction, not a reward
- Enterprise unlimited confirmed — PIR-sharded, Tor-native, Cashu credentialed
- Licence question closed: MIT stays, upstream fork compatibility non-negotiable
- Sparrow Wallet: custom Esplora endpoint, no plugin required, approach Craig Raw post-B9
- Sat rewards (170–2000 sats) confirmed Lightning-only, no on-chain, no Legend integration needed
- Blink/POS integration: not a Legend feature, clean separation confirmed
- Article pipeline architecture resolved: legend-articles-list.md lives in multi-core,
  built articles (HTML/NJK/styling) live in refueler-io — mirrors Share pattern
- Eleventy: landing page at refueler.io/legend only. Explorer interface is vanilla JS SPA.
  User sees no seam. Legend consumes existing Refueler nav and footer components.
- Paper as default theme. Carbon toggle. rs-theme cookie persists across all Refueler surfaces.
- Legend wordmark: product surface under Refueler design system, no separate identity yet.
- Modals confirmed useful: onboarding, credential status, batch query results.

### Cryptographic foundations locked

**The lineage:** Chaum 1982 → DigiCash 1989 → Bitcoin 2009 → Cashu 2022 → Legend 2026

- Chaum blind signatures (1982): direct ancestor of Cashu NUT-00 and Legend query credentials
- Pedersen commitments (1991): primitive for ZK balance proofs and UTXO set commitments
- Camenisch-Lysyanskaya credentials (2001): long-term architecture for Enterprise credential attributes
- Spiral PIR (MIT, 2022): single-server PIR for UTXO lookups — server returns answer without
  learning the query. Mathematically provable, not architectural promises.
- Path ORAM: access pattern hiding, v2 target, index structure should be ORAM-compatible from v1
- ZK balance proofs: prove control of ≥ X BTC without revealing addresses. Lending and
  compliance use case. Groth16/PLONK on bellman or arkworks.
- Federated chain analytics: v3. Privacy-preserving UTXO clustering. No individual data exposed.
- Silent Payments batch scanning optimisation: batch-verify outputs using EC point aggregation,
  ~256x speedup over naive. Active research gap — Legend contribution to BIP-352 ecosystem.

### Build sequence confirmed

**v1 (post-B9):**
- PIR-inspired sharding (3–5 Hetzner nodes)
- Cashu query credentials (21 sats / 100 queries, bot deterrent not monetisation)
- Ephemeral sessions, no cookies, no client correlation
- Silent Payments native (BIP-352)
- Tor API for Enterprise
- Fee estimation (Mempool parity, required at launch)
- UTXO consolidation advisor: qualitative privacy flag only ("merging reveals co-ownership,
  permanent on-chain record") — no score, no heuristic. Honest and accurate.
- Batch address monitoring (breach scenario — newline-separated input, single credential burn)
- Carbon default option, Paper default on arrival from refueler.io, theme persists via cookie
- Denomination toggle: sats / BTC / USD at time of transaction

**v2:**
- Spiral PIR for UTXO lookups (genuine single-server PIR)
- Path ORAM-compatible index layer
- ZK balance proofs for compliance and lending
- UTXO provenance scoring (privacy-aware, replaces qualitative flag)
- Stablesats / DLC recognition (Blink, Wallet of Satoshi hedging fingerprints)
- Homomorphic aggregate queries (batch balance without address disclosure)

**v3:**
- Federated chain analytics model
- Silent Payments batch scanning contribution to BIP-352
- CL credential architecture for Enterprise attribute proofs

### Mempool / Blockstream parity checklist (must match at v1 launch)

- Fee estimation with block targets (next block, 3 blocks, 1 hour, 1 day)
- Mempool depth visualisation
- RBF / CPFP tracking
- Lightning network aggregate stats
- Block visualisation (tx size and fee rate within block)
- Full transaction history per address
- UTXO set per address
- Raw transaction broadcast
- Electrum server compatibility (inherited from electrs lineage)

### UI principles confirmed

- Query input is the first element on the page. No hero above the fold on the explorer.
- Three result states: funds intact (calm, precise), funds moved (immediate, timestamped),
  address not found (explain why — may be Silent Payments requiring scan).
- Determinate progress indicator for PIR reassembly: "Querying shard 1/3… 2/3… assembling"
- No loading spinners implying uncertainty.
- Tabs for navigation but not too many. No 3D visualisation. No garish colours.
- Faster, cleaner, less cluttered than Mempool. Information density is a choice not a default.
- Modals for: onboarding, credential status, batch query input/results, privacy explainers.

### The Coldcard framing (broader than one breach)

Legend is a long-term play. Coldcard is the origin story, not the product definition.
The goal: defacto private Bitcoin query engine for plebs, Lightning node operators,
Cashu wallet users, atomic swap participants, family offices, and Enterprise API clients.
"Own bank now" — the pleb and the family office are the same person at different scales.

**Wedge vs Mempool-over-Tor:** "Tor hides who you are. Legend hides what you asked.
You need both — we give you the second one."

### Article pipeline — legend-articles-list.md to be created in this repo

Candidates from this session:
1. Why Mempool over Tor isn't enough — query unlinkability gap, Spiral PIR
2. What we're building and why — Legend origin, Coldcard firestorm, structural metadata leak
3. Spiral PIR and Bitcoin privacy — single-server PIR, world first for production explorer
4. The UTXO consolidation problem — privacy cost of merging, fee timing, privacy-aware advisor
5. Silent Payments and why no explorer supports them correctly
6. ZK balance proofs for Bitcoin holders — proving reserves without disclosure
7. Article 15: From Chaum to Satoshi to Legend — the 44-year arc of financial privacy

All articles build in refueler-io. This file tracks scope only.

### Carry-forward to Multi-4

- Multi-4: Legend UI/UX design spec — query flow, result states, breach scenario design
- Multi-5: Cashu credential architecture — issuance, spend, top-up without accounts
- Multi-6: Article 14 draft
- legend-articles-list.md to be created in this repo (next session or dedicated pass)
- notes-articles-list.md in refueler-share to be updated with Legend article candidates
  (cross-reference only — built in refueler-io)

---

## Session 2 — AP-7 ad-hoc · 2 Aug 2026

**Status:** Strategic planning — Legend scope defined

### Completed
- Legend scope locked: privacy-first Bitcoin block explorer built on BLAKE3 Esplora foundation
- Product name confirmed: **Legend** (not Blind, not BlindChain — wrong register)
- URL confirmed: `refueler.io/legend` — not a separate domain
- Licence confirmed: MIT stays (matches upstream, fork compatibility)
- REFUELER-BRIDGE.md updated with full Legend section — placed in repo root
- CLAUDE.md updated to v1.1 reflecting dual scope (ARM/BLAKE3 + Legend)
- README.md extended with Legend privacy layer section

### Key decisions locked (AP-7)

**Architecture:**
- PIR-inspired query sharding: 3–5 Hetzner nodes, fixed cost regardless of client count
- Ephemeral query sessions — no cookies, no session tokens, no client correlation
- Cashu blind-signature query credentials — same infrastructure as Share
- Tor-native API for Enterprise
- Silent Payments (BIP-352) native support — first explorer to do this correctly
- Proof-of-query receipts — blind cryptographic proof per query

**Query credit model:**
- Free: 21 sats for 100 queries/day
- Enterprise: unlimited, full PIR + Tor stack

**Who Legend serves:**
- Plebs first — free tier, no account, privacy as default not premium
- Lightning/Cashu wallet users — private channel activity tracking
- Family offices (UK + US) — private address monitoring, compliance reporting
- Enterprise — full stack, self-hosting option

**The Coldcard moment:** Coinkite breach (Aug 2026) exposed customer addresses.
Checking swept addresses on public explorers compounds the breach. Legend is the
correct tool — private address monitoring, no metadata leak. Article 14 timing
is significant. Write now, publish when infrastructure is live.

**Sparrow Wallet:** potential partnership post-B9. Already supports custom Esplora
endpoints — Legend is API-compatible out of the box. Approach Craig Raw post-B9
when Legend has something to show.

### Prerequisites before any code
- Share Lightning node live at B9 (non-negotiable)
- B9-plan Legend planning session

### Carry-forward
- No code written. Planning only.
- Next session: Multi-4 — Legend UI/UX design spec

---

## Session 1 — CC-64 · 8 July 2026

**Status:** Initialisation

### Completed
- Repo created at `github.com/rajesh-taylor/refueler-multi-core`
- README.md pushed (commit `f9c4aff`) — 147 lines
- Ecosystem chart corrected: `refueler.io` = commerce platform, not brand funnel
- `.gitignore` added (Rust/Cargo, build artefacts, env, secrets)
- `LICENSE` added (MIT)
- `CLAUDE.md` added
- `SESSIONS.md` added (this file)

### Carry-forward
- No code written yet. README only.
- Session 2 to scope BLAKE3 integration points and ARM build targets

---

*Next session: Multi-4 — Legend UI/UX design spec*