# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---
## Session Multi-10 · 11 Aug 2026

**Phase:** Adversarial threat review — uncounted session
**Status:** Complete. `legend-threat-model.md` v1.0 produced and committed (`addd8af`).

### Completed

- **`legend-threat-model.md` v1.0 produced and committed.** Pre-build adversarial threat
  review of Legend v1 transport and query architecture. Three attacker profiles
  (Chainalysis tier, GCHQ/NCA tier, passive network/ISP observer), five attack surfaces,
  three structured questions (what is extractable, what is structurally unprotected, what
  is foreclosed). Commit: `addd8af`, 152 insertions. Prompt: `adversarial-1-opus-prompt.md`.

- **Stage-2 obliviousness identified as the load-bearing correctness check.** The v1 privacy
  claim rests entirely on whether stage-2 fetches the full candidate set (oblivious) or only
  the target row (not oblivious — collapse of the role-split). Must be verified and locked as
  a protocol invariant before query code is written. Minimum bucket size *k* also unspecified —
  must be documented. Both items → Legend-6 opener.

- **IP-join finding: collusion-resistance overstated as currently framed.** Both nodes see
  the same client IP seconds apart. IP+timestamp is default TLS-layer data. The honest v1
  floor is *k*-anonymity within a prefix bucket, keyed to IP. Free-tier distress users are
  the most exposed cohort. OHTTP (RFC 9458) is the natural v2 fix; nothing in v1 forecloses it.

- **Canary signalling gap documented.** Under 3-of-4, single-operator IPA compulsion does
  not lapse the canary — the other three participants keep it valid. "FROST resolves Boltz"
  is half true. Requires honest documentation in Enterprise materials and canary UX copy.
  Blocked on solicitor (Opus-C). Canary semantics → Opus-C / solicitor block.

- **Prospective logging risk under IPA Part 3 documented.** A Part-3 notice can compel
  silent addition of prospective logging. Architecture is not retrospectively reversible
  but can be inverted going forward. Also blocked on solicitor.

- **Nothing forecloses v2 transport improvements.** Full-index-per-node enables Spiral PIR
  additively. Browser-direct role-split does not foreclose OHTTP or per-connection Tor.
  No architectural replacement required for any planned v2 feature.

- **Hardening candidates absorbed into existing sessions — no new standalone block.**
  Three gating items: stage-2 obliviousness + *k* → Legend-6; canary semantics → Opus-C;
  2-of-4 unsignable confirmation → FROST ceremony session (already queued).

- **Two-operator milestone identified.** A second operator in a different jurisdiction,
  holding their own FROST share, is required before the first Enterprise contract is signed.
  Not a build blocker — an operational maturity milestone. Logged as carry-forward.

- **Single-node-first development sequence confirmed.** Node A (Hetzner, ~€77/month)
  is sufficient for the full v1 development stack. Five-node production topology
  provisioned when production privacy claims are made — not before.

- **Refueler IP honesty standard established as platform-wide principle.** Added to
  REFUELER-BRIDGE.md. Applies to all current and future Refueler products (Share, Legend,
  Pass, merchant terminal, ticketing). No product claims anonymity where IP is visible.
  All products recommend Tor for high-sensitivity use. All products plan OHTTP or equivalent
  as a v2 structural fix. This is a competitive advantage that cannot be retrofitted by
  competitors whose architectures were not designed with it in mind.

### Files changed

- `legend-threat-model.md` v1.0 (new file, committed `addd8af`)
- `SESSIONS.md` (this entry)
- `MASTER.md` (threat model block added)
- `REFUELER-BRIDGE.md` (Refueler IP honesty standard added)

### Carry-forward

- **Stage-2 obliviousness + minimum *k* floor** — lock in Legend-6 opener as the first
  output of that session, before any query code is written.
- **Canary signalling semantics** — what the canary does and does not signal under single-operator
  IPA compulsion. Fold into Opus-C / solicitor block. Cannot be resolved without legal input.
- **2-of-4 unsignable confirmation** — verify on real hardware in FROST ceremony session.
  Only one acceptable answer: 2-of-4 cannot produce a valid signature under the 3-of-4 scheme.
- **Two-operator milestone** — second operator, different jurisdiction, own FROST share,
  required before first Enterprise contract is signed. Operational milestone, not build blocker.
