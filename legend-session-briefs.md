# legend-session-briefs.md — refueler-legend
> **Version:** 1.2 | **Created:** Ad-hoc · 5 Aug 2026 | **Updated:** Legend-1 · 5 Aug 2026
> Pre-build harness session briefs for Legend-0, Legend-0a, Legend-1, and Legend-2.
> Load alongside the standard five context files when opening each session.
> Mark each brief COMPLETE after the session closes and its output is committed.
> Archive this file after Legend-2. Do not delete.

---

## Model and settings guidance

**Legend-0:** Opus. Extended thinking ON.
Reason: PIR sharding architecture contains a genuine contradiction
(browser SPA splitting queries without a gateway node becoming a surveillance
point) that requires working through rather than papering over.

**Legend-0a:** Opus. Extended thinking ON.
Reason: NUT range review and Esplora filter decisions require working through
architectural interactions before the economics session locks the cost model.

**Legend-1:** Opus. Extended thinking ON.
Reason: Scaling cost model involves compounding estimates. Wrong assumptions
early produce confidently wrong conclusions in the Enterprise pricing session.

**Legend-2:** Opus. Extended thinking OFF.
Reason: Commercial structuring session — judgment calls, not logical derivations.
Standard Opus is sufficient.

**Legend-3 onwards:** Sonnet. Extended thinking OFF.
Build sessions — code generation, file edits, terminal commands.

---

## Legend-0 — Node infrastructure topology
**Status:** COMPLETE — commit `2de5a8a` · 5 Aug 2026
**Produces:** `legend-node-plan.md` (infrastructure section)
**Prerequisite:** None. First session in the pre-build harness.

---

## Legend-0a — Additional architecture session
**Status:** COMPLETE · 5 Aug 2026
**Produces:** No output document. Locks recorded in SESSIONS.md.
**Prerequisite:** Legend-0 complete.

### Locks carried forward to legend-scope.md (queue for next scope session)

- NUT-13 and NUT-09: add to permanent-out list. Deterministic/restorable
  credentials contradict ephemeral-session architecture.
- NUT-28 P2BK: add v2 Enterprise credential hardening note.
- NUT-24 HTTP 402: add v2+ per-query payment model note.
- Plain-language script rendering: add as scoped v2 item.
- Merkle proof scope: v1 (in-browser verification), v2 (proof export artefact),
  v3 (estate report integration).

### Key decisions locked

- NUT-12 DLEQ: v1, mandatory, browser-side.
- NUT-06 mint info: v1, non-monetary status advertised.
- NUT-19 idempotency: v1, in-memory, short TTL, blinded content only.
- NUT-21/22: rejected. OAuth/OIDC clear auth is wrong architecture for Legend.
- NUT-28: v2 Enterprise credential hardening.
- NUT-24: v2+ per-query payment model consideration.
- NO_ADDRESS_SEARCH=1: forbidden on all Legend nodes permanently.
- Data parity across tiers: locked. Data access never differentiates tiers.
- Merkle inclusion proofs: v1, cross-node header fetch, in-browser SPV verify.

---

## Legend-1 — Infrastructure costs and scaling economics
**Status:** COMPLETE · 5 Aug 2026
**Produces:** `legend-economics.md` (new file) + `legend-enterprise-pricing.md` (v0.1, cost basis only)
**Prerequisite:** Legend-0 complete. `legend-node-plan.md` committed. ✓

**Note (updated Legend-0a):** Economics content lives in a new standalone file
`legend-economics.md` — NOT appended to `legend-node-plan.md`. That file is
already long and the infrastructure topology should remain clean and
independently referenceable. Legend-1 output is two files.

---

Session: Legend-1
Phase: 1 pre-build harness — infrastructure costs and scaling economics
Type: Planning and discussion only. No code. No terminal commands.
Output: legend-economics.md (new file) +
legend-enterprise-pricing.md (first draft, infrastructure cost basis only)

Context files loaded: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md,
legend-design-spec.md, legend-scope.md, legend-node-plan.md,
legend-session-briefs.md

Legend-0 must be complete before this session opens.
legend-node-plan.md must exist and be committed. ✓

---

WHAT THIS SESSION DECIDES:

Node topology is locked in legend-node-plan.md. This session prices it,
models how costs scale with traffic, and establishes the infrastructure
cost basis that justifies the Enterprise pricing model.

