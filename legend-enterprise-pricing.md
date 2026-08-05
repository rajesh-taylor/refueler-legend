# legend-enterprise-pricing.md — refueler-legend
> **Version:** 0.1 (first draft — infrastructure cost basis only)
> **Created:** Legend-1 · 5 Aug 2026
> Full commercial model to be completed in Legend-2.
> Load in Legend-2 alongside legend-economics.md.
> Do not share externally — this draft contains internal cost basis notes.
> Companion documents: `legend-economics.md`, `legend-node-plan.md`,
> `legend-design-spec.md`, `legend-scope.md`.

---

## Status

This is the infrastructure cost basis stub produced in Legend-1.
Sections marked **[LEGEND-2]** are placeholders for the commercial structuring
session. No pricing commitments exist until Legend-2 closes this document.

---

## Infrastructure cost basis (confirmed Legend-1)

| Metric | Figure | Basis |
|---|---|---|
| Monthly infrastructure cost | €340–420/month | Live pricing, 5 Aug 2026 |
| Planning midpoint | ~€360/month | Used throughout this document |
| Annual infrastructure cost | ~€4,320/year (~£3,700) | At midpoint |
| Minimum contract for cost-recovery | ~£310/month | Infrastructure floor only |
| One family office client at floor | covers ~58 months of infra | £1,500/month ÷ £308/month |
| One family office client at ceiling | covers ~135 months of infra | £3,500/month ÷ £308/month |

**The "one Enterprise client covers years of infrastructure" claim is confirmed
and understated.** The free tier is funded by Enterprise cross-subsidy, not by
the free tier carrying its own weight.

---

## Tier outline (cost basis only — commercial detail in Legend-2)

The following tier structure emerged from the ad-hoc session (5 Aug 2026) and is
confirmed against the cost model. Pricing ranges are not final until Legend-2.

### Free tier

- **Price:** £0. No account. No payment. No rate limit at v1 launch.
- **Cost to operate:** ~€360/month shared across all users.
- **Funded by:** Enterprise contracts.
- **Data access:** identical to all paid tiers (locked — data parity across tiers).
- **Transport:** standard HTTPS. Client IP visible to contacted nodes.
- **Privacy statement:** *"Your query is private by architecture. Your IP is not
  hidden from the node unless you route through Tor yourself."*
- **No credentials required at v1.** Cashu credential infrastructure is present
  but the gate is not active. Re-evaluate if abuse requires throttling.

### Family office tier — [LEGEND-2]

- **Price range (to be confirmed):** £1,500–3,500/month
- **Infrastructure cost floor anchor:** £310/month
- **Cost basis verdict:** confirmed sustainable at any point in range.
  Floor covers 4.8 years of infrastructure. Ceiling covers 11 years.
- **Included (v1):** [LEGEND-2] — transport, SLA, contact, SP scanning, canary reporting
- **Included (v2):** [LEGEND-2] — Tor API, Double Ratchet, dedicated node isolation
- **Contract structure:** [LEGEND-2]
- **Onboarding:** [LEGEND-2]
- **SLA:** [LEGEND-2] — calibrate to actual three-node topology, not aspirational

### Merchant and franchise tier — [LEGEND-2]

- **Price range (to be confirmed):** £200–500/month or bundled with POS
- **Infrastructure cost basis note:** £200/month covers ~7.8 months of infra.
  Below cost-recovery on standalone basis. Sustainable only as a secondary tier
  cross-subsidised by family office contracts. Do not price as the primary revenue
  source.
- **Contract structure:** [LEGEND-2]

### Estate solicitor tier — per-report — [LEGEND-2]

- **Price range (to be confirmed):** £50–150 per verified estate report
- **Feature dependency:** v2/v3 (ZK balance proof, /legend/verify endpoint)
- **Infrastructure cost basis note:** per-report pricing is a v2/v3 question;
  cost basis modelling deferred to Legend-2 once v2 compute cost per ZK proof
  is estimated.
- **Payment model:** Lightning (pseudonymous). No account required.
  Document Lightning payment as pseudonymous, not anonymous.

### Enterprise API tier — [LEGEND-2]

- **Price range:** [LEGEND-2]
- **Includes at v2:** Tor API endpoint, Double Ratchet per-query ratchet,
  ML-KEM-768 hybrid transport, NUT-11 P2PK bound credentials, dedicated node (v2+)
- **Swan Bitcoin scenario:** [LEGEND-2] — internal only, not in published version

---

## What Legend-2 must resolve

In priority order:

1. **Family office tier** — final price, inclusions, SLA, contract structure,
   onboarding process, PI insurance requirements
2. **Merchant/franchise tier** — add-on vs bundled with POS, per-location vs flat rate
3. **Estate solicitor tier** — per-report pricing, payment flow, v2 feature timeline
4. **Swan scenario** — internal assessment only, not published
5. **Open source and the Enterprise sale** — how "read the code" is framed to a
   compliance officer
6. **Document structure** — transform this stub into a sales-usable reference

---

## Locked constraints from Legend-1 (non-negotiable in Legend-2)

- Minimum contract price: **£310/month on cost-recovery grounds alone.**
  Do not go below this regardless of competitive pressure.
- Data parity across tiers: **locked.** Tiers never differentiate on data access,
  only on transport and wrapper.
- No advertising, no data monetisation: **locked.** Revenue is contracts.
- FlokiNET custom quote required before node C is provisioned.
  This number may shift the cost floor by up to €60/month.
- "One Enterprise client covers years of infrastructure" is confirmed language
  for the sales conversation and for user-facing copy. Do not qualify it further
  than it warrants — it undersells the reality.

---

*Legend-2 completes this document.*
*"Nothing stops this train."*
