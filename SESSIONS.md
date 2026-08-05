# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-0a · 5 Aug 2026

**Phase:** 1 pre-build harness — additional architecture (pre-economics)
**Status:** Complete. Discussion session. No output document. Locks recorded here.

### Completed

**NUT range — additional protocols reviewed:**

- NUT-12 DLEQ proofs locked v1: browser-side verification, mandatory.
  Detects mint tagging attacks (per-user key partitioning by a malicious or
  compelled mint). Each credential issuance must include a DLEQ proof; browser
  rejects credentials signed with a different key than the published keyset.
  Critical under the compelled-operator threat model documented in legend-node-plan.md.
- NUT-06 mint info locked v1: Legend mint advertises non-monetary status and
  the structural absence of mint/melt endpoints. Machine-readable form of the
  legal distinction in node-plan Section 4.
- NUT-19 idempotent issuance locked v1: in-memory only, short TTL, blinded
  content only. Gives retry safety for node failover without any session
  persistence — consistent with the ephemeral-session constraint.
- NUT-01/02 vocabulary adopted: each monthly FROST mint instance is one keyset,
  documented in NUT-02 terms for Cashu library compatibility.
- NUT-28 P2BK (Pay-to-Blinded-Key): noted for v2 Enterprise credential
  hardening. ECDH-derived key blinding per credential means a leaked Enterprise
  token cannot be linked to the client's long-lived public key by the mint.
  NUT-11 P2PK is sufficient for v1; P2BK upgrades it at v2.
- NUT-24 HTTP 402 Payment Required: noted for v2+ consideration. Cashu tokens
  as in-band HTTP payment for API resources is a clean per-query payment
  model requiring no account or session. No role at v1 — free unlimited tier
  means nothing to gate. Re-evaluate if pricing model changes post-v1.
- NUT-13 and NUT-09 permanently rejected: deterministic/restorable credentials
  contradict the ephemeral-session architecture. Queue for legend-scope.md
  permanent-out list.
- NUT-17 WebSockets rejected v1. Re-evaluate for Enterprise monitoring sessions
  in v2 where long-lived authenticated connections are already the model.
- NUT-21 / NUT-22 reviewed and rejected: NUT-21 is OAuth 2.0 / OIDC clear
  authentication — requires user registration and a JWT containing user identity.
  NUT-22 issues blind tokens to that identified user. Blindness is within a
  registered-user anonymity set only. Wrong architecture for Legend's anonymous
  credential model. No alignment possible. Verification task from Legend-0a
  pre-session is closed; do not revisit.
- NUT-27 Nostr mint backup permanently rejected: deterministic key derivation
  from a seed for mint restoration is the exact persistence model Legend's
  rotating-instance architecture exists to prevent.
- Monetary NUTs (04, 05, 08, 14, 15, 20, 23, 25, 29, 30) structurally absent.
  Absence is load-bearing for the non-monetary legal claim. NUT-06 mint info
  document advertises their absence explicitly.

**Esplora filtered data — decisions locked:**

- Raw block binary: filtered, all tiers. No user in Legend's audience needs
  raw blocks from an explorer. Pure bandwidth cost with no privacy story.
- Script assembly strings: restored v1. Cheap to serve (decoded at query time,
  no index cost). On the critical path to v3 estate tooling — solicitor
  verifying a Liana/Miniscript inheritance script needs the script rendered.
  Plain-language script interpretation scoped v2.
- Wallet-level metadata: permanently out. Scope doc already covers this;
  no private key handling extends to no wallet metadata.
- Address history and spending history: full, with honest documented caps,
  paginated. Cap stated in UI rather than silently truncated. Enterprise raised
  limits as a config value only — a load differentiator, not a data-access one.
- Prefix index (NO_ADDRESS_SEARCH): REQUIRED ON. Stage-1 k-anonymity query
  (script-hash prefix → candidate row IDs) depends on this index. Setting
  NO_ADDRESS_SEARCH=1 is forbidden on Legend nodes. User-facing address
  autocomplete stays OFF — autocomplete leaks partial queries keystroke by
  keystroke. Index present; feature suppressed. Foot-gun note required in all
  build sessions touching node or electrs configuration.