QUESTIONS TO WORK THROUGH:

1. V1 LAUNCH COSTS — MONTHLY TOTAL
   - Price each node from legend-node-plan.md against current Hetzner
     dedicated and VPS pricing.
   - Include: compute, storage (chain data volume from Legend-0),
     bandwidth (estimate query volume at 5,000–20,000 MAU),
     Tor hidden service overhead for Enterprise endpoint.
   - What is the honest monthly total at v1 launch?
   - How does this compare to the ~€96/month figure in legend-design-spec.md —
     is that figure still accurate given the node topology locked in Legend-0?
   - Legend-0 revised estimate: €170–220/month. Verify or refine against
     current Hetzner AX-class and FlokiNET dedicated pricing.

2. SCALING COST MODEL
   - At 100,000 MAU: what does the node infrastructure cost?
   - At 1M queries/month: what does it cost?
   - At 10M queries/month (year 3–7 realistic ceiling): what does it cost?
   - At what query volume does the current architecture require additional nodes?
   - What is the cost-per-query at each scale point?
   - Silent Payments scanning is computationally heavier than address lookups —
     model its cost separately. At what adoption rate does it require
     dedicated scanning nodes?
   - Merkle proof overhead: +1–2 KB per verified result plus 80-byte cross-node
     header fetch (locked Legend-0a). Include in bandwidth quantification.

3. BANDWIDTH AND CHAIN DATA
   - Bitcoin chain is ~650 GB now (node-plan figure), growing ~60 GB/year.
   - Every node holds a full indexed copy — storage cost compounds.
     This is locked; full index per node is required by the privacy architecture.
   - Prefix/address index is non-strippable (locked Legend-0a). The ~1 TB+
     index estimate already assumes it present.
   - Bandwidth cost for PIR role-split: the client makes two fetch() calls
     (stage-1 and stage-2 to different nodes). Quantify the overhead vs a
     standard single-node Esplora query.
   - PIR sharding bandwidth: client downloads candidate row ID set from stage-1
     (larger than a direct answer by design). Quantify this overhead.

4. THE ONE ENTERPRISE CLIENT BREAK-EVEN
   - legend-design-spec.md states: "one Enterprise client covers years
     of infrastructure."
   - Verify or revise this claim against the actual cost model.
   - At what Enterprise contract value does one client cover 12 months
     of infrastructure?
   - At what value does one client cover 24 months?
   - This number anchors the minimum Enterprise contract price in Legend-2.

5. SELF-HOSTING ECONOMICS
   - Legend is open source. Self-hosters run their own nodes.
   - What does a self-hosted Legend instance cost on Hetzner or a Raspberry Pi?
   - Does self-hosting undercut the Enterprise offer or strengthen it?
   - What does the AI-assisted setup guide need to cover for a non-technical
     self-hoster to get a working instance?
   - Self-hosted instances use a simpler single-key rotating-key-set Cashu mint
     rather than the FROST 2-of-3 multi-node ceremony (locked Legend-0).
     Note this distinction in the economics document.

6. INFRASTRUCTURE COST AS HONEST SIGNAL
   - The cost model should be published honestly — users should know
     what it costs to run Legend and why the Enterprise tier exists.
   - Does the "how the free tier works" section of refueler.io/legend
     need updating after this session?

OUTPUT THIS SESSION PRODUCES:
- legend-economics.md (new file):
  Monthly cost at v1, scaling cost model, bandwidth overhead quantified,
  break-even analysis, self-hosting cost estimate, honest signal framing.
- legend-enterprise-pricing.md (first draft, infrastructure cost basis only):
  Cost floor per tier established. Full pricing model completed in Legend-2.

Close session by updating SESSIONS.md with Legend-1 entry and carry-forward to Legend-2.
Mark this brief COMPLETE in legend-session-briefs.md.

"Nothing stops this train."

---

## Legend-2 — Enterprise packaging and commercial model
**Status:** PENDING
**Produces:** `legend-enterprise-pricing.md` (completed)
**Prerequisite:** Legend-0, Legend-0a, and Legend-1 complete. All output documents committed.

---

Session: Legend-2
Phase: 1 pre-build harness — Enterprise packaging and commercial model
Type: Planning and discussion only. No code. No terminal commands.
Output: legend-enterprise-pricing.md (completed)

