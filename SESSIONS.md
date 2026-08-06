# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

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