- **Nostr relay jurisdictions** — verify hosting jurisdiction of damus, nostr.band, nostr.wine.
  Cannot confirm without web access. Required before canary publication design is finalised.
- **`frost-secp256k1` audit status** — pin to a confirmed-audited version. Required before
  any node goes live.
- **All prior carry-forwards from Multi-9 remain open** (legend-enterprise-pricing.md break-even,
  legend-design-spec.md stale infrastructure figure, legend-ux-language.md §4/§8 GBP edit,
  FROST ceremony session, provider quote replies, solicitor engagement).

---
## Session Multi-9 · 11 Aug 2026

**Phase:** Planning and vision — use case library, session sequencing, B9 gate review
**Status:** Complete. `legend-use-cases.md` v1.0 produced and committed (`6e3eb82`).

### Completed

- **B9 gate formally lifted.** Legend runs its own independent node. Legend v1 has
  no Lightning dependency. The Cashu mint for query credentials is non-monetary and
  requires no Lightning node. B9 (Share Lightning node) is no longer a prerequisite
  for any Legend build or planning session. Decision logged here and in
  `legend-use-cases.md` §B9 gate section and `MASTER.md`.

- **`legend-use-cases.md` v1.0 produced and committed.** Eight civilisational use
  cases, each a full design brief with named human moment, interface implications,
  article candidate flag, and preliminary version assignments. Commit: `6e3eb82`,
  688 insertions.

- **Eight use cases defined (UC-1 through UC-8):**
  - UC-1: The Bradford Inheritance — estate lookup, distress mode, phone
  - UC-2: The Grandparent's Ledger — stewardship mode, legacy print, laptop
  - UC-3: The Bitcoin-Backed Loan — verification export, lender view, tablet
  - UC-4: The Council and the Whale — treasury watch, FROST attestation, desktop
  - UC-5: The Florentine District — civic treasury mode + article (dual output)
  - UC-6: The Hanseatic Federation — federation settlement mode + article (dual output)
  - UC-7: The Block War — human cost calculator, historical fee context + article
  - UC-8: The UTXO Lottery — movement eligibility, trustless draw, cryptographic sketch + article

- **Five cross-scenario design principles locked:**
  1. Multiple principals, same data — context inferred from query shape, not login
  2. Verification as a first-class output — exportable, independently verifiable artefacts
  3. Time and history matter as much as current state — chain as civilisational ledger
  4. Plain language as a technical choice — precision and legibility are not in tension
  5. Silence is information — "Held since block 840,000. No movement detected."

- **Four new article candidates identified:**
  - "The Florentine Protocol" — city-states, talent attraction, SP as Medici stipend envelope
  - "The Hanseatic Protocol" — merchant federation, Lightning settlement, on-chain bill of lading
  - "Who Gets Priced Out" — block war, human cost calculator, global south frame
  - "The UTXO Lottery" — movement velocity as monetary property, trustless draw

- **Opus session structure for use cases locked:**
  - One scenario per Opus session. Maximum two if thematically paired.
  - Natural pairs: UC-1+UC-2 (estate/inheritance device contrast), UC-5+UC-6 (civic/institutional)
  - UC-8 runs solo — half UX session, half cryptographic design
  - Eight standard questions per session (see `legend-use-cases.md` §Opus session structure)
  - Standard outputs: interface spec → `legend-design-spec.md`, copy → `legend-ux-language.md`,
    version assignments → `legend-scope.md`, article outline → `legend-articles-list.md`

- **Session sequence confirmed** — see Session queue below.

- **Design vision discussion logged in `legend-use-cases.md`.** Key framing: Legend is
  infrastructure for how value gets read and trusted at civilisational scale. "Pandora's box
  rather than block explorer" — everything inside is useful. Design backwards from the human
  moment, not forwards from the technology.

- **"Absence is correct" principle named** — a display pattern for wherever privacy-preserving
  technology produces invisible-but-real activity. Show the signal. Explain the silence.
  Applies across Silent Payments display, Fedimint settlement, SP-paid civic stipends.

- **UTXO Lottery cryptographic sketch drafted** — Merkle lineage proof of movement history,
  block hash at predetermined height as entropy source, deterministic winner function
  (BLAKE3 of UTXO outpoint + draw block hash). Full design in UC-8 Opus session.

