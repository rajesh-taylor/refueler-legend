# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---
## Session Legend-7B · 7 Aug 2026

**Phase:** 1 pre-build harness — update session
**Status:** Complete. Five files updated to reflect five-node topology and FROST 3-of-4.

### Completed

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

- `legend-incident-protocol.md` v1.1
- `SESSIONS.md` (this entry)
- `legend-economics.md`
- `legend-scope.md`
- `legend-design-spec.md`

### Carry-forward

- **Opus-A (solicitor) items** — unchanged. IPA compelled-continuation question; NDO publication
  exposure; publication-delegate liability; MLAT characterisation. None can progress without counsel.
- **Provider selection session** — Nodes B, D, E are TBD. Required before any node is provisioned.
  Output: completed locations table with no TBD entries, confirmed jurisdiction independence.
- **FROST 3-of-4 implementation confirmation** — `frost-secp256k1` crate supports configurable
  thresholds; confirm 3-of-4 and test ceremony before first node goes live with query traffic.
- **DKG ceremony duration** — measure on real hardware; replace "minutes" in SLA materials with a
  confirmed figure before any Enterprise contract is signed.
- **Index rebuild time** — measure on matching hardware; replace "hours to ~1 day" with a real
  figure. Open question in Opus-A notes.
- **Sub-quorum partial-signature open question** (Opus-A notes, new) — can two participants produce
  a partial Schnorr signature under a 3-of-4 key that a third can complete when rejoining? Affects
  operator priority timeline in §3D.
- **Custom FlokiNET quote** — still open. Required before provisioning Node C.
- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **GBP denomination edit** — `legend-ux-language.md` §4/§8. Apply next time that file is touched.
- **Design-spec token block** — pre-CC-74 stale `--bg` values. Correct next time that file is edited.

---
## Session Legend-7 · 7 Aug 2026

**Phase:** 1 pre-build harness — Opus-B adversarial review of legend-incident-protocol.md
**Status:** Complete. `legend-node-plan.md` v1.0 → v1.1 produced. Legend-7B update session queued.

### Completed

- Opus-B adversarial review of `legend-incident-protocol.md` v1.0 against six simulated attack
  scenarios. Five ranked gaps identified and documented.
- Six simulations run:
  1. **Seizure + migration compound event** — two nodes lost in overlapping windows; FROST signing
     falls to 2-of-2, below documented 2-of-3 floor. Identified sub-quorum gap.
  2. **AI attack at DKG boundary** — sustained exploit campaign coinciding with monthly ceremony;
     operator exhaustion causes missed DKG; window closes, shares destroyed, canary automation
     halts. Identified DKG-under-attack gap.
  3. **Provider forces migration under time pressure** — second node fails mid-migration; procedure
     had no branch for this compound state. Identified degraded-migration gap.
  4. **Snapshot restoration with unverified provenance** — recovery proceeds from a snapshot of
     unknown integrity; §3A step 4 treated binary and index verification as one step. Identified
     binary-vs-index split gap.
  5. **Channel asymmetry** — node's own `canary.json` confirms but GitHub and Nostr unreachable;
     no definition of whether this counts as published. Identified channel-asymmetry definition gap.
  6. **Warm standby Node D under attack** — confirmed D's isolation from active attack traffic
     (private interface architecture, `legend-node-plan.md` §7); no gap.
- Five ranked gaps, in priority order:
  1. §3D sub-quorum procedure absent — highest risk; sub-quorum state had no operator protocol.
  2. DKG-under-attack: no stated priority rule; residual path from D to C unmitigated.
  3. §3C degraded-migration branch absent — compound failure during migration had no procedure.
  4. §3A step 4 not split — binary and index integrity verification conflated.
  5. Channel-asymmetry publication definition absent — "published" was undefined for partial channel failure.
- `legend-node-plan.md` v1.0 → v1.1 produced:
  - Three-node topology replaced with five-node topology (Nodes A–E).
  - Node D defined: full participant, warm standby, fourth independent jurisdiction, fully synced from day one.
  - Node E defined: chain-only cold standby, no FROST share, no Esplora index at launch; chain continuously
    synced so Esplora index build is the only remaining step when E is activated.
  - FROST 2-of-3 replaced with FROST 3-of-4 across four full participants (A, B, C, D).
  - §9 redundancy/failover section added: N-1, N-2, N-3 (sub-quorum) states defined.
  - Provider selection criteria updated: Node B changed from Hetzner Helsinki (shared parent with A)
    to TBD non-Hetzner provider. Nodes B, D, E all TBD pending provider session.
  - Cost carry-forward noted: five-node estimate ~€350–550/month.

### Files changed

