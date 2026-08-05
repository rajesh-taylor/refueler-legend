# legend-session-briefs.md — refueler-legend
> **Version:** 1.0 | **Created:** Ad-hoc · 5 Aug 2026
> Pre-build harness session briefs for Legend-0, Legend-1, and Legend-2.
> Load alongside the standard five context files when opening each session.
> Mark each brief COMPLETE after the session closes and its output is committed.
> Archive this file after Legend-2. Do not delete.

---

## Model and settings guidance

**Legend-0:** Opus. Extended thinking ON.
Reason: PIR sharding architecture contains a genuine contradiction
(browser SPA splitting queries without a gateway node becoming a surveillance
point) that requires working through rather than papering over.

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
**Status:** PENDING
**Produces:** `legend-node-plan.md` (infrastructure section)
**Prerequisite:** None. First session in the pre-build harness.

---

Session: Legend-0
Phase: 1 pre-build harness — node infrastructure topology
Type: Planning and discussion only. No code. No terminal commands.
Output: legend-node-plan.md (infrastructure section — topology, locations, specs,
PIR sharding architecture, warrant canary design)

Context files loaded: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md,
legend-design-spec.md, legend-scope.md, legend-session-briefs.md

---

WHAT THIS SESSION DECIDES:

Legend's v1 privacy architecture depends on PIR-inspired query sharding across
multiple Hetzner nodes. The design-spec says 3–5 nodes. The scope says the same.
Neither document specifies locations, specs, topology, or how the sharding
actually works at the network level. This session locks all of that.

QUESTIONS TO WORK THROUGH:

1. NODE COUNT AND LOCATIONS
   - What is the minimum node count for PIR sharding to be meaningfully private
     in v1 (before Spiral PIR in v2)?
   - Which Hetzner datacentre locations make sense: Falkenstein (DE),
     Helsinki (FI), Ashburn (US), Singapore?
   - What does geographic distribution buy legally — which combinations place
     nodes in different jurisdictions meaningfully for MLAT purposes?
   - What is the right v1 count vs the right v2/v3 count, and can we add nodes
     without architectural changes?

2. NODE SPECS
   - What does each node need to run: indexed Bitcoin chain data,
     the query API, the sharding layer?
   - What are the RAM, storage, and CPU requirements for a fully indexed
     Bitcoin node running the Esplora API?
   - Hetzner dedicated vs VPS — which is appropriate at v1 launch scale?
   - Storage growth rate: Bitcoin chain is ~600GB now, growing ~60GB/year.
     Does each node hold a full copy or do they shard the data itself?

3. PIR SHARDING TOPOLOGY — HOW IT ACTUALLY WORKS IN v1
   - In v1 we have PIR-inspired sharding, not full Spiral PIR (that is v2).
   - The claim: no single node sees the complete query. The client reassembles.
   - How does the API gateway split a query across nodes in practice?
   - Does the client software do the splitting, or does a gateway node do it?
   - If a gateway node does it, is that gateway a single point of failure
     and a single point of surveillance?
   - What does "client reassembles locally" mean for a browser-based SPA
     with no installed software?
   - Be honest about what v1 sharding actually achieves vs what Spiral PIR
     achieves — these are different claims and the honest version matters.

4. WARRANT CANARY ARCHITECTURE
   - Each node publishes a signed statement every N blocks:
     "I have not received a legal order compelling disclosure."
   - What does this signed statement contain, who signs it,
     and where is it published?
   - What happens to the canary if a node goes offline —
     does silence read as canary death?
   - What is the correct canary update cadence: per block, daily, weekly?
   - How does a user or Enterprise client verify the canary
     without creating a query log at the canary verification endpoint?

5. REDUNDANCY AND FAILOVER
   - If one node goes offline during a query, what happens to the user's result?
   - Does the sharding architecture degrade gracefully with N-1 nodes?
   - What does the node status page (already specced in legend-design-spec.md)
     need to show to be honest about node health?

CONSTRAINTS THAT DO NOT CHANGE:
- No server-side query logs. Ever. Not even for debugging.
- Ephemeral sessions are non-negotiable. Failover cannot create session persistence.
- Enterprise isolation (dedicated nodes for Enterprise clients) —
  decide in this session whether that is v1 or v2.
