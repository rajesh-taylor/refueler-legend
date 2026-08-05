# legend-enterprise-pricing.md — refueler-legend
> **Version:** 1.0 (completed) | **Created:** Legend-1 · 5 Aug 2026 | **Completed:** Legend-2 · 5 Aug 2026
> Commercial model and sales reference for Legend paid tiers.
> Load in every sales, legal, onboarding, and commercial session.
> The Swan scenario section is INTERNAL ONLY — remove before any external share.
> Companion documents: `legend-economics.md`, `legend-node-plan.md`,
> `legend-design-spec.md`, `legend-scope.md`.

---

## How to read this document

This is a sales reference written to compliance-officer standard. It states what
each tier cannot do as clearly as what it can. Nothing in this document promises
a capability that does not exist at the stated version, and no tier ever
differentiates on data access — data parity across tiers is locked and
non-negotiable. Tiers differ on transport, isolation, support, and documentation.
Never on data.

All prices ex-VAT. Infrastructure cost basis: `legend-economics.md`, verified
against live pricing 5 August 2026.

---

## Tier summary table

| Tier | Price | Availability | What it buys |
|---|---|---|---|
| Free | £0 | v1 launch | The explorer. Full data. No account, no payment, no rate limit. |
| Family office | £1,500/month (v1) · £2,500/month (v2) · £3,500/month (v2+) | v1, invite-only, max 5 clients | Institutional wrapper: SLA, named contact, compliance pack, canary alerting, SP scanning support |
| Merchant / franchise | £250/month entity · £500/month franchise (≤25 locations) | v1, Refueler POS merchants only | Support relationship and accountant onboarding around the free product |
| Estate report | £50 (statement) · £150 (full report) | v2 / v3 | Per-report verified estate output for UK solicitors |
| Enterprise API | From £2,500/month | v2 | Tor API, Double Ratchet, ML-KEM-768 hybrid, NUT-11/P2BK credentials |

Minimum contract on cost-recovery grounds: **£310/month**. No tier is priced
below it except the merchant tier, which exists only under family office
cross-subsidy and is never the primary revenue plan.

---

## Free tier

- **Price:** £0. No account. No payment. No rate limit at v1 launch.
- **Data access:** identical to every paid tier. Locked.
- **Transport:** standard HTTPS. Client IP visible to contacted nodes.
- **Honest scope:** *"Your query is private by architecture. Your IP is not
  hidden from the node unless you route through Tor yourself."*
- **Funded by:** Enterprise contracts. Three nodes cost ~€360/month; one family
  office contract covers that many times over. The free tier is the proof that
  the architecture works, not the product.
- Cashu credential infrastructure is present but ungated at v1. Re-evaluate
  only if abuse forces throttling.

---

## Family office tier

### Pricing ladder

| Price | Version | Adds |
|---|---|---|
| **£1,500/month** | v1 | Everything in the v1 service scope below |
| **£2,500/month** | v2 | Tor-native API endpoint, Double Ratchet per-query transport, ML-KEM-768 hybrid key exchange |
| **£3,500/month** | v2+ | Dedicated node isolation (fourth node, contractually reserved) |

The ladder is capability-defined, not negotiation-defined. Each step exists
because a specific transport or isolation capability ships at that version.
**At v1, only the £1,500 tier is sold.** The upper bands are quoted in the
contract as scheduled upgrades with their version dependencies stated — a
client is never charged for a capability before it is live.

Comparator note: Mempool Enterprise (~$500–2,000/month) sells query capacity on
shared logging infrastructure. Legend sells jurisdiction-distributed
infrastructure, FROST 2-of-3 key management, per-node warrant canaries, and a
no-logs architecture that is structural rather than policy. These are different
products; the price difference is the wrapper, and the wrapper is the point.

### Included at v1 (£1,500/month)

- Unlimited queries on the shared three-node topology (Falkenstein, Helsinki,
  Reykjavik — two providers, two legal jurisdictions)
- Silent Payments (BIP-352) scanning, bounded-range default, full-range available
- Warrant canary monitoring with automated expiry alerting (see SLA)
- Named contact: Rajesh Taylor, founder and operator. There is no account team.
  The person who built the architecture answers the compliance team's questions.
- Compliance documentation pack (see below)
- Quarterly architecture review call
- Batch query support for portfolio monitoring (NUT-29 batched credentials)

### Explicitly not included at v1