- `legend-node-plan.md` v1.1 — committed
- `SESSIONS.md` (this entry)

### Carry-forward (to Legend-7B)

- `legend-incident-protocol.md` v1.1 — fold in all five Opus-B gaps (covered in Legend-7B).
- `legend-economics.md` — five-node cost model update.
- `legend-scope.md` — topology references update.
- `legend-design-spec.md` — status page privacy modes review.

---
## Session Legend-6 · 7 Aug 2026

**Phase:** 1 pre-build harness — operational incident runbook
**Status:** Complete. `legend-incident-protocol.md` v1.0 produced. Committed `31c5caa`.

### Completed

- `legend-incident-protocol.md` v1.0 produced. Eight sections plus `## Opus-A notes`:
  - Section 1 — four canary failure modes (A operational lapse / B single-node seizure /
    C UK IPA 2016 compulsion / D AI-assisted attack), scenario matrix, and the C/D
    structural distinction (canary continuity decoupled from query API).
  - Section 2 — operator incapacitation: short-term, extended, and permanent.
    FROST share escrow and timer-based dead-man's switch both rejected (trust assumptions /
    self-defeating). Pre-signed notices (date-bounded, not canaries) are the sound option.
    Permanent incapacitation disclosed honestly as a solo-operator constraint.
  - Section 3 — node recovery: 3A (provider failure), 3B (seizure), 3C (provider migration).
    Replacement-provider criteria specified by jurisdiction properties, not vendor list.
    SLA credit assessment rule stated: single-node outage does not breach 99.0% availability
    but burns ≥95% full-splitting budget.
  - Section 4 — FROST re-keying: routine monthly DKG vs. emergency re-key, node-offline
    24-hour extension, compromised-share procedure, client-observable states and maximum window.
  - Section 5 — AI-assisted attack response. Anomaly detection via content-blind counters only.
    Canary-continuity independence confirmed as the structural Boltz-failure safeguard. DKG
    during active attack: canary/mint continuity outranks query-API restoration. Suspend vs.
    degrade criteria stated.
  - Section 6 — client notification protocols. Tiered matrix (free tier / Enterprise). All
    incident types from Section 1 mapped to window, content, and pre-signed status.
    Pre-written canary-lapse-no-compulsion notice specified (the Boltz countermeasure).
  - Section 7 — five pre-signed statement templates, ready to sign and hold in escrow:
    scheduled maintenance pause / operator incapacitation / node seizure public statement /
    service suspension (attack, no timeline stated) / triple canary expiry standing note.
  - Section 8 — revision schedule with log table. Quarterly from v1 + after any incident
    + after any topology or legal change.
  - `## Opus-A notes` — three categories: IPA 2016 legal claims for solicitor review
    (compelled-continuation untested; NDO publication exposure; publication-delegate liability;
    MLAT characterisation); architectural implementation dependencies (decoupled canary signing,
    two-share canary publication for restored node, DKG duration, Nostr relay liveness, offline
    manifest re-sign custody); open questions for Section 2 production standard (delegate
    identity, release mechanism, share-escrow rejection as documented decision, snapshot
    integrity method, permanent-incapacitation contract language).
- `REFUELER-BRIDGE.md` licence corrected: Legend licence updated from Apache 2.0 → MIT,
  consistent with `CLAUDE.md` and MIT upstream lineage.

### Files changed

- `legend-incident-protocol.md` v1.0 — new file (commit `31c5caa`)
- `SESSIONS.md` (this entry)
- `REFUELER-BRIDGE.md` (Legend licence correction, MIT)

### Carry-forward

- **legend-incident-protocol.md** — first Opus session complete ✅. Second Opus session
  (attack vector simulations, node recovery drills) queued when infrastructure is live.
- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **Custom FlokiNET quote** — still open. Required before provisioning node C.
- **MIT/Apache licence discrepancy** — resolved this session. Verified: `CLAUDE.md`,
  `legend-node-plan.md`, `REFUELER-BRIDGE.md` all now state MIT for `refueler-legend`.
- **IPA compelled-continuation question** — queued for UK solicitor before
  `legend-incident-protocol.md` is considered legally finalised.

---
# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-5 · 6 Aug 2026

**Phase:** 1 pre-build harness — MASTER.md compression
**Status:** Complete. `MASTER.md` v1.0 produced and committed.

### Completed

- All nine harness files read in full: `CLAUDE.md` (v1.5), `SESSIONS.md`, `REFUELER-BRIDGE.md`,
  `legend-design-spec.md` (v1.2), `legend-scope.md` (v1.2), `legend-node-plan.md` (v1.0),
  `legend-ux-language.md` (v1.0), `legend-economics.md` (v1.0), `legend-enterprise-pricing.md` (v1.0).