- Infrastructure must be honest about what v1 sharding actually achieves.
  Do not overclaim. The honest version is stronger than the marketing version.

OUTPUT THIS SESSION PRODUCES:
A document: legend-node-plan.md
Sections: node count and locations (locked), node specs (locked),
PIR sharding topology (honest description of v1 vs v2),
warrant canary design, redundancy model,
Enterprise isolation decision (v1 or v2).
Committed to refueler-legend alongside CLAUDE.md and SESSIONS.md.
Referenced by every build session that touches infrastructure.

Close session by updating SESSIONS.md with Legend-0 entry and carry-forward to Legend-1.
Mark this brief COMPLETE in legend-session-briefs.md.

"Nothing stops this train."

---

## Legend-1 — Infrastructure costs and scaling economics
**Status:** PENDING
**Produces:** `legend-node-plan.md` (economics section appended) + `legend-enterprise-pricing.md` (first draft)
**Prerequisite:** Legend-0 complete. `legend-node-plan.md` committed.

---

Session: Legend-1
Phase: 1 pre-build harness — infrastructure costs and scaling economics
Type: Planning and discussion only. No code. No terminal commands.
Output: legend-node-plan.md (economics section appended) +
legend-enterprise-pricing.md (first draft, infrastructure cost basis only)

Context files loaded: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md,
legend-design-spec.md, legend-scope.md, legend-node-plan.md,
legend-session-briefs.md

Legend-0 must be complete before this session opens.
legend-node-plan.md must exist and be committed.

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

2. SCALING COST MODEL
   - At 100,000 MAU: what does the node infrastructure cost?
   - At 1M queries/month: what does it cost?
   - At 10M queries/month (year 3–7 realistic ceiling): what does it cost?
   - At what query volume does the current architecture require additional nodes?
   - What is the cost-per-query at each scale point?
   - Silent Payments scanning is computationally heavier than address lookups —
     model its cost separately. At what adoption rate does it require
     dedicated scanning nodes?

3. BANDWIDTH AND CHAIN DATA
   - Bitcoin chain is ~600GB now, growing ~60GB/year.
   - If each node holds a full indexed copy, storage cost compounds.
   - If nodes shard the data (not just the queries), storage cost is shared
     but the architecture is more complex.
   - Which approach did Legend-0 lock? Price both if undecided.
   - Bandwidth cost for PIR sharding: the client downloads more than it
     strictly needs (by design). Quantify the bandwidth overhead vs a
     standard Esplora query.

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

6. INFRASTRUCTURE COST AS HONEST SIGNAL
   - The cost model should be published honestly — users should know
     what it costs to run Legend and why the Enterprise tier exists.
   - Does the "how the free tier works" section of refueler.io/legend
     need updating after this session?

OUTPUT THIS SESSION PRODUCES:
- legend-node-plan.md: economics section appended.
  Sections added: monthly cost at v1, scaling cost model, bandwidth overhead,
  break-even analysis, self-hosting cost estimate.
- legend-enterprise-pricing.md: first draft, infrastructure cost basis only.
  Full pricing model completed in Legend-2.

Close session by updating SESSIONS.md with Legend-1 entry and carry-forward to Legend-2.
Mark this brief COMPLETE in legend-session-briefs.md.

"Nothing stops this train."

---

## Legend-2 — Enterprise packaging and commercial model
**Status:** PENDING
**Produces:** `legend-enterprise-pricing.md` (completed)
**Prerequisite:** Legend-0 and Legend-1 complete. Both output documents committed.

---

Session: Legend-2
Phase: 1 pre-build harness — Enterprise packaging and commercial model
Type: Planning and discussion only. No code. No terminal commands.
Output: legend-enterprise-pricing.md (completed)

Context files loaded: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md,
legend-design-spec.md, legend-scope.md, legend-node-plan.md,
legend-enterprise-pricing.md (first draft from Legend-1),
legend-session-briefs.md

Legend-0 and Legend-1 must be complete before this session opens.

---

WHAT THIS SESSION DECIDES:

Infrastructure costs are modelled in legend-node-plan.md. This session
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