- Tor-native API (v2 — free-tier users may route via Tor Browser themselves)
- Double Ratchet / post-quantum transport (v2)
- Dedicated node isolation (v2+)
- Compliance report generation (v3)
- Any data access difference from the free tier. There is none. Locked.

### Invite-only at v1 — capped at five clients

One operator serves five Enterprise relationships honestly. More is a promise
the topology and the founder's hours cannot keep. The cap is stated to
prospects plainly: it is a truthful constraint, not manufactured scarcity.

**Onboarding process:**
1. Introduction — warm referral (Refueler merchant network, professional
   network, or inbound from published articles). No cold outbound at v1.
2. Architecture call — founder walks the compliance team through the node plan,
   the canary architecture, and the honest-scope statements. The UK operator
   caveat is raised by us, unprompted, in this call.
3. Compliance pack delivered for internal review.
4. Contract execution, first invoice, endpoint credentials issued (NUT-11
   P2PK-bound to the client's key).
5. Quarterly review cadence begins.

### Contract structure

- **Three-month minimum term, then monthly rolling. 60 days' notice either side.**
- Annual prepay available at eleven months' price.
- 30-day payment terms, invoiced in GBP.
- No auto-renewal cliffs, no multi-year lock-in. A solo-operator vendor asking
  for a three-year commitment is asking the client to underwrite key-person
  risk; the short structure is the honest one and signals confidence.

### Compliance and PI insurance pack

Delivered at onboarding, versioned, re-issued on material change:

1. **Architecture summary** — node topology, jurisdictions, providers, the
   role-split query design in precise language (collusion-resistant query
   splitting with blind credential unlinkability — not "PIR", not
   "zero-knowledge" — the v1 claim, exactly as achieved)
2. **Honest-scope statement** — what Legend structurally cannot see, what it
   can (IP over HTTPS), and what an attacker obtains from a compromised node
3. **UK operator caveat, in full** — IPA 2016 exposure, what per-node
   geographic distribution protects against (hosting-provider orders) and what
   it does not (operator-level compulsion). Verbatim from `legend-node-plan.md`
   Section 6.
4. **Canary verification procedure** — three independent channels (node-direct,
   GitHub mirror, Nostr), verifiable without contacting Legend infrastructure
5. **Incident response commitment** — matching the SLA below
6. **Open source verification path** — repo, MIT licence, reproducible build
   procedure, pinned dependency hashes
7. **Audit roadmap** — independent cryptographic audit scheduled Phase 5;
   until published, no "audited" claim appears anywhere, including here

### SLA — calibrated to the actual topology

This SLA is written for a three-node topology operated by one person in London.
It promises what that infrastructure delivers, not what a marketing document
would prefer.

| Commitment | Value |
|---|---|
| Monthly availability target | **99.0%** (service available = ≥2 nodes operational, role-split intact) |
| Full-splitting mode (all 3 nodes) | ≥95% of each month |
| Support response | Within 4 UK business hours, Mon–Fri 09:00–18:00 London |
| Canary expiry notification | Automated, within 12 hours of an expired state |
| Incident notification | Status page + direct email within 24 hours |
| Post-incident summary | Written, within 5 business days |
| Service credit | 10% of monthly fee for any calendar month below 97% availability |

**Not promised:** 24/7 on-call, 99.9%+, sub-hour response. One human operates
this service. Degraded modes (N-1 reduced splitting, N-2 single node) are
displayed honestly in the client's own tooling and on the status page — the
privacy claim is never silently degraded, contractually as well as
architecturally.

---

## Merchant and franchise tier

### Structure

**Always an add-on. Never bundled into the Refueler POS contract.** Bundling
creates a cross-product dependency and pricing seam of exactly the kind the
scope discipline exists to prevent. Legend and POS remain separately priced,
separately cancellable products that happen to share a customer.

- **Single business entity: £250/month**
- **Franchise: £500/month flat, covering up to 25 locations.** Above 25:
  custom quote. The rate is flat because the user is the franchise's
  accountant, not the till — one professional checking addresses is one
  workload regardless of location count.
- Sold at v1 only into the existing Refueler POS merchant base. Warm pipeline,
  no cold sales motion.
- Contract: monthly rolling from day one, 30 days' notice. Lower stakes than
  family office; lower ceremony.

### What the merchant's accountant is actually buying

The features an accountant needs — address verification, payment confirmation,
denomination toggle (sats/BTC/GBP at time of transaction) — are all present in
the free tier, because data parity is locked. The merchant tier buys:

- Onboarding session for the accountant (how to verify receipts privately,
  why the query log they are *not* creating matters — the Article 21 argument
  applied to a café in Limehouse)
- Named support contact and response commitment (next business day)
- An invoice their books can process — a real requirement for a business
  expense, stated without embarrassment
- Priority in the support queue behind family office clients

### Honest cost note (internal pricing discipline)

£250/month covers ~10 months of fleet infrastructure; £200 covers ~7.8. Both
sit below the £310 cost-recovery floor in isolation. This tier is sustainable
only while family office contracts carry the primary cost. It is a pipeline
and ecosystem tier — the merchant's accountant and solicitor are the warm
introduction to the professional tiers — and is never to be priced or
projected as the primary revenue source.

---

## Estate solicitor tier — per-report

### Pricing

| Report | Price | Contains | Version dependency |
|---|---|---|---|
| Block-height balance statement | **£50** | Holdings verified at a stated block height, Merkle inclusion proof export, plain-language summary | v2 (proof export artefact) |
| Full verified estate report | **£150** | The above, plus ZK balance proof, multi-sig quorum verification in plain language, methodology statement | v3 (ZK proofs, /legend/verify) |

**Designed now, sold at v2/v3.** No estate report is offered before the
underlying feature ships. This section exists so the pricing model, payment
flow, and insurer requirements are settled before the build, not improvised
after it.

### Payment — two routes, stated honestly

1. **Lightning, at point of purchase.** No account, no email, no identity
   created. Invoice presented in the /legend/verify flow; report generated
   client-side on payment. Lightning is **pseudonymous, not anonymous** —
   channel counterparties and payment correlation exist. Stated in the flow.
2. **Invoiced (BACS), for firms.** UK law firms pay against invoices with
   purchase orders. An invoice is an identified commercial relationship —
   this route trades the anonymity of route 1 for the procurement reality of
   a regulated firm. The trade is stated, not hidden. Firms choosing this
   route get a standing account for billing purposes only; report content
   remains client-side generated and unseen by the operator either way.

### What the PI insurer pack contains

1. **Methodology statement** — what was verified, against what (the Bitcoin
   chain at a stated block height), by what procedure
2. **Proof and limitation statement** — what the report proves (control of at
   least X BTC at block N, quorum construction of a stated multi-sig) and what
   it does not (identity of the controller, absence of other holdings,
   anything about keys Legend never touches)
3. **Independent verification procedure** — steps by which any third party can
   re-verify the report against the public chain without Legend's involvement
4. **Legal acceptance caveat** — admissibility depends on legal frameworks
   outside Legend's control. Legend provides the technical verification; it
   does not warrant court acceptance. The SRA has mandated crypto asset
   accounting in estates without providing tooling; Legend is tooling, not
   legal advice.

---

## Enterprise API tier

Formalised at v2. From **£2,500/month**, aligned with the family office v2
band, quoted individually by integration scope.

Includes at v2: Tor hidden service endpoint, Double Ratchet per-query
transport, ML-KEM-768 + X25519 hybrid key exchange, NUT-11 P2PK-bound
credentials (NUT-28 P2BK hardening at v2), batch credential issuance for
monitoring workloads, the family office SLA and compliance pack.

Target buyers: wallet apps wanting a private Esplora endpoint, Bitcoin-backed
lenders (post-v2 audit), compliance platforms. The Sparrow-compatible endpoint
means integration cost for a wallet is near zero — the contract sells the
transport hardening, the SLA, and the credential architecture.

---

## Self-hosting and its relationship to paid tiers

Legend is MIT-licensed and self-hosting is documented, encouraged, and costed
honestly: ~£160–210 one-off on a Raspberry Pi 5, ~£10–15/year electricity,
3–7 days initial sync (`legend-economics.md` §5).

**The sales framing of "don't trust us, read the code":** it is a due-diligence
enabler. Every privacy claim in the contract can be verified against published
source and reproducible builds by any auditor the client appoints. No closed
competitor can offer that verification path. To a compliance officer, open
source is not a discount signal — it is the only mechanism by which the
architecture claims can be independently confirmed.

**The honest answer to "why pay when we could run this ourselves?":**
Self-hosting means owning Bitcoin Core configuration, a from-source
reproducible build, mint initialisation and rotation, firewall and SSH
hardening, and a permanent update discipline — with single-key custody and no
multi-party architecture, because a self-hoster trusts themselves. The
Enterprise contract is the institutional wrapper around exactly that labour,
plus the things a self-hosted instance structurally cannot have: FROST 2-of-3
key management across two jurisdictions, three independent warrant canaries,
an SLA, a compliance pack their PI insurer can file, and a named contact. A
family office trustee does not self-host their Bloomberg terminal. Publishing
the self-hosting route honestly *makes* this argument; hiding it would weaken
it with precisely the sophisticated buyer Legend wants.

---

## INTERNAL ONLY — the Swan Bitcoin scenario

*Remove this section from any externally shared version.*

**Premise:** a well-resourced Bitcoin company (Swan as the archetype) forks
Legend rather than contracting, if the code is ahead of the relationship.

**What MIT permits:** everything. Full fork, closed-source derivative,
commercial resale, no contribution back. The only obligation is preservation
of the copyright and licence notice. Plan on this basis; anything else is
wishful reading of the licence.

**What cannot be forked:**
- **The canary track record.** Credibility of a warrant canary is a function
  of unbroken time. A forked deployment starts at day zero. Twelve months of
  Legend canary history is twelve months no fork can acquire at any price.
- **The audit report.** An independent cryptographic audit attaches to a
  specific deployment and operational practice, not to the source in the
  abstract. Their fork would need its own audit — real money and real time.
- **The operational discipline.** Monthly FROST DKG ceremonies, rotation
  runbooks, tweak-index maintenance, cross-jurisdiction ops. Encoded in
  practice, not in the repo.
- **The relationship.** SLA, named contact, compliance pack — the things the
  Enterprise buyer is actually purchasing.

**When the fork calculation turns unfavourable:** audit published + /legend/verify
live + ≥12 months unbroken canary history + Enterprise credential architecture
(NUT-11/P2BK) in production. Before that point the code genuinely is most of
the value and a fork is rational for them — which is why the approach happens
after, not before.

**Approach timing, therefore:** post-audit, post-/legend/verify, post twelve
months of canary record. Not earlier. An early approach advertises the code
before the moat exists.

**Contribution-back norm:** establish it publicly now, before any fork is
contemplated. `CONTRIBUTING.md` states the expectation that improvements to
the privacy layer flow upstream, and the project publicly credits upstream
lineage (electrs, esplora) as the norm it practises itself. No legal force
under MIT — the mechanism is reputational pricing: a silent fork by a
prominent company becomes a visible norm violation, decided against a norm
that predates the fork. Cheap to establish, only effective if established
early.

---

## Honest scope statements — per tier, one paragraph each

**Free:** query privacy by architecture; IP visible over HTTPS unless you use
Tor; no logs exist, structurally; operated by one person in the UK, subject to
UK law (caveat published in full).

**Family office:** everything in Free, plus the institutional wrapper. The
wrapper is support, documentation, alerting, and accountability — not
different data, not (at v1) different transport. The v1 claim is
collusion-resistant query splitting, not cryptographic PIR; that upgrade is
v2 and is not sold before it ships.

**Merchant:** the free product plus a support relationship and an invoice.
Stated exactly that plainly to the buyer.

**Estate reports:** technical verification against the public chain.
Not legal advice, not a warranty of admissibility.

**Enterprise API:** hardened transport and credential architecture at v2.
Data identical to the free tier — the parity lock is the product's spine, and
it appears in every contract.

---

## Locked in Legend-2

- Family office pricing ladder: £1,500 (v1) / £2,500 (v2) / £3,500 (v2+).
  Capability-defined steps. Only the v1 band is sold at v1.
- Family office cap: five clients at v1. Invite-only.
- Contract: 3-month minimum, monthly rolling, 60 days' notice. Annual prepay
  at eleven months' price.
- SLA: 99.0% monthly / ≥95% full-splitting / 4 business-hour response /
  12-hour canary alerting / 10% credit below 97%. No 24/7 claims.
- Merchant: £250 entity, £500 franchise flat (≤25 locations), always add-on,
  never bundled with POS, sold only to existing POS merchants at v1.
- Estate reports: £50 statement (v2) / £150 full report (v3). Lightning
  (pseudonymous, stated) or invoiced BACS (identified, stated).
- Swan approach timing: post-audit + post-/legend/verify + ≥12 months canary
  history. CONTRIBUTING.md contribution-back norm to be published at repo
  build-out (queue for first build session).
- All Legend-1 constraints reaffirmed: £310 floor, data parity, no
  advertising or data monetisation, FlokiNET quote before node C.

---

*Completed Legend-2 · 5 Aug 2026.*
*"Nothing stops this train."*
