# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries to `SESSIONS-ARCHIVE.md`.*
*Session naming: Multi-[n] for planning. Legend-[n] for build. UC-[n] Opus for use-case scoping.*

---

## Session Multi-15 · 26 Aug 2026

**Phase:** Use case scoping — UC-7 The Block War (Opus session)
**Status:** Complete. Fee Context Layer locked. Affordability register locked. Fifth contextual denomination rule (USD-first for network costs) locked. Article UC-7 outline locked. Eight questions answered. Five files patched.

### Key decisions

- **No new mode** — confirmed. The Fee Context Layer is a conditional display layer, not a mode. Architectural sibling to the N-1/N-2 degraded-mode banner (network-state-triggered), not to the Recipient-View family. Future network-state features check against this class.

- **Fee Context Layer locked** — two fidelities:
  - *Network-context* (no address queried): percentile sentence + comparable-spike history. Text only. No chart.
  - *Personalised* (address queried): human-cost calculator + per-UTXO economic-dust flags, computed in-browser against the already-returned UTXO set. No additional server query; no address leaves the browser.
  Trigger: current fee rate > 90th percentile of trailing-12-month daily-median fee rate. Collapsible; not non-dismissible (distinct from the privacy banner). Active register governs composition.

- **Fee-rate history index locked as v1 build prerequisite** — percentile claim and comparable-spike history require a 12-month per-block fee-rate record precomputed at ingestion. Directly analogous to the SP tweak index. Without it, Fee Context Layer cannot ship.

- **Affordability register locked** — sibling to the bereavement register under the Distress context. *Distress Mode is not inherently bereavement.* Two registers now sit under Distress Mode: bereavement (UC-1, Bradford) and affordability (UC-7, Lagos). Both are distress; neither is the other.

