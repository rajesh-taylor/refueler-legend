# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-1 · 5 Aug 2026

**Phase:** 1 pre-build harness — infrastructure costs and scaling economics
**Status:** Complete. `legend-economics.md` and `legend-enterprise-pricing.md` (v0.1) produced.

### Completed

**Pricing confirmed against live data (screenshots, 5 Aug 2026):**
- Hetzner AX52: €59/month ex-VAT, unlimited traffic, €39 one-off setup.
- FlokiNET dedicated (Iceland): €180–300/month, 32 TB/month bandwidth cap on 1 Gbit.
  Custom quote required for exact NVMe match — budget to upper end of range.

**V1 launch cost locked:**
- Node A (Hetzner Falkenstein): €59/month
- Node B (Hetzner Helsinki): €59/month
- Node C (FlokiNET Reykjavik): ~€240/month midpoint
- **Total: ~€360/month midpoint, range €340–420/month**
- Year-one effective (setup fees amortised): ~€365/month
- Previous estimates (€96, €170–220) are confirmed wrong. Do not use.

**Scaling model confirmed:**
- Architecture handles ~100M queries/month on current topology before load
  forces node four. Traffic alone does not drive node four — Enterprise
  isolation (v2) does.
- Per-query egress: ~150 KB worst case (2–3× single-node Esplora by design).
- FlokiNET 32 TB cap is not a constraint at any realistic v1–v2 scale.
  Monitor from launch; review if node C egress approaches 5 TB/month.

**Silent Payments tweak index locked as v1 build requirement:**
- Precomputed per-block tweak index is the difference between shippable and not.
  Without it, SP scanning is 1.5–3 core-hours per scan — not viable.
  With it: ~8 minutes parallelised, 5–10 concurrent full-range scans per node.
- Storage overhead: ~30–60 GB per node. Within spec.
- Dedicated SP scanning nodes: v2/v3 line item, not a v1 cost.
- Queue for legend-scope.md: add tweak index as v1 build requirement under
  Silent Payments.

**Storage confirmed:**
- Per-node at v1: ~1.2–1.3 TB total (chain + index + tweak index).
- Growth: ~90–100 GB/year per node.
- Headroom to ~2029 on 2 TB spec confirmed.
- Separate drives (chain/index split, no RAID): confirmed correct.

**Enterprise break-even confirmed:**
- Annual infrastructure: ~€4,320/year (~£3,700).
- Cost-recovery contract floor: ~£310/month.
- Family office at floor (£1,500/month) covers ~4.8 years of infrastructure.
- "One Enterprise client covers years of infrastructure" confirmed and understated.

**Self-hosting:**
- Pi 5 route: ~£160–210 one-off, ~£10–15/year electricity.
- Self-hosted mint: single-key rotating-key-set, not FROST 2-of-3. Stated plainly.
- Self-hosting strengthens the Enterprise offer. Honest answer to "why pay?"
  is the institutional wrapper: FROST, canaries, jurisdiction, SLA, named contact.

**Copy consequence:**
- legend-design-spec.md "~€96/month" must become "~€360/month".
  Queue for next session touching that file — do not edit mid-harness.
- Below-the-fold "how the free tier works" copy: draft produced in
  legend-economics.md §6. Final copy in Legend-3 (UX language session).

### Files produced

- `legend-economics.md` v1.0 — new file
- `legend-enterprise-pricing.md` v0.1 — first draft, cost basis only

### Carry-forward to Legend-2

- **Custom FlokiNET quote required before provisioning node C.**
- **legend-scope.md queued edits:** SP tweak index as v1 build requirement;
  NUT-13/09 permanent-out; NUT-28 v2 Enterprise note; NUT-24 v2+ note;
  plain-language script rendering v2; Merkle proof scope v1/v2/v3.
- **legend-design-spec.md queued edit:** €96 → €360 correction.
- **Minimum Enterprise contract locked:** £310/month cost-recovery floor.
- **Legend-2 opens with both new files committed.**

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

- Raw block binary: filtered, all tiers.
- Script assembly strings: restored v1.
- Wallet-level metadata: permanently out.
- Address history and spending history: full, paginated, cap stated in UI.
- Prefix index (NO_ADDRESS_SEARCH): REQUIRED ON. Forbidden on all Legend nodes.
  User-facing autocomplete stays OFF.
- Data parity across tiers locked.

**Merkle inclusion proofs — locked v1:**

- Upstream endpoints restored. In-browser SPV verification.
- Cross-node header fetch: proof from one node, header from another.
- Result panel: one quiet line — "Inclusion verified against block header."
- Proof export artefact: v2. Estate integration: v3.
- Witness commitment verification: deferred indefinitely.

### Carry-forward to Legend-1

- €170–220/month opening figure: superseded by Legend-1 confirmed figure (€360).
- Queue legend-scope.md edits at next scope session.

---

## Session Legend-0 · 5 Aug 2026

**Phase:** 1 pre-build harness — node infrastructure topology
**Status:** Complete. `legend-node-plan.md` committed at `2de5a8a`.

### Completed

- Three nodes locked: Hetzner Falkenstein (DE), Hetzner Helsinki (FI),
  FlokiNET Reykjavik (IS). Two providers, two legal jurisdictions.
- Per-node spec locked: 8+ cores, 64 GB RAM, 2 TB NVMe dedicated.
- Full index per node confirmed — roles are logical, not physical data partitions.
- No gateway architecture. Browser talks to nodes directly. Locked permanently.
- Role-split query topology locked. Signed node manifest locked.
- NUT-00 nested blinding, NUT-07, NUT-11, NUT-29 confirmed.
- Rotating mint instances, FROST 2-of-3, warrant canary architecture all locked.
- UK operator caveat locked. Argon2id at rest. Double Ratchet v2. ML-KEM-768 v2.
- Graceful degradation locked: N-1 documented, N-2 honest browser notice.
- Enterprise isolation deferred to v2.
- €96/month stale. Revised to €170–220 (Legend-0); superseded by €360 (Legend-1).

### Files committed

- `legend-node-plan.md` v1.0 — commit `2de5a8a`

---

*Next session: Legend-2 — Enterprise packaging and commercial model*
*Produces: legend-enterprise-pricing.md (completed)*
*Load: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md, legend-design-spec.md,*
*legend-scope.md, legend-node-plan.md, legend-economics.md,*
*legend-enterprise-pricing.md (v0.1), legend-session-briefs.md*