- Data parity across tiers locked: free and Enterprise serve identical chain
  data. Tiers differentiate on transport (Tor, Double Ratchet), load limits,
  and wrapper — never on data access. Data-tiering converts "privacy is the
  default" into "privacy is a product."

**Merkle inclusion proofs — locked v1:**

- Upstream Esplora endpoints restored (/tx/:txid/merkle-proof and merkleblock
  variant). In-browser SPV verification: SHA-256d up the branch, compare to
  block header merkle root. Small JS, no installed software required.
- Cross-node header fetch: proof retrieved from one node, block header from
  a different node. Role rotation means a single lying node is caught by
  cross-node header mismatch. Forging a verified result requires the same
  two-node collusion that already bounds the v1 threat model. The multi-node
  architecture bought for query privacy doubles as response-integrity
  verification at no additional cost.
- Rationale: a compelled operator can be forced to log; he cannot forge a
  Merkle branch. Addresses the honest limitation stated in node-plan Section 6
  (UK operator / IPA 2016) — response verifiability is the mitigation where
  query-log compulsion cannot be fully ruled out.
- Result panel: one quiet line — "Inclusion verified against block header" —
  in the calm register the design spec mandates.
- Proof export artefact (txid, Merkle branch, header, verification
  instructions): v2. Estate report integration (proof as court-admissible
  evidence of holdings at block height): v3.
- Witness commitment verification: deferred indefinitely. Protects against
  malleability display bugs; no user in Legend's audience is threatened by them.

### Carry-forward to Legend-1

- Bandwidth model: +1–2 KB per verified result plus 80-byte cross-node header
  fetch. Immaterial to totals; include as a line in bandwidth overhead
  quantification.
- No topology change. €170–220/month opening figure stands.
- Storage: prefix/address index confirmed non-strippable. ~1 TB+ index
  estimate in legend-node-plan.md already assumes it present.
- Queue legend-scope.md edits at next scope session: NUT-13/09 permanent-out,
  NUT-28 v2 Enterprise credential note, NUT-24 v2+ note, plain-language
  script rendering v2, Merkle proof scope per version (v1/v2/v3).
- Open Legend-1: economics session. Load standard six context files plus
  legend-node-plan.md. Output: legend-economics.md (new file) +
  legend-enterprise-pricing.md (first draft, cost basis only).
  legend-node-plan.md is NOT appended — economics lives in its own file.

---

## Session Legend-0 · 5 Aug 2026

**Phase:** 1 pre-build harness — node infrastructure topology
**Status:** Complete. `legend-node-plan.md` committed at `2de5a8a`.

### Completed

- Three nodes locked: Hetzner Falkenstein (DE), Hetzner Helsinki (FI),
  FlokiNET Reykjavik (IS). Two providers, two legal jurisdictions.
- Per-node spec locked: 8+ cores, 64 GB RAM, 2 TB NVMe dedicated.
  Hetzner AX-class for DE/FI, FlokiNET dedicated for IS.
- Full index per node confirmed — roles are logical, not physical data partitions.
- No gateway architecture. Browser talks to nodes directly. Locked permanently.
- Role-split query topology locked: stage-1 (index node, script-hash prefix,
  k-anonymity set) → stage-2 (data node, opaque row IDs). Browser randomises
  node selection per query from signed manifest.
- Signed node manifest architecture locked: expansion to four+ nodes is a
  manifest update, not a code change.
- NUT-00 nested blinding across both stages: blinding factors ephemeral,
  client-side, discarded after query. Post-hoc node collusion cannot reconstruct.
- NUT-07 token state check, NUT-11 P2PK Enterprise credentials,
  NUT-29 batched minting — all confirmed for Legend mint.
- Rotating mint instances locked: monthly cadence, independent FROST keypair
  per window, key destruction at window close, blind exchange protocol for
  credential migration.
- Separate Legend Cashu mint confirmed: non-monetary, independent of
  Share infrastructure.
- FROST 2-of-3 threshold signatures locked: mint key management and canary
  signing. Full key never assembled on any node. Single jurisdiction seizure
  yields one unusable share.