- `MASTER.md` v1.0 produced: 343 lines, nine sections covering node topology, economics,
  enterprise commercial model, design system, copy register, scope by version, open items,
  and file index. Decisions only; pointers to detail files throughout.
- Standard session load from Legend-6 onwards: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md`
  + one specific detail file (4 files, not 9).
- Six CryptoRoadmap sessions scoped and queued: three Opus research sessions
  (primitives audit, transport/scanning layer, ZK architecture) + three Sonnet
  translation sessions. Timing: post-Phase-1 working explorer, before v2 build opens
  (target window: January 2027). See session queue below.
- Preliminary research note filed: Ristretto255 (Share Safari/WebCrypto performance,
  Legend ZK/DLEQ cleanliness, Tor payload reduction), Checklist PIR (Henzinger et al.
  2023) sublinear online phase, FMD (Beck et al. 2021) for Silent Payments scanning.
  Unverified performance claims flagged for calibration in CryptoRoadmap-1.
- Licence divergence noted: CLAUDE.md locks MIT; REFUELER-BRIDGE.md says Apache 2.0
  for Legend. CLAUDE.md is authoritative. Correct BRIDGE at next update.
- GBP denomination edit still open: locked in CLAUDE.md v1.5 but not applied to
  `legend-ux-language.md` §4/§8. Apply at next session touching that file.

### Files produced

- `MASTER.md` v1.0 — new file, committed `be071f5`
- `SESSIONS.md` — this entry, committed and pushed

### Carry-forward

- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **Custom FlokiNET quote** — open. Mandatory before provisioning node C.
- **legend-incident-protocol.md** — two Opus sessions once infrastructure is live.
- **GBP denomination edit** — apply to `legend-ux-language.md` §4/§8 next time that file is touched.
- **Design-spec token block** — `legend-design-spec.md` Design tokens section carries
  pre-CC-74 stale `--bg` values. Correct next time that file is edited.
- **BRIDGE licence correction** — Apache 2.0 → MIT for Legend section. Next BRIDGE update.

---

## Session Legend-4 · 6 Aug 2026

**Phase:** 1 pre-build harness — node status page spec
**Status:** Complete. `legend-design-spec.md` v1.1 → v1.2.

### Completed

- Status page section added to `legend-design-spec.md`: page purpose and scope,
  three-section structure (privacy mode / node status / canary status), per-section
  layout and typography spec, canary expired state (muted amber, typographic only,
  no icon), two new CSS tokens (`--canary-expired`), explicit list of what the page
  does not show, URL and navigation placement.
- Privacy mode: four states locked. Full splitting / Reduced splitting / Single node /
  Credential rotation in progress.
- Canary section visually separated from operational section by full-width rule.
- FROST integrity note added beneath canary blocks — `--text-tertiary`.
- Token additions: `--canary-expired: #B8860B` (Paper) / `#C9A227` (Carbon).

### Files changed

- `legend-design-spec.md` v1.1 → v1.2
- `SESSIONS.md`

---

## Session Legend-3B · 6 Aug 2026

**Phase:** 1 pre-build harness — UX language and information hierarchy
**Status:** Complete. `legend-ux-language.md` v1.0 produced.

### Completed (abbreviated)

- `legend-ux-language.md` v1.0: eight sections covering voice/register, landing hierarchy,
  query flow copy, result anatomy, modal copy, honest-scope statements, degraded mode
  notices, locked copy index (50 entries).

### Files produced

- `legend-ux-language.md` v1.0

---

*Legend-1 through Legend-3A archived. Decisions captured in `legend-economics.md` v1.0,
`legend-enterprise-pricing.md` v1.0, `legend-scope.md` v1.2, `legend-design-spec.md` v1.1.*

---

## Session queue

### Immediate — B9 dependent

**Legend-6** (first build session — gated on B9 Lightning node live)
- If B9 live: Eleventy shell at `refueler.io/legend`, SPA mount point, Paper/Carbon
  theme wired, query input idle state.
- If B9 not yet live: `legend-incident-protocol.md` first draft (node recovery,
  FROST re-keying, attack simulations).
- Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-design-spec.md`.

### Phase 1 build — Legend-6 through Legend-~15 (target: December 2026)

Eleventy shell → SPA query flow → result states → batch query / breach scenario →
Silent Payments display → Article 14 → `refueler.io/legend` live.
Each session: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + one detail file.

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

## Session Legend-5 · 6 Aug 2026

**Phase:** 1 pre-build harness — MASTER.md compression
**Status:** Complete. `MASTER.md` v1.0 produced and committed.

### Completed

- All nine harness files read in full: `CLAUDE.md` (v1.5), `SESSIONS.md`, `REFUELER-BRIDGE.md`,
  `legend-design-spec.md` (v1.2), `legend-scope.md` (v1.2), `legend-node-plan.md` (v1.0),
  `legend-ux-language.md` (v1.0), `legend-economics.md` (v1.0), `legend-enterprise-pricing.md` (v1.0).
- `MASTER.md` v1.0 produced: 343 lines, nine sections covering node topology, economics,
  enterprise commercial model, design system, copy register, scope by version, open items,
  and file index. Decisions only; pointers to detail files throughout.
- Standard session load from Legend-6 onwards: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md`
  + one specific detail file (4 files, not 9).
- Six CryptoRoadmap sessions scoped and queued: three Opus research sessions
  (primitives audit, transport/scanning layer, ZK architecture) + three Sonnet
  translation sessions. Timing: post-Phase-1 working explorer, before v2 build opens
  (target window: January 2027). See session queue below.
- Preliminary research note filed: Ristretto255 (Share Safari/WebCrypto performance,
  Legend ZK/DLEQ cleanliness, Tor payload reduction), Checklist PIR (Henzinger et al.
  2023) sublinear online phase, FMD (Beck et al. 2021) for Silent Payments scanning.
  Unverified performance claims flagged for calibration in CryptoRoadmap-1.
- Licence divergence noted: CLAUDE.md locks MIT; REFUELER-BRIDGE.md says Apache 2.0
  for Legend. CLAUDE.md is authoritative. Correct BRIDGE at next update.
- GBP denomination edit still open: locked in CLAUDE.md v1.5 but not applied to
  `legend-ux-language.md` §4/§8. Apply at next session touching that file.

### Files produced

- `MASTER.md` v1.0 — new file, committed `be071f5`
- `SESSIONS.md` — this entry, committed and pushed

### Carry-forward

- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **Custom FlokiNET quote** — open. Mandatory before provisioning node C.
- **legend-incident-protocol.md** — two Opus sessions once infrastructure is live.
- **GBP denomination edit** — apply to `legend-ux-language.md` §4/§8 next time that file is touched.
- **Design-spec token block** — `legend-design-spec.md` Design tokens section carries
  pre-CC-74 stale `--bg` values. Correct next time that file is edited.
- **BRIDGE licence correction** — Apache 2.0 → MIT for Legend section. Next BRIDGE update.

---

## Session Legend-4 · 6 Aug 2026

**Phase:** 1 pre-build harness — node status page spec
**Status:** Complete. `legend-design-spec.md` v1.1 → v1.2.

### Completed

- Status page section added to `legend-design-spec.md`: page purpose and scope,
  three-section structure (privacy mode / node status / canary status), per-section
  layout and typography spec, canary expired state (muted amber, typographic only,
  no icon), two new CSS tokens (`--canary-expired`), explicit list of what the page
  does not show, URL and navigation placement.
- Privacy mode: four states locked. Full splitting / Reduced splitting / Single node /
  Credential rotation in progress.
- Canary section visually separated from operational section by full-width rule.
- FROST integrity note added beneath canary blocks — `--text-tertiary`.
- Token additions: `--canary-expired: #B8860B` (Paper) / `#C9A227` (Carbon).

### Files changed

- `legend-design-spec.md` v1.1 → v1.2
- `SESSIONS.md`

---

## Session Legend-3B · 6 Aug 2026

**Phase:** 1 pre-build harness — UX language and information hierarchy
**Status:** Complete. `legend-ux-language.md` v1.0 produced.

### Completed (abbreviated)

- `legend-ux-language.md` v1.0: eight sections covering voice/register, landing hierarchy,
  query flow copy, result anatomy, modal copy, honest-scope statements, degraded mode
  notices, locked copy index (50 entries).

### Files produced

- `legend-ux-language.md` v1.0

---

*Legend-1 through Legend-3A archived. Decisions captured in `legend-economics.md` v1.0,
`legend-enterprise-pricing.md` v1.0, `legend-scope.md` v1.2, `legend-design-spec.md` v1.1.*

---

## Session queue

### Immediate — B9 dependent

**Legend-6** (first build session — gated on B9 Lightning node live)
- If B9 live: Eleventy shell at `refueler.io/legend`, SPA mount point, Paper/Carbon
  theme wired, query input idle state.
- If B9 not yet live: `legend-incident-protocol.md` first draft (node recovery,
  FROST re-keying, attack simulations).
- Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-design-spec.md`.

### Phase 1 build — Legend-6 through Legend-~15 (target: December 2026)

Eleventy shell → SPA query flow → result states → batch query / breach scenario →
Silent Payments display → Article 14 → `refueler.io/legend` live.
Each session: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + one detail file.

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