- **Fifth contextual denomination rule locked** — network-cost figures are USD-first, regardless of the active holding denomination. A fee is a cost, not a holding. USD is the global network-cost reference. GBP is a UK-parochial default that fails a Lagos user and anyone outside the UK. The unifying principle (denomination follows the reader's relationship to the holding or cost) now has five applications.

- **Non-forecast discipline locked** — temporal extension of the UC-6 provenance-agnosticism rule. Legend states comparable-spike history as fact; it never forecasts when the current spike will clear. Cause-attribution ("nation-state attack", "miner coordination") is permanently out of scope at v1/v2; descriptive anomaly heuristics at v3 only, stated explicitly as such.

- **Wallet-line boundary logged** — Legend prices a hypothetical consolidation from the user's own returned UTXO set; never constructs, signs, or broadcasts. Pricing a hypothetical is not wallet behaviour.

- **Economic-dust flag locked** — new sibling to the UTXO consolidation advisor. Per-UTXO, tertiary, below the UTXO table. Same no-recommendation, factual treatment.

- **Lightning note (v1)** — factual on-chain note only: address has appeared in Lightning channel transactions. No capability inference. V2 extends to "you may have off-chain options" framing with honest scope.

- **Article UC-7 ("Who Gets Priced Out") outline locked** — opening line and closing line locked. Full beats logged.

### Files changed

- `legend-ux-language.md` v1.4 → v1.5 — Affordability register added (§1); temporal extension of provenance-agnosticism rule added (§1); §4 extended with fifth contextual denomination rule (USD-first for network costs) and updated unifying principle; bereavement register note clarifying Distress Mode ≠ bereavement
- `legend-copy-index.md` v1.0 → v1.1 — §6 new honest-scope statement (non-forecast / no-diagnosis, locked UC-7); §8 denomination rule note extended (fifth rule); 10 new locked strings appended (UC-7)
- `legend-scope.md` v1.8 → v1.9 — Fee-rate history index added as v1 build prerequisite; Fee Context Layer added to professional use cases (v1); mempool-anomaly cause detection added (v3, descriptive only); v1 query scope note amended (basic fee display → Fee Context Layer); Distress Mode note updated (two registers); version summary table: new Fee Context Layer row, fee-rate history index row, denomination toggle row updated; permanent out-of-scope table: fee-spike cause attribution added; v2: UTXO consolidation cost projection and Lightning framing added
- `legend-articles-list.md` v1.4 → v1.5 — UC-7 provisional stub replaced with full outline; publishing sequence table UC-7 dependency updated to Fee Context Layer (v1)
- `SESSIONS.md` — this entry

### Carry-forward

- **UC-8 next** — The UTXO Lottery (single scenario + cryptographic design session). Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md` + `legend-scope.md`.
- **Recipient-View family — four consumers.** Changes to the substrate must be checked against all four: Lender View · Watch Recipient View · Civic Treasury View · Federation Settlement View.
- **Fee Context Layer = new layer class.** Future network-state conditional display features check against this class before speccing as a mode.
- **Provenance-agnosticism rule now has a temporal dimension** — check future features that involve historical pattern display (fee history, settlement cadence) against the no-forecast extension.
- **⚠ Fee-rate history index = v1 data prerequisite** — must be built before Fee Context Layer ships. Flag at Legend-6+ build planning alongside SP tweak index.
- **Denomination rule update** — §4 and §8 now have five contextual rules. Any future context that introduces a new denomination default must be checked against all five before adding a sixth.
- **All prior carry-forwards remain open** — Legend-6 build; Opus-C solicitor (IPA + insolvency); FROST ceremony; provider quote replies; two-operator milestone; stale figures in `legend-economics.md`, `legend-enterprise-pricing.md`, `legend-node-plan.md` header; Share × Legend v2 planning session; ⚠ fifth-consumer flag on optional signed civic attestation (parked v2 decision).

---

## Session Multi-14 · 26 Aug 2026

**Phase:** Use case scoping — UC-5 The Florentine District + UC-6 The Hanseatic Federation (paired Opus session)
**Status:** Complete. Civic Treasury View and Federation Settlement View locked. Three architectural decisions resolved. Two article outlines locked. Fourth contextual denomination rule locked. Provenance-agnosticism rule locked. Five files patched.

### Completed

- **Three forks resolved before speccing:**
  1. Civic Treasury View is a new member of the Recipient-View family (sibling to Lender,
     Watch Recipient, and Federation Settlement Views) — not a Watch Recipient View variant.
     Load-bearing element differs: UC-4 = movement status (stillness); UC-5 = outflow pattern
     over time (consistency). Different questions; distinct surfaces on the same substrate.
  2. BTC-first denomination for declared institutional reserves — fourth contextual rule,
     extends rather than contradicts the unifying principle. Locked UC-5 Opus.
  3. Provenance-agnosticism rule locked — Legend describes cadence, never asserts provenance.
     Generalises the "Activity detected" / "Compromised" decision. Forbidden: asserting a UTXO
     represents a Fedimint settlement unless the address is *declared* by the federation.

- **Civic Treasury View locked** — read surface, v1. Recipient-View family member. Sibling to
  Lender View, Watch Recipient View, Federation Settlement View. Load-bearing element: activity
  summary (outflow/inflow count + monthly aggregate panel). One genuinely new v1 build component:
  the monthly aggregate panel (Month · Inflows · Outflows · Net). All other components reuse
  existing substrate. Publisher context (identity caveat) is the first element after the header —
  non-omittable. Declared-not-exhaustive footer is the load-bearing copy discipline. BTC-first.
  Full spec in `legend-ui-modes.md`.

- **Civic Treasury Mode locked** — v2, Legend's third explicitly-invoked mode. Specifies the
  parked "public-body transparency module" from `legend-scope.md`. Entry CTA: `Publish a treasury
  view →`. Same URL-fragment mechanism as UC-3/UC-4. Actions: `Copy treasury-view link` +
  `Download monthly summary`. ⚠ Optional signed civic attestation parked as v2 decision item
  (would be fifth artefact consumer — field-format check across all four required first).

- **Recipient-View family finalised (four members):**
  Lender View (UC-3) · Watch Recipient View (UC-4) · Civic Treasury View (UC-5) · Federation
  Settlement View (UC-6). One substrate; four load-bearing elements. Changes to substrate check
  against all four.

- **Civic transparency register locked** — sibling to bereavement, stewardship, verification/
  disclosure, and institutional registers. Guards two failure modes: (a) observer reads a
  voluntary, partial, self-declared disclosure as a complete institutional audit; (b) observer
  reads Legend's rendering as an identity endorsement. Never: "audit", "certified", "official",
  "verified organisation". Never: "outflows are invisible" (only recipients are unlinkable).

- **Provenance-agnosticism rule locked** — generalises the batch-result decision to all heuristic
  display enhancements. Assertive provenance (e.g. "this is a Fedimint settlement") permitted only
  when the address is *declared* by the party described. Undeclared address: descriptive + conditional
  only. Keeps settlement-pattern detection on the holder's side of the third-party-analytics line.

- **Federation Settlement View locked** — variant of Civic Treasury View. Load-bearing element:
  settlement cadence (most recent first). Two modes: (a) declared — assertive; header, publisher
  context, cadence, declared absence note, full substrate; (b) undeclared — settlement-pattern
  display enhancement on standard result + conditional absence note only; no provenance assertion.
  Full spec in `legend-ui-modes.md`.

- **Fourth contextual denomination rule locked** — Civic Treasury View / Federation Settlement View:
  BTC-first. Declared institutional reserve states itself in BTC. Sats secondary; fiat-at-time
  via toggle. Updated in `legend-ux-language.md` §1 and §4, `legend-copy-index.md` §8, `legend-scope.md`.

- **Two article outlines locked** — "The Florentine Protocol" and "The Hanseatic Protocol."
  Both opening lines promoted from provisional to locked. Custom House (Lower Thames Street, London)
  added as historical anchor in "The Hanseatic Protocol" — the port ledger that was public but
  surveilled, versus Legend's port ledger that is readable by anyone and logged by no one.

- **Public-body transparency module cross-reference updated** — `legend-scope.md` v2 section now
  points to Civic Treasury Mode as its specification. Parked status lifted.

- **Share × Legend planning note logged** — at v2, Share may provide encrypted transport where
  Legend provides the civic treasury view. One place the two products touch without Share becoming
  a Legend dependency. Requires a dedicated planning session before v2 build on either side.
  Not baked into any current spec.

### Files changed

- `legend-ui-modes.md` — Civic Treasury View, Civic Treasury Mode, Federation Settlement View (new sections)
- `legend-ux-language.md` — civic transparency register + provenance-agnosticism rule (§1 new sections); fourth denomination rule + updated unifying principle (§4)
- `legend-copy-index.md` — denomination rule note extended (§8); civic treasury + provenance-agnosticism honest-scope statements (§6); UC-5 and UC-6 strings appended (§8)
- `legend-scope.md` — UI modes list updated; public-body module cross-reference updated; UC-5 and UC-6 version rows added (version summary table); denomination toggle row updated
- `legend-articles-list.md` — UC-5 and UC-6 full outlines replacing provisional stubs; publishing sequence table updated; closing note updated
- `SESSIONS.md` (this entry)

### Carry-forward

- **UC-7 (The Block War) next** — single scenario; human cost calculator, fee context layer,
  Lightning correlation panel. Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md`. ✅ Complete · 26 Aug 2026
- **Recipient-View family — four consumers.** Changes to the substrate (chrome-less, three-reads
  stillness, canary summary, verification anchor, per-address table, declared-not-exhaustive footer)
  must be checked against all four: Lender View · Watch Recipient View · Civic Treasury View ·
  Federation Settlement View.
- **⚠ Fifth-consumer flag (parked v2 decision).** Optional signed civic attestation on Civic
  Treasury Mode would be the fifth consumer of the canonical FROST-signed artefact (current four:
  UC-2/3/4/9). Field-format check across all four required before implementation. Do not implement
  without that check.
- **Share × Legend v2 integration — dedicated planning session required** before any v2 build
  begins on either side. Share provides transport; Legend provides the civic treasury view.
  Not a Legend dependency at v1 or v2 without that session.
- **Provenance-agnosticism rule** — check any future heuristic display feature (settlement pattern,
  CoinJoin detection, Fedimint recognition, etc.) against this rule before speccing.
- **All prior carry-forwards remain open** — Legend-6 build; Opus-C solicitor (IPA + insolvency);
  FROST ceremony; provider quote replies; two-operator milestone; stale figures in
  `legend-economics.md`, `legend-enterprise-pricing.md`, `legend-node-plan.md` header.

---

## Session Multi-13 · 25 Aug 2026

**Phase:** File restructure — context management before UC-5
**Status:** Complete. All five core documents split and produced. MASTER.md retired.

### Completed

- **File split plan approved and executed.** Five core documents split into nine.
  All files under 450 lines (exception: `legend-use-cases-2.md` at ~500 lines).

- **New files created:**
  - `legend-ui-modes.md` — all UI mode specs extracted from `legend-design-spec.md`
  - `legend-design-appendix.md` — post-B9 scope additions + session roadmap extracted from `legend-design-spec.md`
  - `legend-use-cases-2.md` — UC-5 through UC-9 + Opus session structure + B9 gate note + scope essay extracted from `legend-use-cases.md`
  - `legend-copy-index.md` — Sections 6–8 (honest-scope statements, degraded mode notices, locked copy index) extracted from `legend-ux-language.md`
  - `SESSIONS-ARCHIVE.md` — all session entries prior to UC-3 archived here

- **Files updated:**
  - `legend-design-spec.md` — header updated, modes and appendix extracted, cross-references updated
  - `legend-ux-language.md` — trimmed to Sections 1–5; §6–8 now in `legend-copy-index.md`; infrastructure cost updated to €673/month
  - `legend-scope.md` — carry-forward section deleted (history belongs in SESSIONS.md); stale figures removed; node table updated to reflect confirmed providers (B, D, E no longer TBD)
  - `legend-use-cases.md` — trimmed to UC-1–4 full specs; UC-5+ extracted to `legend-use-cases-2.md`; Opus session load instructions updated
  - `SESSIONS.md` — this restructure entry; pre-UC-3 sessions archived

- **MASTER.md retired.** Stale figures (three-node/2-of-3/€360) removed from circulation. Content distributed across split files. MASTER.md is no longer used as a session loader.

- **CLAUDE.md load instruction block** updated by Rajesh before session open (manual paste of new file load instructions from Multi-[n] plan). No further change required.

### Files changed

- All nine files above (new + updated)
- `SESSIONS-ARCHIVE.md` (new)

### Carry-forward

- **All prior carry-forwards remain open:** Legend-6 build; Opus-C solicitor (IPA + insolvency); FROST ceremony; provider quote replies; two-operator milestone; stale figures in `legend-economics.md`, `legend-enterprise-pricing.md`, `legend-node-plan.md` header.
- **`legend-ux-language.md` §2 infrastructure cost** — "roughly €360 a month" → "roughly €673 a month" patched in this restructure. Verify the below-the-fold body copy on the live holding page does not also carry the old figure.
- **UC-5 + UC-6 Opus next** — paired session, Florentine District + Hanseatic Federation. Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md` + `legend-copy-index.md`. ✅ Complete · 26 Aug 2026

---

## Session queue

### Next: UC-8 (Opus) — The UTXO Lottery

Single scenario + cryptographic design session. Merkle lineage proof, block hash
entropy, deterministic winner function. Stress-test miner manipulation economics.
Article outline: "The UTXO Lottery."
Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md` + `legend-scope.md`

### Phase 1 build — Legend-6 through Legend-~15 (target: December 2026)

Eleventy shell → SPA query flow → result states → batch query / breach scenario →
Silent Payments display → Article 14 → `refueler.io/legend` live.
Each session: `CLAUDE.md` + `SESSIONS.md` + `legend-design-spec.md` + one detail file.
UC Opus sessions run in parallel with build sessions — Opus thinks, Sonnet builds.

### CryptoRoadmap block — target: January 2027

*Runs after Phase 1 working explorer is live. Before any v2 build session
touches the PIR or ZK layers. Three Opus sessions + three Sonnet sessions.*

**CryptoRoadmap-1 (Opus) — Primitives audit**
Load: `CLAUDE.md`, `SESSIONS.md`, `legend-scope.md`
Cover: Ristretto255 vs secp256k1 for Legend and Share (ecosystem compatibility cost,
WebCrypto acceleration, Safari memory profile, honest performance figures — no
speculative multipliers); FROST + blind signatures against Crites-Komlo-Maller 2023
formalisation; threshold blind signatures as a combined primitive; Checklist PIR
(Henzinger et al. 2023) vs Spiral PIR 2022 for Legend's UTXO query pattern —
including the client hint payload size question which is the real adoption risk.
Output: `legend-crypto-primitives.md`

**CryptoRoadmap-2 (Opus) — Transport and scanning layer**
Load: `CLAUDE.md`, `SESSIONS.md`, `legend-node-plan.md`
Cover: Signal Sealed Sender as a query-sender-hiding model; ML-KEM-768 hybrid
against NIST FIPS 203 final spec and `ml-kem` Rust crate maturity; Double Ratchet
session initialisation latency on the Enterprise API; Fuzzy Message Detection
(Beck et al. 2021) for Silent Payments block subscription without scan key
revelation — false-positive rate at production block volume; post-quantum blind
signatures honest timeline.
Output: `legend-crypto-transport.md`

**CryptoRoadmap-3 (Opus) — ZK architecture and estate/lending use case**
Load: `CLAUDE.md`, `SESSIONS.md`, `legend-scope.md`, `legend-enterprise-pricing.md`
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