- **`legend-articles-list.md`** — Articles A, B, C added (queued since Multi-8, now done).
  Four new article candidates (UC-5, UC-6, UC-7, UC-8) added. Publishing sequence updated.

### Files changed

- `legend-use-cases.md` v1.0 (new file, committed `6e3eb82`)
- `legend-articles-list.md` (Articles A, B, C + four new candidates added)
- `MASTER.md` (B9 gate, session queue, use case block updated)
- `SESSIONS.md` (this entry)

### Carry-forward

- **Duplicate `### Carry-forward` heading** in Legend-7B entry (lines 115–116) — remove at
  next session touching SESSIONS.md directly.
- **legend-enterprise-pricing.md** — break-even floor still shows ~£385/month. Update to
  ~£566/month at next session touching that file.
- **legend-design-spec.md** — €96/month figure still stale. Replace with ~€673/month
  planning estimate at next session touching that file.
- **legend-ux-language.md** §4/§8 — GBP denomination edit still not applied. Apply at next
  session touching that file.
- **Design-spec token block** — pre-CC-74 stale `--bg` values still in spec. Fix at next
  design session.
- **FROST 3-of-4 ceremony** — blocked on nodes provisioned. Measure DKG duration on real
  hardware; replace "minutes" placeholder in SLA materials with confirmed figure.
- **Provider quote replies** — expected 10–11 Aug 2026. Reconvene when all five received
  to confirm final cost table and unlock `legend-economics.md` §6 published copy.
- **Solicitor (Opus-C)** — IPA compelled-continuation question. Firms: Bristows, Mishcon
  de Reya, AWO. Blocked until engaged.

---

## Session Legend-7B · 7 Aug 2026

**Phase:** 1 pre-build harness — provider selection and quote dispatch
**Status:** Complete. Provider recommendations locked. Five quote emails drafted.
Files updated: legend-node-plan.md v1.2, legend-economics.md v1.2.

### Completed

- **Provider selection finalised.** Five nodes, five independent legal frameworks:
  - Node A: Hetzner, Falkenstein DE — confirmed, unchanged.
  - Node B: Frantech/BuyVM, Luxembourg — selected. KVM Slice x8 + Storage Slabs.
    Custom quote requested 7 Aug 2026.
  - Node C: FlokiNET, Reykjavik IS — confirmed provider, custom quote requested
    7 Aug 2026 (storage upgrade path, IPMI, bandwidth overage).
  - Node D: OVHcloud NA, Canada — selected. Advance-1, $115/month (~€106, ~£89).
    Quote email sent; storage upgrade path and entity confirmation requested.
  - Node E: Infomaniak, Geneva CH — selected. Cloud VDS €158/month.
    Quote email sent; single-tenant confirmation and storage upgrade path requested.

- **US nodes explicitly rejected.** CLOUD Act 2018 allows US-headquartered providers
  to be compelled to produce data stored anywhere globally. Removes genuine legal
  independence regardless of physical server location. Documented in legend-node-plan.md
  §1 jurisdiction rationale.

- **FHE (Fully Homomorphic Encryption) evaluated and rejected.** 3–6 orders of magnitude
  slower than plaintext for arbitrary key-value store operations on a ~1 TB index. Not
  viable on any realistic hardware in this decade. Spiral PIR (v2 scope) is the correct
  path. FHE noted for CryptoRoadmap-1 research file as a v4+ / unlikely consideration.

- **Storage spec updated.** Floor raised from "2 TB NVMe" to "2×1 TB NVMe minimum,
  2×2 TB preferred where price delta is small." Chain + index total August 2026: ~1.65 TB.
  Projected by April 2028 halving: ~1.9 TB. 2×1 TB (JBOD ~1.85 TB usable) is marginal
  at that horizon. Storage upgrade window: target Q4 2027. All provider quotes include
  explicit upgrade path questions.

- **Planning cost updated.** ~€450/month superseded. New planning estimate: ~€673/month
  (~£566/month) ex-VAT. Increase reflects genuine jurisdiction independence across
  five legal frameworks — premium is structural, not avoidable. "One Enterprise client
  covers years of infrastructure" remains true: family office floor (£1,500/month) covers
  ~2.6 years; ceiling (£3,500/month) covers ~6.2 years.

- **Minimum Enterprise cost-recovery floor updated:** ~£566/month (from ~£385/month).
  legend-enterprise-pricing.md requires update at next session touching that file.

