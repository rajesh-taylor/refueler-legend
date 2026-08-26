# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries to `SESSIONS-ARCHIVE.md`.*
*Session naming: Multi-[n] for planning. Legend-[n] for build. UC-[n] Opus for use-case scoping.*

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
  Lightning correlation panel. Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md`.
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
- **UC-5 + UC-6 Opus next** — paired session, Florentine District + Hanseatic Federation. Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md` + `legend-copy-index.md`.

---

## Session UC-4 Opus · 25 Aug 2026

**Phase:** Use case scoping — UC-4 The Council and the Whale
**Status:** Complete. Verification/watch boundary locked. Treasury Watch Mode and Watch Recipient View locked. Institutional register + no-alert honest-scope statement locked. 15 new locked strings. Four honesty corrections to the prior brief. Automated alerts confirmed v3 (not v2). Canonical artefact now four consumers.

### Completed

- **Verification/watch boundary locked.** Same machinery (fragment-encoded address
  set, in-browser cross-node SPV, nothing stored on Legend). They differ only in
  temporal purpose. A verification supports one decision and warns "may be stale,
  re-check." A watch supports a standing covenant and warns "Legend does not watch
  for you — no alert is not no movement." The honesty burden inverts.
  Locked principle: *a verification is a snapshot, a watch is a live link,
  neither is an alarm.*

- **Honest "future detection" constrained.** Push movement alerting is v3-only,
  blind-channel-only (Pass Access-class, per UC-9), reactive (post-confirmation),
  never preventive. Any non-blind channel (e.g. email) would let Legend join the
  council's identity to the address — prohibited. Constraint is architectural,
  not a policy decision.

- **Treasury Watch Mode locked** — Legend's second explicitly-invoked mode.
  `Create a watch →` on standard/Stewardship results; never in Distress Mode.
  Reuses the verification/batch modal chrome. v1: `Copy watch link`. v2:
  `Download signed watch attestation` + `Add proof of control` (BIP-322, at
  watch creation time only). Full anatomy fed to `legend-ui-modes.md`.

- **Watch Recipient View locked** — live variant of the UC-3 Recipient View
  with institutional register applied. Movement status is the load-bearing
  element (replaces holding period). First canary summary inside a recipient
  surface — reuses four-node canary state from the status page; no new machinery
  beyond the existing `--canary-expired` token. No-alert footer inverts the UC-3
  honest-scope footer. GBP-first denomination. Three-reads stillness constraint
  applies. Full anatomy fed to `legend-ui-modes.md`.

- **Four corrections to the prior brief:**
  1. Canary string "…has not been legally compromised" rejected — overclaims
     against CLAUDE.md canary honesty note (3-of-4 does not catch single-operator
     IPA compulsion). Replaced with factual "all four canaries are current" face-line
     plus honest explainer link.
  2. "Date of next scheduled check" deleted — Legend schedules nothing in a pull
     model; implies monitoring Legend does not do.
  3. "Movement alert" (v1 language) → "movement status" — `alert` reserved
     strictly for v3 push notification.
  4. "Automated movement alerts" moved v2 → v3; "session-based/persistent"
     reframed — nothing persists on Legend; durability is in the link, not
     the server.

- **Canonical v2 artefact now shared across FOUR consumers — UC-2/UC-3/UC-4/UC-9.**
  Watch attestation variant adds `watch created at block [height]` line. Single
  locked field format; changes checked against all four.

- **UC-9 Elena dependency confirmed.** Watch Recipient View + Treasury Watch Mode
  are the upstream components Elena (sovereign treasury officer) reuses. Changes to
  these surfaces must be checked against the UC-9 Elena track.

- **Institutional register locked (§1) and no-alert honest-scope statement
  locked (§6).** 15 new locked strings drafted for §8, all fed to
  `legend-copy-index.md`.

### Files changed

- `legend-use-cases.md` v1.3 → v1.4 (UC-4 full spec replaces brief — applied by hand)
- `legend-ui-modes.md` (new in restructure — Treasury Watch Mode + Watch Recipient View sections)
- `legend-copy-index.md` (new in restructure — UC-2 + UC-3 + UC-4 additions: institutional register §1, denomination defaults §4, no-alert statement §6, all outstanding locked strings §8)
- `legend-scope.md` v1.4 → v1.7 (UC-2 + UC-3 + UC-4 additions: watch version rows, four-consumer artefact note, v3 blind-channel constraint, version summary rows)
- `SESSIONS.md` (this entry)

### Carry-forward

- **UC-5 + UC-6 Opus next** (paired) — Florentine District + Hanseatic Federation.
  Both civic/institutional, both article candidates, both use the "absence is
  correct" display pattern. UC-4's Watch Recipient View may inform the civic
  treasury public view — check for surface reuse before speccing a new mode.
- **Canonical v2 artefact — four consumers (UC-2/UC-3/UC-4/UC-9).** Single locked
  spec; any change checked against all four.
- **v3 alerting is a locked architectural constraint** — blind-channel (Pass)
  delivery only; cross-reference UC-9 Pass Access-class credential (NUT-12 DLEQ;
  no redemption-linkable address binding). Not to be re-derived.
- **Canary summary now appears in a recipient surface** — Watch Recipient View
  canary line must draw from the same four-node canary state as the status page.
  Flag for Legend-6+ build: one canary state source, two surfaces.
- **Build recommendation stands** — run remaining UC sessions in parallel with
  Legend-6 build; UC-4 does not block Phase 1 build work.