- Warrant canary: one per node, independently signed, daily publication,
  72-hour expiry, block-hash freshness oracle, silence = death. Three
  publication channels: node static file, GitHub mirror, per-node Nostr npub.
- UK operator caveat locked: IPA 2016 scope stated plainly in privacy explainer,
  Enterprise contract materials, and below-the-fold legend page copy.
- Argon2id for all key material at rest on all three nodes.
- Signal-style Double Ratchet (X3DH + ratchet per query) confirmed for
  Enterprise transport, v2.
- ML-KEM-768 hybrid with X25519 confirmed for Enterprise post-quantum transport,
  v2. Credential layer post-quantum deferred as research problem.
- BLAKE3 role on x86 nodes documented honestly: bulk parallelism benefit,
  not per-lookup parity with SHA-NI. Consensus hashing untouched.
- Graceful degradation locked: N-1 = reduced splitting (documented),
  N-2 = single node with honest browser notice, never silent.
- Enterprise isolation deferred to v2. Contract language designed in Legend-2.
- €96/month figure in legend-design-spec.md confirmed stale.
  Revised estimate: €170–220/month. Opens Legend-1.

### Files committed

- `legend-node-plan.md` v1.0 — commit `2de5a8a`

### Carry-forward

- Update `legend-session-briefs.md`: mark Legend-0 COMPLETE. ✓
- Open Legend-0a before Legend-1: additional architecture questions. ✓ (complete)
- Legend-1: economics session. legend-node-plan.md must be committed. ✓

---

## Session Ad-hoc · 5 Aug 2026

**Phase:** 0 — Foundation (pre-build harness)
**Status:** Research, threat modelling, scope expansion

### Completed

- Threat model expanded: Mempool/Blockstream reframed as data harvesters.
  Documented: device correlation, wallet fingerprinting, geographic inference,
  wealth correlation, state-level bulk collection, MLAT subpoena risk,
  family office session-linking attack surface.
- Jurisdiction mobility threat documented: Five Eyes MLAT reach to US/Canadian
  infrastructure. Pre-departure query behaviour as legal evidence. → Article 22.
- Family office attack surface documented: corporate IP, IT logging, client
  session-linking via query sequence. → Article 21.
- Estate planning confirmed as v3 professional use case. → Article 23.
- /legend/verify confirmed as v2 placeholder: dedicated URL, ZK proof verification.
- Cashu mint health confirmed as v2 feature: on-chain reserve check, mint blindness preserved.
- Lightning node pubkey → private channel correlation confirmed as v2 feature.
- Node count value documented: PIR privacy, geographic legal distribution,
  Enterprise isolation, per-node warrant canary architecture.
- Traffic projections: year 1–2 realistic 5,000–20,000 MAU. Year 3–7
  10–50M queries/month if Silent Payments adoption follows SegWit curve.
- Enterprise pricing model: family office £1,500–3,500/month, merchant/franchise
  bundled with POS, estate solicitor per-report £50–150.
- Swan Bitcoin assessment: most likely to fork. Approach only post-audit.
- Mempool.space Enterprise ~$500–2,000/month. Blockstream no paid tier.
- Pre-build harness plan locked: 6 sessions before Multi-8,
  producing legend-node-plan.md, legend-enterprise-pricing.md, legend-ux-language.md.
- Session naming convention confirmed: Multi-[n] through Multi-8,
  then Legend-[n] from first build session onwards.

### Files updated

- `legend-articles-list.md` → v1.1: Articles 21, 22, 23 added.
- `legend-scope.md` → v1.1: v2 and v3 expanded. Carry-forward updated.
- `SESSIONS.md` → this entry.

### Carry-forward

- Commit all three files from `/Users/rajeshtaylor/Documents/refueler-legend/`
- Next session: node infrastructure costing. Produces `legend-node-plan.md`. No code.

---

*Next session: Legend-1 — infrastructure costs and scaling economics*
*Produces: legend-economics.md + legend-enterprise-pricing.md (first draft)*
*Load: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md, legend-design-spec.md, legend-scope.md, legend-node-plan.md, legend-session-briefs.md*