- **SLA reality documented.** At small-client scale: hardware replacement SLA (hours),
  network uptime SLA (99.9%), software stack entirely operator-owned. Architecture
  tolerates node-down gracefully (N-1 = reduced splitting, not outage). IPMI remote
  management requested in all five quote emails — enables OS-level recovery without
  provider support dependency.

- **Five quote emails drafted (Legend-8 session).** One per provider. Each includes:
  exact hardware spec; storage upgrade path questions (in-place feasibility, process,
  downtime, 2×2 TB pricing); IPMI/remote management confirmation; hardware fault SLA;
  Bitcoin/Lightning payment question. Rajesh to send manually; replies expected
  10–11 Aug 2026.

- **B9 clarified.** B9 refers to the Share project Lightning node milestone — the
  prerequisite gate before any Legend build session. Not a Legend milestone.
  *(Gate subsequently lifted in Multi-9 — see above.)*

- **Go-live timeline.** No nodes to be provisioned before: (a) B9 live on Share;
  (b) quote replies confirmed; (c) UK legal sorted; (d) Legend design prototype live
  at refueler.io/legend for Enterprise demo. Prototype costs nothing beyond existing
  hosting — static shell + simulated query flow, no live nodes required for demo.

- **`legend-incident-protocol.md` v1.0 → v1.1** — Opus-B gaps folded in:
  - §3D sub-quorum procedure added: what is impossible at sub-quorum, what remains possible,
    operator priority order (six steps), per-canary time-to-expiry monitoring requirement,
    Node D warm-standby promotion as first restoration path, Node E Esplora index build triggered.
  - §4 DKG-under-attack subsection added: compound failure where DKG boundary arrives during
    active exploit campaign. Stated as a priority decision: query API suspends before DKG begins;
    DKG outranks query-API continuity; this is the one path from scenario D to scenario C that
    architecture does not otherwise prevent.
  - §3C degraded-migration branch added: procedure when a second node fails during a migration
    in progress. Node D and E activation triggers, status-page dual-state requirement, compound-
    incident Enterprise notification rule, prohibition on running migration DKG with unverified
    replacement node.
  - §3A step 4 split into 4a (binary integrity — manifest-verified binary required before
    rejoining) and 4b (index integrity — snapshot must be provenance-confirmed or full resync;
    unverifiable snapshot is an untrusted node, not a shortcut).
  - Channel asymmetry publication definition added to §6: published = FROST-signed + confirmed on
    ≥2 channels including ≥1 off-node channel. Node's own `canary.json` alone does not count.
    Channel-delivery degradation distinguished from canary expiry. Enterprise notifications must
    state which channels were confirmed.
  - FROST 3-of-4 references throughout: all 2-of-3 and three-node references updated. Three-node
    DKG extension window rule updated: 24-hour extension triggered only when ≥2 nodes offline (not
    one). Emergency re-key degraded state updated from 2-of-2 to 3-of-3 transient.
  - Pre-signed statement bank §7 updated: all FROST 2-of-3 signatures → 3-of-4; triple expiry →
    quadruple expiry in §7 #5 and all cross-references; Node E absence from canary set noted as
    structural (not a signal).
  - Revision log: v1.1 entry added.
  - Opus-A notes: sub-quorum N-2 partial-signature open question added. Existing Opus-A notes
    unchanged (solicitor-blocked).
- **`SESSIONS.md`** — this entry, plus archival of session queue (queue remains unchanged in
  position; Legend-7 and 7B entries prepended).
- **`legend-economics.md`** — three-node cost figures replaced with five-node estimates;
  Nodes B/D/E flagged as TBD pending provider session.
- **`legend-scope.md`** — topology references updated three → five nodes; Node D and E defined.
- **`legend-design-spec.md`** — status page privacy modes reviewed against five-node reality;
  "Single node" state updated to reflect sub-quorum context; no other breaks found.

### Files changed

- `legend-node-plan.md` v1.1 → v1.2
- `legend-economics.md` v1.1 → v1.2
- `SESSIONS.md` (this entry)
- `legend-scope.md`
- `legend-design-spec.md`

### Carry-forward

- **legend-enterprise-pricing.md** — break-even figures use old €450/month base.
  Update minimum contract floor from ~£385/month to ~£566/month at next session
  touching that file.