- **All prior carry-forwards remain open** — Legend-6 build; Opus-C solicitor (IPA + insolvency); FROST ceremony; provider quotes; two-operator milestone; stale figures in `legend-economics.md`, `legend-enterprise-pricing.md`, `legend-node-plan.md` header.

---

## Session UC-3 Opus · 23 Aug 2026

**Phase:** Use case scoping — UC-3 The Bitcoin-Backed Loan
**Status:** Complete. Shared-artefact commissioning distinction locked. Verification Mode and Lender/Recipient View locked. Verification/disclosure register locked. 17 new locked strings. One substantive version change (proof-of-control → v2).

### Completed

- **Shared v2 artefact confirmed and commissioning distinction locked.** UC-2 estate
  and UC-3 loan consume one canonical Merkle-anchored, FROST-signed verified-holdings
  statement. Estate = verifier-commissioned, no proof-of-control (subject absent).
  Loan = subject-commissioned, proof-of-control attached (subject present). Format
  identical. Same substrate also underlies UC-9's Chain Trace Report. Its field format
  is a single locked spec; changes must be checked against all three consumers (UC-2,
  UC-3, UC-9).

- **Verification Mode locked** — Legend's first explicitly-invoked mode (vs inferred
  Distress/Stewardship). Triggered by `Create a verification →` on standard and
  Stewardship results; never in Distress Mode. Generation modal reuses batch-modal
  chrome. v1 action: `Copy shareable link` (URL fragment only). v2 actions:
  `Download signed verification` + `Add proof of control`.

- **v1 share-link mechanism locked** — address + reference block in URL fragment
  only; never sent to server; no server-side record; recipient's browser runs
  in-browser cross-node SPV to verify. Log-free and storage-free, consistent with
  the no-logs guarantee.

- **Recipient View / Lender View locked** — read-only, chrome-less surface a share
  link opens into. Stripped of all Legend nav, query box, onboarding, chrome. Keeps:
  wordmark, verified data, holding-period statement (prominent, front and centre),
  verification anchor, proof-of-control state, independent-verification affordance,
  per-address table (full untruncated), honest-scope footer. Three-reads stillness
  constraint applies. GBP-first. Full anatomy fed to `legend-ui-modes.md`.

- **Verification/disclosure register locked** — sibling to bereavement/stewardship
  registers; governs the one moment Legend helps a user disclose, not conceal.
  Inverted failure mode (letting a user believe a chosen disclosure is still private)
  is what the copy guards against. Register anchor string 18 in §8 and §6 statement.

- **Contextual denomination rule extended** — Recipient / Verification context =
  GBP-first (professional counterparty). Unifying principle stated: denomination
  follows the reader's relationship to the holding (owner→sats; non-owner→fiat).

- **Proof-of-control moved to v2.** BIP-322 signed-message verification; Legend
  never handles keys. BIP-322 only — BIP-137 (legacy, P2WPKH-only, not Taproot-
  compatible, not multi-sig-compatible) never used for this purpose.

- **Sales argument sequencing locked.** v1 and v2 stated plainly to prospects as
  current capability. v3 address-hiding ZK proof (prove threshold balance without
  revealing address) offered as explicit roadmap only — never implied as current.
  The privacy gradient across three versions is itself the sales argument for v3.

- **17 new locked strings drafted** for `legend-copy-index.md` §8.

### Files changed

- `legend-use-cases.md` v1.2 → v1.3 (UC-3 full spec replaces brief)
- `legend-ui-modes.md` (new in restructure — Verification Mode + Recipient/Lender View sections)
- `legend-copy-index.md` (new in restructure — verification/disclosure register + §6 statement + 17 strings + denomination rule extension)
- `legend-scope.md` v1.5 → v1.6 (proof-of-control v2 line + BIP-322 note + version-summary rows)
- `SESSIONS.md` (this entry)

### Carry-forward

- **UC-4 Opus next** — The Council and the Whale. ✅ Complete · 25 Aug 2026
- **Canonical v2 artefact field format now shared across UC-2 / UC-3 / UC-9** —
  treat as a single locked spec; changes must be checked against all three consumers.
  (Updated to four consumers at UC-4.)
- **Build recommendation** — start Legend-6 Eleventy shell build session now; run
  remaining UC sessions (UC-4 through UC-9) in parallel with build sessions.
- **BIP-322 implementation note** — multisig proof-of-control signing quorum UX
  deferred to a v2 build session; not a lock.
- **Article 23** — `/legend/verify` + lending use case; UC-3 is its upstream.
  Draft at v3 approach.
- **All prior carry-forwards remain open** — Legend-6 build; Opus-C solicitor; FROST ceremony; provider quotes; two-operator milestone; stale figures in `legend-economics.md` and `legend-enterprise-pricing.md`.

---

## Session queue

### Next: UC-5 + UC-6 (Opus, paired) — The Florentine District + The Hanseatic Federation

Two scenarios, thematically paired — both civic/institutional, both article candidates,
both use the "absence is correct" display pattern.
Outputs: civic treasury UI mode spec, federation settlement UI mode spec,
two article outlines ("The Florentine Protocol", "The Hanseatic Protocol").
Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md` + `legend-copy-index.md`

### UC-7 (Opus) — The Block War

Single scenario. Fee spike, global south, nation-state mempool attack.
Human cost calculator, historical fee context layer, Lightning correlation panel.
Article outline: "Who Gets Priced Out."
Load: `CLAUDE.md` + `SESSIONS.md` + `legend-use-cases-2.md`

### UC-8 (Opus) — The UTXO Lottery

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