Context files loaded: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md,
legend-design-spec.md, legend-scope.md, legend-node-plan.md,
legend-economics.md, legend-enterprise-pricing.md (first draft from Legend-1),
legend-session-briefs.md

Legend-0, Legend-0a, and Legend-1 must be complete before this session opens.

---

WHAT THIS SESSION DECIDES:

Infrastructure costs are modelled in legend-economics.md. This session
builds the full commercial model on top of that cost basis: what each
tier costs, what it includes, how it is sold, and how the relationship
is structured. Output is a complete legend-enterprise-pricing.md that
any future session — sales, legal, onboarding — can reference.

QUESTIONS TO WORK THROUGH:

1. FAMILY OFFICE TIER
   - Price range discussed in ad-hoc session: £1,500–3,500/month.
     Validate against Legend-1 break-even and comparable services
     (Mempool Enterprise ~$500–2,000/month).
   - What is included at each price point within the range?
   - What triggers the higher end: node count, SLA level,
     Silent Payments scanning, dedicated nodes?
   - Is the family office tier invite-only at v1?
     If yes, what is the onboarding process and who is the named contact?
   - What does the contract look like: monthly rolling, annual, minimum term?
     What does their PI insurance and compliance team need to see?
   - What SLA is honest to promise at v1 infrastructure scale?
     Do not promise what the node topology cannot deliver.

2. MERCHANT AND FRANCHISE TIER
   - Refueler POS merchants are the warm pipeline.
   - Is Legend included in a Refueler merchant tier above a threshold,
     or is it always an add-on?
   - What is the add-on price: £200–500/month?
   - A franchise with 20 locations: one Legend contract covering all.
     Per-location fee, flat franchise rate, or bundled into POS contract?
   - What does the merchant's accountant actually need from Legend —
     address verification, payment confirmation, denomination toggle —
     and is that a different feature set from the family office tier?

3. ESTATE SOLICITOR TIER — PER-REPORT
   - Price range: £50–150 per verified estate report.
   - What does a verified estate report contain: block-height balance,
     ZK proof, multi-sig quorum check, plain-language summary?
   - This is a v2/v3 feature — but the pricing model is designed now.
   - How is a per-report purchase made without creating an account?
   - Lightning payment for the report: pseudonymous. Document honestly.
   - What does a UK solicitor's PI insurer need to see to accept
     a Legend estate report as evidence?

4. THE SWAN BITCOIN SCENARIO
   - Swan will likely fork if the code is ahead of the relationship.
   - At what point does the Enterprise API become complex enough that
     forking costs more than paying?
   - What specific feature combination makes the fork calculation
     unfavourable: ZK balance proofs + warrant canary + audit report
     + /legend/verify + SLA?
   - Correct approach timing: post-audit, post-/legend/verify, post what?
   - If they fork: what does MIT licence permit, what does it not permit,
     and is there a contribution-back norm worth establishing publicly
     before they fork?

5. OPEN SOURCE AND THE ENTERPRISE SALE
   - How is "don't trust us, read the code" framed in the sales conversation?
   - What is the honest answer when a family office asks:
     "why should we pay you when we could run this ourselves?"

6. PRICING DOCUMENT STRUCTURE
   - legend-enterprise-pricing.md must be usable as a sales reference,
     not just an internal planning document.
   - Tone: authoritative, precise, not marketing. States what it cannot
     do as clearly as what it can. A document a compliance officer trusts.
   - Sections needed: tier summary table, per-tier inclusions and exclusions,
     contract structure and onboarding process, SLA commitments,
     self-hosting option, Swan scenario notes (internal only, not published),
     honest scope statements per tier.

OUTPUT THIS SESSION PRODUCES:
legend-enterprise-pricing.md (completed):
- Tier summary table: free, family office, merchant/franchise,
  estate solicitor, Enterprise API
- Per-tier inclusions and exclusions
- Contract structure and onboarding process
- SLA commitments (honest, calibrated to actual infrastructure)
- Self-hosting option and its relationship to paid tiers
- Swan scenario notes (internal, not published)
- Honest scope statements per tier

Close session by updating SESSIONS.md with Legend-2 entry.
Carry-forward: Legend-3 is UX language and information hierarchy session.
Mark this brief COMPLETE in legend-session-briefs.md.

"Nothing stops this train."