- **legend-design-spec.md** — €96/month figure still stale (carried from Legend-1).
  Replace at next session: "~€673/month planning estimate (~£566/month), five dedicated
  nodes across five jurisdictions and three continents. Confirmed quotes pending."
- **GBP denomination edit** — legend-ux-language.md §4/§8. Still open from Legend-7B.
- **Design-spec token block** — pre-CC-74 stale --bg values. Still open from Legend-7B.
- **FROST 3-of-4 implementation confirmation** — unchanged from Legend-7B carry-forward.
- **DKG ceremony duration** — unchanged from Legend-7B carry-forward.
- **Sub-quorum partial-signature open question** (Opus-A notes) — unchanged.
- **Custom FlokiNET quote** — still open; now formally requested in Legend-8.
- **Provider reply session** — once all five quotes received, reconvene to:
  confirm final cost table, resolve open questions, update both files with confirmed
  figures, unlock legend-economics.md §6 published copy for use.
- **Opus-A (solicitor) items** — unchanged. IPA compelled-continuation question; NDO publication
  exposure; publication-delegate liability; MLAT characterisation. None can progress without counsel.

---

*Legend-1 through Legend-3A archived. Decisions captured in `legend-economics.md` v1.0,
`legend-enterprise-pricing.md` v1.0, `legend-scope.md` v1.2, `legend-design-spec.md` v1.1.*

---

## Session queue

### Immediate — no gate

**Adversarial-1 (Opus, extended thinking)**
v1 transport and query architecture threat model. Attacker model: unlimited budget,
nation-state / Chainalysis tier. Questions: what can they extract from v1? What does
the architecture not protect against? Do any current Legend-6–10 build decisions
foreclose better v2 transport options?
Output: `legend-threat-model.md` (or equivalent). May generate a hardening work block.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-scope.md` + `legend-node-plan.md`

**UC-1 (Opus) — The Bradford Inheritance**
Single scenario. The youngest daughter, phone, distress mode, father's estate.
Eight standard questions from `legend-use-cases.md`. Full interface design for
distress mode and stewardship mode. Device/age split. Solicitor document output.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Legend-6 (Sonnet build)**
Eleventy shell at `refueler.io/legend`. SPA mount point, Paper/Carbon theme wired,
query input idle state. CONTRIBUTING.md. Feeds from UC-1 output if UC-1 runs first.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-design-spec.md`

**UC-2 (Opus) — The Grandparent's Ledger**
Single scenario. Arthur, laptop, Sunday afternoon, stewardship mode.
Legacy view, stewardship summary, print layout, silence as affirmation.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Legend-7 (Sonnet build)**
Query flow, result states. Feeds from UC-1 output.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-design-spec.md`

**UC-3 (Opus) — The Bitcoin-Backed Loan**
Single scenario. James and Priya, tablet, lender verification.
Verification export flow, lender view, fiat prominence, holding period display.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Articles draft block (Sonnet)**
Draft Articles A, B, C (queued). No publish. 2–3 weeks sitting time.
Outlines for Articles 14, 15 also worth roughing in during this block.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-articles-list.md`

**UC-4 (Opus) — The Council and the Whale**
Single scenario. West Midlands council, housing development collateral.
Treasury watch, FROST attestation, institutional language layer.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-5 + UC-6 (Opus, paired) — The Florentine District + The Hanseatic Federation**
Two scenarios, thematically paired — both civic/institutional, both article candidates,
both use the "absence is correct" display pattern.
Outputs: civic treasury UI mode spec, federation settlement UI mode spec,
two article outlines ("The Florentine Protocol", "The Hanseatic Protocol").
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-7 (Opus) — The Block War**
Single scenario. Fee spike, global south, nation-state mempool attack.
Human cost calculator, historical fee context layer, Lightning correlation panel.
Article outline: "Who Gets Priced Out."
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-8 (Opus) — The UTXO Lottery**
Single scenario + cryptographic design session. Merkle lineage proof, block hash
entropy, deterministic winner function. Stress-test miner manipulation economics.
Article outline: "The UTXO Lottery."
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md` + `legend-scope.md`

### Phase 1 build — Legend-6 through Legend-~15 (target: December 2026)

Eleventy shell → SPA query flow → result states → batch query / breach scenario →
Silent Payments display → Article 14 → `refueler.io/legend` live.
Each session: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + one detail file.
UC Opus sessions run in parallel with build sessions — Opus thinks, Sonnet builds.

### CryptoRoadmap block — target: January 2027
*Runs after Phase 1 working explorer is live. Before any v2 build session
touches the PIR or ZK layers. Three Opus sessions + three Sonnet sessions.*

**CryptoRoadmap-1 (Opus) — Primitives audit**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-scope.md`
Cover: Ristretto255 vs secp256k1 for Legend and Share (ecosystem compatibility cost,
WebCrypto acceleration, Safari memory profile, honest performance figures — no
speculative multipliers); FROST + blind signatures against Crites-Komlo-Maller 2023
formalisation; threshold blind signatures as a combined primitive; Checklist PIR
(Henzinger et al. 2023) vs Spiral PIR 2022 for Legend's UTXO query pattern —
including the client hint payload size question which is the real adoption risk.
Output: `legend-crypto-primitives.md`

