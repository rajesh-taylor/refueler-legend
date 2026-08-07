# legend-economics.md — refueler-legend
> **Version:** 1.1 | **Created:** Legend-1 · 5 Aug 2026 | **Updated:** Legend-7B · 7 Aug 2026
> Infrastructure cost model for Legend — privacy-first Bitcoin block explorer.
> Load in every economics, Enterprise pricing, and business model session.
> Companion documents: `legend-node-plan.md`, `legend-design-spec.md`,
> `legend-scope.md`, `legend-enterprise-pricing.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Pricing basis and caveat

All figures are derived from live pricing confirmed by screenshot on 5 August 2026:
- Hetzner AX52: **€59/month** ex-VAT, unlimited traffic, €39 one-off setup fee.
- FlokiNET dedicated (Iceland): **€180–300/month** depending on hardware
  configuration and NVMe premium. 32 TB/month bandwidth cap on 1 Gbit port.
  NVMe upgrades and custom specs require a quote; matching the AX52 RAM and
  storage spec lands toward the upper end of the range.

FlokiNET does not run a consumer Ryzen product line. Their standard inventory is
dual-CPU Intel Xeon or AMD EPYC with ECC RAM. The spec premium is structural —
you are paying for the jurisdiction and the legal architecture it provides, not
commodity hardware. This is the correct trade.

**Prices are verified as of 5 August 2026. Re-check against live order pages
before each annual renewal cycle.**

---

## Section 1 — V1 launch costs: monthly total

**Five-node topology locked from Legend-7.** The original three-node estimate (€340–420/month)
is superseded. All references to €340–420, €358, or €360 as a total infrastructure cost in
other documents are stale and should be corrected on next edit.

### Per-node breakdown

| Node | Location | Provider | Hardware | Monthly (ex-VAT) |
|---|---|---|---|---|
| A | Falkenstein, DE | Hetzner | AX52 (8c Ryzen 7 7700, 64 GB DDR5, 2×1 TB NVMe) | €59 |
| B | **TBD** | **TBD (non-Hetzner — provider session required)** | Dedicated, AX52-equivalent spec | **TBD** |
| C | Reykjavik, IS | FlokiNET | Dedicated, matching RAM/NVMe (custom quote) | ~€240 (midpoint) |
| D | **TBD** | **TBD (fourth jurisdiction — provider session required)** | Dedicated, AX52-equivalent spec | **TBD** |
| E | **TBD** | **TBD (fifth jurisdiction — provider session required)** | Dedicated, chain-only (2 TB NVMe, lighter I/O profile acceptable) | **TBD** |
| **Total** | | | | **~€350–550/month (see range below)** |

**Nodes B, D, and E are TBD.** Provider selection session required before provisioning.
The range below is the planning estimate pending confirmed quotes.

**Five-node stated range: €350–550/month.** Derivation:
- Node A (Hetzner AX52): €59/month — confirmed.
- Node C (FlokiNET dedicated): ~€240/month — confirmed midpoint, custom quote required before provisioning.
- Node B (non-Hetzner dedicated, AX52-equivalent): €60–100/month estimated. Candidates include
  Frantech/BuyVM (Luxembourg), Servers.com (Baltics), OVHcloud dedicated (FR). Actual quote required.
- Node D (fourth jurisdiction, full participant): €60–120/month estimated. Same spec requirement.
- Node E (chain-only standby, lighter I/O profile): €50–80/month estimated. No Esplora index at
  launch reduces I/O and RAM pressure; spec can be slightly lower than A/B/D.
- **Lower bound (€350):** all TBD nodes at their minimum estimates.
- **Upper bound (€550):** TBD nodes at premium or jurisdiction-premium providers; FlokiNET at
  €300 upper end; setup fees amortised in year one.
- **Planning midpoint: ~€450/month.** Use this figure in all cost and break-even calculations
  until confirmed quotes replace the estimates.

One-off setup fees: Hetzner charges €39/node; other providers vary. Assume ~€150–200 total
across five nodes, amortised over year one (~€13–17/month). Year-one effective monthly: ~€465.

**Do not use the old three-node figures (€340–420/month, €358/month, €360/month) in any user-facing
or Enterprise-facing copy.** The correct stated range is €350–550/month. The planning midpoint is
~€450/month. These figures are estimates pending confirmed quotes for Nodes B, D, and E.

### VAT

Hetzner invoices ex-VAT. UK operator purchasing B2B from a German entity:
reverse charge applies; no German VAT paid, but the operator accounts for it
under UK VAT rules. FlokiNET and other providers may have different VAT treatments
depending on their registered jurisdiction and the operator's VAT status — verify
at invoice before provisioning each node. All figures in this document are ex-VAT
unless stated.

### FlokiNET bandwidth cap

FlokiNET baselines at 32 TB/month on 1 Gbit. At v1 traffic estimates (see §2),
monthly egress per node peaks at ~150–500 GB — comfortably inside 32 TB. This
cap becomes relevant only at sustained scale above ~150,000 MAU with high query
rates. Monitor per-node egress monthly; trigger a FlokiNET bandwidth review if
node C egress approaches 5 TB/month.

### Comparison to stale figures

- `legend-design-spec.md` states ~€96/month — this was a rough VPS estimate,
  before the dedicated-node topology was locked in Legend-0. It is wrong.
- The three-node estimates of €170–220/month (Legend-0 revision) and €340–420/month
  (Legend-1, Legend-economics.md v1.0) are superseded by the five-node topology
  locked in Legend-7. **Do not use any of these figures in user-facing or
  Enterprise-facing copy.**
- **Correct figure: ~€450/month planning midpoint, range €350–550/month pending
  confirmed quotes for Nodes B, D, and E.**

**Action required:** `legend-design-spec.md` funding model section states
"~€96/month (3–5 Hetzner nodes)." Correct at next session touching that file.
Replace with: "~€450/month (five dedicated nodes across five jurisdictions:
Hetzner AX52 (DE), provider TBD (jurisdiction TBD), FlokiNET (IS), plus two
further TBD nodes). Figures estimated August 2026; confirmed quotes pending."
This action was carried forward from Legend-1 and remains open.

---

## Section 2 — Scaling cost model

### Query payload with privacy architecture

A privacy-preserving query carries more data than a naive Esplora query by design:

| Component | Payload | Notes |
|---|---|---|
| Stage-1 fetch (index node) | 5–50 KB | Candidate row ID set; k-anonymity overhead |
| Stage-2 fetch (data node) | 10–100 KB | Full UTXO and transaction data for row IDs |
| Merkle proof | +1–2 KB | Locked Legend-0a |
| Cross-node header fetch | +80 bytes | Locked Legend-0a |
| **Total per verified query** | **~20–155 KB** | Call it 150 KB worst case |

A standard single-node Esplora query returns 5–60 KB. The privacy overhead is
**roughly 2–3× per query by design.** This is stated honestly in user-facing copy.

### Cost and compute at scale

| Scale | MAU | Queries/month | Egress/month (fleet) | Infra cost | Cost/query |
|---|---|---|---|---|---|
| V1 launch | 5,000–20,000 | 150k–600k | 23–90 GB | ~€450 | ~€0.00075–0.003 |
| Growth | 100,000 | ~3M | ~450 GB | ~€450 | ~€0.00015 |
| Scale | 1M queries/mo | 1M | ~150 GB | ~€450 | ~€0.00045 |
| High scale | 10M queries/mo | 10M | ~1.5 TB | ~€450–550 | ~€0.000045 |

Figures use ~€450/month planning midpoint. Will update when confirmed quotes for Nodes B, D, E are received.

Assumes ~30 queries/MAU/month at v1 (mixed browsing/monitoring pattern).

**At 10M queries/month, throughput is ~4 req/s average, ~40 req/s at peak.** Five
dedicated 8-core boxes will not notice. Egress at 10M is ~300 GB/node/month (fleet
spread across five nodes) — inside Hetzner's unmetered allowance and well inside
FlokiNET's 32 TB cap. Node E carries no query traffic (chain-only); fleet egress
is distributed across four query-active nodes (A, B, C, D).

**When does the architecture require a fourth node?** Load alone does not force
it. The fourth node arrives for Enterprise isolation (v2, contractual requirement)
long before traffic demands it. On load grounds alone, the current three-node
topology handles ~100M queries/month before degradation.

### Silent Payments: separate cost model

Silent Payments scanning is qualitatively different from address lookups. A naive
per-query full-range scan (ECDH per eligible input per scan key) is 1.5–3
core-hours over one year of blocks — not shippable.

**Required build decision: precomputed per-block tweak index.**
One public tweak computed per eligible transaction at block ingestion, shared
across all future scans. This is a build requirement, not a configuration choice.
Without it, Silent Payments scanning cannot ship.

With the tweak index:
- Per-scan compute: ~65 core-minutes single-threaded, ~8 minutes on 8 cores
- Storage overhead: ~30–60 GB per node (within 2 TB spec, alongside chain + address index)
- Concurrent SP scans sustainable per node: **5–10 full-range scans**

**Dedicated Silent Payments scanning nodes:** not required at v1 or v2 scale.
On a SegWit-shaped adoption curve, concurrent SP scans exceed three-node capacity
at year 2–3 volumes — a v2/v3 line item. Bounded-range scans (user specifies
start block) reduce cost dramatically for the common case and should be the
default UX. Full-range scan available but noted as compute-intensive.

**Queue for `legend-scope.md`:** add precomputed tweak index as a v1 build
requirement under Silent Payments. It is a prerequisite for that feature, not
an optimisation.

---

## Section 3 — Bandwidth and chain data

### Storage growth

| Item | Current | Growth/year | Per-node | Notes |
|---|---|---|---|---|
| Bitcoin chain | ~650 GB | ~60 GB | ~650 GB | Full copy per node — locked |
| Esplora/electrs index | ~400–600 GB | proportional | ~500 GB | Includes prefix/address index (non-strippable — locked Legend-0a) |
| SP tweak index | 30–60 GB | proportional | ~45 GB | Required at v1 |
| **Total per node at v1** | **~1.2–1.3 TB** | **~90–100 GB/year** | — | Within 2 TB spec |

**Headroom to 2029 on current spec: confirmed.** Storage upgrade cycle opens
at approximately year three to four depending on SP adoption. The 2 TB NVMe
spec was chosen correctly in Legend-0.

### Storage configuration (locked)

Separate drives (no RAID): chain data on one NVMe, index + tweak data on the
other. Chain data is fully re-syncable; there is nothing irreplaceable on these
nodes. Redundancy by RAID buys nothing that a re-sync doesn't. Confirmed correct.

### PIR role-split bandwidth overhead

The privacy architecture doubles egress relative to a naive explorer, specifically:
- **Stage-1 overhead:** client receives a candidate row ID set (5–50 KB) rather
  than a direct answer. The k-anonymity set is deliberately larger than needed —
  this is the mechanism, not a bug.
- **Stage-2 fetch:** a second HTTP request to a different node, same session.
- **Net overhead vs single-node Esplora: 2–3× per query.**

This figure belongs in user-facing copy as a concrete, honest claim:
*"A private query costs roughly twice the bandwidth of a surveilled one."*

---

## Section 4 — Enterprise client break-even

### Annual infrastructure cost

At the five-node planning midpoint of ~€450/month:
- **Annual infrastructure cost: ~€5,400/year (~£4,630/year at current rates)**

At the five-node upper bound of €550/month:
- **Annual infrastructure cost: ~€6,600/year (~£5,660/year at current rates)**

### Break-even contract values

| Contract value | Covers infra for (at €450/month midpoint) |
|---|---|
| ~£385/month | 12 months (cost-recovery floor, five-node midpoint) |
| £770/month | 24 months |
| £1,500/month (family office floor) | ~47 months (~3.9 years) |
| £3,500/month (family office ceiling) | ~110 months (~9.2 years) |

**The claim "one Enterprise client covers years of infrastructure" remains verified.**
At the five-node midpoint, one family office at the floor covers nearly four years.
At the ceiling, just over nine years. The economics claim holds with wider margins
than the three-node estimate; the additional nodes increase the annual cost by
~25% while materially improving the resilience and legal geography of the infrastructure.

### Implications for Legend-2

- **Minimum viable Enterprise contract on pure cost-recovery grounds: ~£385/month**
  (five-node midpoint; recalculate when confirmed quotes replace estimates).
  The three-node figure of £310/month is superseded. Anything below £385/month is
  a loss-leader at current estimates; do not commit to a contract price below this
  floor before confirmed quotes are in.
- **The merchant/franchise add-on floor (£200–500/month) is still below cost-recovery
  on its own** at five-node pricing. Acceptable as a secondary tier if family office
  revenue covers the primary cost — not as a standalone basis for the free tier.
- **Free tier economics:** five nodes at ~€450/month supporting unlimited free
  queries is not sustainable on infrastructure cost alone. It is sustainable
  because Enterprise contracts cross-subsidise it. The free tier is funded by
  Enterprise. This is the honest copy for the below-the-fold "how the free tier
  works" section. The additional two nodes strengthen this argument: the fifth node
  (Node E) exists solely to reduce restoration time — it is infrastructure for
  resilience, not for revenue, and the honest characterisation of the free tier
  is that it runs on infrastructure built to survive, not to scale cheaply.

---

## Section 5 — Self-hosting economics

### Raspberry Pi 5 route

| Item | Cost |
|---|---|
| Raspberry Pi 5 (8 GB) | ~£80 |
| 2 TB NVMe HAT | ~£60–90 |
| PSU, case, cooling | ~£20–40 |
| **One-off hardware total** | **~£160–210** |
| Electricity (~5–7W load) | ~£10–15/year |

Initial Bitcoin IBD on Pi hardware: 3–7 days (this is where BLAKE3 ARM
parallelism earns its keep — 4–8× faster internal hashing vs SHA-256 on ARM).
Ongoing: minimal maintenance, no monthly fee beyond electricity.

### Hetzner CAX21/CAX31 ARM route (cloud, ARM Ampere)

An alternative for self-hosters who don't want local hardware:
- Hetzner CAX21 (4 vCPU ARM, 8 GB RAM): ~€8/month — too small for the full index
- Hetzner CAX41 (16 vCPU ARM, 32 GB RAM): ~€28/month — viable for the BLAKE3
  ARM build with reduced index depth
- Not the same privacy posture as a dedicated box; noted for completeness

### Self-hosted mint distinction

Self-hosted instances run a **single-key rotating-key-set Cashu mint**, not the
FROST 2-of-3 multi-node ceremony used on the Refueler-operated nodes. One machine,
no threshold to distribute. The self-hoster's privacy comes from being the operator
— they trust themselves, so the multi-party trust architecture is unnecessary.
This distinction is stated plainly in the self-hosting guide.

### Self-hosting and the Enterprise offer

Self-hosting strengthens the Enterprise offer rather than undermining it.

The family office question: *"Why should we pay when we could run this ourselves?"*

Honest answer: self-hosting means buying a £200 computer, inheriting an initial
sync of 3–7 days, configuring Bitcoin Core, building the electrs fork from source
(pinned hashes, reproducible build), initialising and rotating a Cashu mint,
maintaining ufw/SSH hardening, and owning an update discipline. The Enterprise
contract is the institutional wrapper around exactly that labour — plus FROST
2-of-3 key management, warrant canaries, geographic jurisdiction distribution,
SLA, and a named contact who answers the compliance team's questions.

Publishing the self-hosting cost and complexity honestly *makes* this argument.
Hiding it would weaken it with any sophisticated buyer.

### AI-assisted self-hosting guide scope

Covers, in order:
1. Bitcoin Core installation and IBD — estimated time on target hardware
2. BLAKE3 electrs fork: reproducible build from pinned source, hash verification
3. Single-key Cashu mint: initialisation and monthly key rotation procedure
4. Firewall baseline: ufw rules (Bitcoin p2p, Esplora API port, SSH only)
5. SSH hardening: key auth, non-standard port, fail2ban
6. Sparrow Wallet: paste Legend endpoint URL into Server preferences
7. Honest single-node privacy statement: *"You are the operator. Your privacy
   comes from trusting yourself, not from multi-party architecture."*

---

## Section 6 — Infrastructure cost as honest signal

### What to publish

The cost model is not internal. It belongs on the page, in the /notes/ register:

Draft copy for below-the-fold "how the free tier works" section:
> *"Legend costs roughly €450 a month to run — five dedicated servers across five
> jurisdictions, chosen for their legal geography, not their price.
> One Enterprise contract covers the infrastructure cost many times over.
> Your free queries are not the product. They are the proof that the
> architecture works."*

Note: update the cost figure in this copy once confirmed quotes for Nodes B, D, E
replace the planning estimate. Do not use the old three-node figure (€360).

This is a draft. Final copy belongs to the UX language session (Legend-3).

### legend-design-spec.md correction

The funding model section contains: *"~€96/month (3–5 Hetzner nodes)"*.
This is wrong on both the figure and the provider. A carry-forward queued since
Legend-1 (for €360); now superseded by five-node topology. Correction queued for
the next session that edits `legend-design-spec.md`:

Replace with: *"~€450/month planning midpoint, range €350–550/month (five dedicated
nodes across five jurisdictions: Hetzner AX52 (DE), provider TBD (jurisdiction TBD),
FlokiNET dedicated (IS), plus two further TBD nodes). Figures estimated August 2026;
confirmed quotes pending for Nodes B, D, and E."*

---

## Carry-forward

- **Cost planning midpoint updated: ~€450/month, range €350–550/month.**
  Three-node estimates (€340–420, €360) are superseded. Do not use in any copy.
- **Minimum Enterprise contract on cost-recovery grounds: ~£385/month**
  (five-node midpoint). Recalculate when confirmed quotes replace estimates.
- **Family office range reaffirmed: £1,500–3,500/month** — covers 3.9–9.2 years
  of infrastructure per contract month at five-node midpoint. The "years of
  infrastructure" claim holds comfortably.
- **FlokiNET bandwidth cap (32 TB/month):** not a v1 concern at estimated query
  volumes. Fleet egress distributed across four active query nodes; Node E carries
  none. Monitor per-node egress monthly from launch.
- **Custom FlokiNET quote required before provisioning Node C** — confirm exact
  NVMe, bandwidth spec, and price before committing.
- **Provider selection session required before provisioning Nodes B, D, E.**
  TBD entries in the cost table must be resolved. Output: completed per-node
  cost table with no TBD entries, confirmed jurisdiction independence.
- **Queue for legend-scope.md:** SP tweak index as v1 build requirement;
  NUT-13/09 permanent-out; NUT-28 v2 note; NUT-24 v2+ note;
  plain-language script rendering v2; Merkle proof scope v1/v2/v3.
  Also: five-node topology references (applied in Legend-7B).
- **Queue for legend-design-spec.md:** €96 → €450 correction (supersedes the
  earlier €360 carry-forward from Legend-1; update applied in Legend-7B).

---

*"Nothing stops this train."*
