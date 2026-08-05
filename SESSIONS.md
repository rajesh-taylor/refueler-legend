# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-2 · 5 Aug 2026

**Phase:** 1 pre-build harness — Enterprise packaging and commercial model
**Status:** Complete. `legend-enterprise-pricing.md` v1.0 (completed) produced. Pre-build harness closed.

### Completed

**Family office tier locked:**
- Pricing ladder, capability-defined: £1,500/month (v1) → £2,500/month (v2:
  Tor API + Double Ratchet + ML-KEM-768) → £3,500/month (v2+: dedicated node
  isolation). Only the v1 band is sold at v1 — upper bands quoted as scheduled
  upgrades with version dependencies stated. Never charged before live.
- Invite-only at v1, capped at **five clients**. Truthful solo-operator
  constraint, stated plainly to prospects.
- Named contact: Rajesh Taylor. No account team — the founder answers.
- Contract: 3-month minimum, then monthly rolling, 60 days' notice either
  side. Annual prepay at eleven months' price. Short structure deliberate —
  signals confidence, respects key-person risk.
- Compliance/PI pack defined: architecture summary (precise v1 claim language),
  honest-scope statement, UK operator caveat in full, canary verification
  procedure, incident commitments, open-source verification path, audit roadmap.
- Onboarding: warm referral → architecture call (operator caveat raised by us,
  unprompted) → compliance pack → contract + NUT-11-bound credentials →
  quarterly reviews.

**SLA locked — calibrated to actual topology:**
- 99.0% monthly availability (available = ≥2 nodes, role-split intact).
- Full-splitting mode ≥95% of month.
- 4 UK business-hour support response, Mon–Fri 09:00–18:00 London.
- Canary expiry notification automated within 12 hours.
- Incident notice within 24h; written post-incident summary within 5 business days.
- Service credit: 10% of monthly fee below 97%.
- Explicitly not promised: 24/7, 99.9%, sub-hour response. One human in London.

**Merchant/franchise tier locked:**
- Always an add-on. Never bundled into the POS contract — bundling is the
  cross-product seam the scope discipline forbids.
- £250/month per business entity; £500/month franchise flat up to 25 locations
  (custom above). Flat because the accountant is the user, not the till.
- Sold only into the existing Refueler POS merchant base at v1. No cold sales.
- Monthly rolling, 30 days' notice.
- Feature set = free tier (data parity) + accountant onboarding + named support
  + processable invoice. Sold as exactly that.
- Below cost-recovery standalone (reaffirmed) — sustainable only under family
  office cross-subsidy. Pipeline tier, never primary revenue.

**Estate solicitor tier locked (designed now, sold v2/v3):**
- £50 block-height balance statement (v2 — Merkle proof export dependency).
- £150 full verified estate report: + ZK balance proof, multi-sig quorum in
  plain language, methodology statement (v3 — /legend/verify dependency).
- Two payment routes, both stated honestly: Lightning at point of purchase
  (no account; pseudonymous, not anonymous — stated in flow) and invoiced
  BACS for firms (identified relationship; procurement reality of regulated
  firms, stated not hidden). Report content client-side generated either way.
- PI insurer pack: methodology, proof-and-limitation statement, independent
  third-party re-verification procedure, legal-acceptance caveat (admissibility
  outside Legend's control).

**Swan scenario assessed (INTERNAL — in pricing doc, marked for removal from
any external share):**
- MIT permits full closed fork with no contribution back. Plan on that basis.
- Unforkable assets: canary uptime track record (time-based), audit report
  (attaches to deployment, not source), FROST/rotation operational discipline,
  the SLA relationship.
- Fork calculation turns unfavourable at: audit published + /legend/verify
  live + ≥12 months unbroken canary history + Enterprise credential
  architecture in production.
- Approach timing: after all of the above. Not before — earlier approach
  advertises the code before the moat exists.
- Contribution-back norm: publish via CONTRIBUTING.md at repo build-out.
  No legal force; mechanism is reputational pricing established pre-fork.
  **Queued for first build session.**

**Open source and the Enterprise sale:**
- "Read the code" framed as due-diligence enabler: every contract claim
  independently verifiable against source + reproducible builds — a
  verification path no closed competitor offers.
- "Why pay?" answer locked per legend-economics.md §5: institutional wrapper
  (FROST, canaries, jurisdictions, SLA, compliance pack, named contact).
  Bloomberg-terminal analogy confirmed for sales conversation.

### Files produced

- `legend-enterprise-pricing.md` v1.0 — completed, replaces v0.1 stub

### Carry-forward to Legend-3 (UX language and information hierarchy)

- **Queued file edits still outstanding** (first session touching each file):
  - `legend-scope.md`: SP tweak index as v1 build requirement; NUT-13/09
    permanent-out; NUT-28 v2 note; NUT-24 v2+ note; plain-language script
    rendering v2; Merkle proof scope v1/v2/v3.
  - `legend-design-spec.md`: €96 → €360 correction.
- **Custom FlokiNET quote required before provisioning node C** (still open).
- **CONTRIBUTING.md contribution-back norm** — queue for first build session.
- Legend-3 produces `legend-ux-language.md`: final below-the-fold free-tier
  copy (draft in legend-economics.md §6), result-state language, privacy
  explainer copy, honest-scope statements in user register.
- **Pre-build harness complete.** Legend-0, 0a, 1, 2 all closed.
  `legend-session-briefs.md` marked complete and ready for archive.

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

---

## Session Legend-0a · 5 Aug 2026

**Phase:** 1 pre-build harness — additional architecture (pre-economics)
**Status:** Complete. Discussion session. No output document. Locks recorded here.

### Completed (abbreviated — full detail in archive)

- NUT-12 DLEQ locked v1 (browser-side, mandatory). NUT-06 mint info locked v1.
  NUT-19 idempotency locked v1. NUT-01/02 vocabulary adopted.
- NUT-28 P2BK noted v2. NUT-24 HTTP 402 noted v2+.
- NUT-13/09 permanently rejected. NUT-17 rejected v1. NUT-21/22 rejected.
  NUT-27 permanently rejected. Monetary NUTs structurally absent.
- Esplora filtered data locked: raw block binary filtered; script assembly
  restored v1; prefix index required on; data parity across tiers locked.
- Merkle inclusion proofs locked v1: cross-node header fetch, in-browser SPV.
  Proof export v2. Estate integration v3.

---

*Next session: Legend-3 — UX language and information hierarchy*
*Produces: legend-ux-language.md*
*Load: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md, legend-design-spec.md,*
*legend-scope.md, legend-node-plan.md, legend-economics.md,*
*legend-enterprise-pricing.md (v1.0), + apply queued edits to*
*legend-scope.md and legend-design-spec.md at session open.*