**CryptoRoadmap-2 (Opus) — Transport and scanning layer**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-node-plan.md`
Cover: Signal Sealed Sender as a query-sender-hiding model; ML-KEM-768 hybrid
against NIST FIPS 203 final spec and `ml-kem` Rust crate maturity; Double Ratchet
session initialisation latency on the Enterprise API; Fuzzy Message Detection
(Beck et al. 2021) for Silent Payments block subscription without scan key
revelation — false-positive rate at production block volume; post-quantum blind
signatures honest timeline.
Output: `legend-crypto-transport.md`

**CryptoRoadmap-3 (Opus) — ZK architecture and estate/lending use case**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-scope.md`,
`legend-enterprise-pricing.md`
Cover: Bulletproofs+ (Eagen et al. 2022) vs Groth16 vs PLONK for ZK balance proofs
— proof size (solicitor emails a file), verification time (lender runs this), prover
time (client-side generation); `bellman` vs `arkworks` Rust ecosystem maturity 2026;
Camenisch-Lysyanskaya credentials for v3 Enterprise attributes; `/legend/verify`
endpoint architecture for a non-cryptographer solicitor; academic contribution
pathway and citation credibility requirements.
Output: `legend-crypto-zk.md`

**CryptoRoadmap-4 (Sonnet) — Primitives decisions**
Translates `legend-crypto-primitives.md` into version assignments and build
complexity estimates. Feeds back into `legend-scope.md` version tables.
Output: version table amendments, queued edits to `legend-scope.md`.

**CryptoRoadmap-5 (Sonnet) — Transport decisions**
Translates `legend-crypto-transport.md` into version assignments.
Output: queued edits to `legend-node-plan.md` transport section.

**CryptoRoadmap-6 (Sonnet) — ZK decisions**
Translates `legend-crypto-zk.md` into version assignments and the `/legend/verify`
build spec stub.
Output: `/legend/verify` spec stub, queued edits to `legend-scope.md`.

### Preliminary research filed (Legend-5)

The following claims from initial research are noted for CryptoRoadmap-1 calibration.
Do not treat as confirmed until the Opus session produces honest figures:

- Ristretto255 batch verification latency reduction: mechanism confirmed, multiplier
  (3–5×) is speculative — calibrate in CryptoRoadmap-1.
- Checklist PIR "controls 4 of 5 nodes" framing: overstates the guarantee for our
  three-node topology. The two-server security model needs precise mapping to three
  nodes before this claim appears anywhere.
- "secp256k1 not a natural fit for ZK": overstated. Correct version: Ristretto255
  eliminates cofactor edge cases, making ZK implementation harder to get wrong —
  not that secp256k1 cannot do ZK.
- Share/Safari performance claims (WebWorker throttling, memory footprint, hardware
  acceleration): directionally correct, require browser benchmarks before being
  stated as product claims.

### Phase 2 and beyond — post CryptoRoadmap block

v2 PIR architecture, Spiral or Checklist PIR (decided in CryptoRoadmap-4),
ZK balance proofs, post-quantum transport hardening, `legend-incident-protocol.md`,
Enterprise onboarding.

### refueler-ecash-lab

Internal research and test environment (NUT-12, NUT-14, NUT-18, NUT-20).
Red folder — not a GitHub repo yet. Make it a repo when B9 is stable and
Legend's mint design is locked enough to define what the lab needs to test.
Do not conflate with production mint architecture. Findings feed `mintInterface.ts`.

---

*"Nothing stops this train."*
