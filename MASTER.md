# MASTER.md — refueler-legend
> **Version:** 1.0 | **Created:** Legend-5 · 6 Aug 2026
> Compression of the six planning documents. Rebuilt fresh at the end of each
> planning block — not maintained incrementally. Source of truth is always the
> detail file named in each section.

---

## 1 — What MASTER.md is

MASTER.md compresses six planning documents: `legend-node-plan.md`, `legend-economics.md`,
`legend-enterprise-pricing.md`, `legend-design-spec.md`, `legend-ux-language.md`, `legend-scope.md`.
It does not replace `CLAUDE.md` or `SESSIONS.md`, which load separately in every session,
and it does not duplicate what CLAUDE.md already carries (repo identity, BLAKE3 locked
decisions, Legend architectural decisions, query credit model, locked phrases, copy
register permanent rules, build sequence, key articles).
Rebuild cadence: fresh at the close of each planning block. Detail files are authoritative
on any conflict.

---

## 2 — Node topology
*Source and authority: `legend-node-plan.md`*

### Three nodes. Locked.

| Node | Provider | Datacentre | Jurisdiction |
|---|---|---|---|
| A | Hetzner | Falkenstein | Germany (EU) |
| B | Hetzner | Helsinki | Finland (EU) |
| C | FlokiNET | Reykjavik | Iceland (EEA, non-EU) |

Two Hetzner nodes are acceptable because node C sits outside the EU legal-assistance
fast lane — a different MLAT pathway. Three Hetzner nodes would be one company: a single
order on Hetzner GmbH reaches all of them. Identical spec per node: 8+ core x86_64,
64 GB RAM, 2 TB NVMe (two drives, no RAID — chain on one, index + tweak data on the
other), Ubuntu 24.04 LTS, dedicated not VPS. Each runs: Bitcoin Knots (BIP110, RBF
disabled), BLAKE3 electrs fork in Esplora mode, Legend query API, FROST mint
participant, canary publication, public health endpoint. Full chain index per node —
roles (index/data) are logical per-query assignments by the browser, never physical
partitions. No gateway, ever. Signed node manifest at `refueler.io/legend/manifest.json`
(offline release key); adding node four is a manifest update, not a code change.
Enterprise dedicated node: v2, contract language designed in Legend-2.

### FROST 2-of-3

One key share per node; any 2-of-3 required to issue credentials or sign canaries.
Full key never exists on any machine. Monthly DKG ceremony creates each rotating mint
instance; all shares destroyed at window close (forward secrecy at mint level).
Shares held in memory during the window, never on disk unencrypted; anything at rest
is Argon2id-encrypted. A seized node yields one share — cryptographically useless alone.
Implementation: `frost-secp256k1` (ZF FROST project). Node offline at DKG: 24-hour
ceremony window, previous window extended 24h maximum, status page shows rotation state.

### Canary architecture

One canary per node, independently meaningful. Signed statement embeds recent block
height + hash (chain as freshness oracle), FROST 2-of-3 signature. Published daily,
72-hour expiry — a 48-hour silence window before death. **Silence reads as death, by
design.** No grace periods; a node down for maintenance kills its canary; resurrection
requires a fresh signed statement. Three independent publication channels:
1. Static `canary.json` on the node itself
2. GitHub mirror (`canaries/[node-id]/latest.json`) — immutable commit history
3. Nostr note from per-node npub — verifiable via any relay, Legend never contacted

Verification creates no log anywhere. Enterprise clients receive automated expiry
alerts (SLA: within 12 hours).

### UK operator caveat

All three nodes are operated by one person domiciled in London. The Investigatory
Powers Act 2016 permits compelled disclosure, non-disclosure orders, and — untested
in UK courts — arguably compelled continuation of canary publication. Geographic
distribution across Germany, Finland, and Iceland protects against orders served on
the *hosting providers* in those jurisdictions; it does not protect against a UK
order served on the operator personally, which reaches all three nodes regardless
of hardware location. Stated unprompted in the privacy explainer modal, the
below-the-fold explainer, and Enterprise contract materials. A product that
overstates its legal protections is weaker than one that understates them.

---

## 3 — Economics
*Source and authority: `legend-economics.md` (full figures, scaling tables, self-hosting costs)*

- **V1 infrastructure cost: €340–420/month. Planning midpoint: €360/month** (ex-VAT).
  Two Hetzner AX52 (€59 each) + FlokiNET dedicated (~€240 midpoint, custom quote
  mandatory before provisioning). Verified against live pricing 5 Aug 2026;
  re-check before each annual renewal. Never use the stale €96 figure anywhere.
- **Storage headroom:** ~1.2–1.3 TB per node at v1 (chain ~650 GB, index ~500 GB,
  SP tweak index ~45 GB), growing ~90–100 GB/year. 2 TB spec confirmed to ~2029.
- **Break-even line: ~£310/month.** Minimum viable contract on cost-recovery grounds.
  No tier priced below it except merchant (sustainable only under family office
  cross-subsidy). One family office at £1,500/month covers ~4.8 years of
  infrastructure; at £3,500, ~11 years. Free tier is funded by Enterprise —
  this is the honest public copy, not an internal note.
- Privacy overhead is 2–3× bandwidth per query vs a naive explorer, by design.
  Stated honestly in user-facing copy.
- SP scanning requires the precomputed per-block tweak index (see §7) — without it,
  a full-range scan is 1.5–3 core-hours and unshippable; with it, ~8 minutes on
  8 cores, 5–10 concurrent scans per node.

---

## 4 — Enterprise commercial model
*Source and authority: `legend-enterprise-pricing.md` (full terms, compliance pack, Swan section — INTERNAL ONLY)*

**Data parity across all tiers is locked and non-negotiable.** Tiers differ on
transport, isolation, support, documentation. Never on data.

### Pricing ladder — capability-defined, not negotiation-defined

| Tier | Price | Version |
|---|---|---|
| Free | £0 — no account, no rate limit | v1 |
| Family office | £1,500/month | v1 (only band sold at v1) |
| Family office | £2,500/month — Tor API, Double Ratchet, ML-KEM-768 hybrid | v2 |
| Family office | £3,500/month — dedicated node isolation | v2+ |
| Merchant | £250/month entity · £500/month franchise (≤25 locations) | v1, POS merchants only, always add-on, never bundled |
| Estate reports | £50 statement (v2) · £150 full report (v3) | per-report |
| Enterprise API | from £2,500/month | v2 |

### Terms

- **Five-client cap at v1. Invite-only.** One operator serves five relationships
  honestly; the cap is stated to prospects as a truthful constraint.
- Contract: 3-month minimum, then monthly rolling, 60 days' notice either side.
  Annual prepay at eleven months' price. 30-day terms, invoiced GBP.
  No multi-year lock-in — short structure is the honest one for a solo operator.
- A client is never charged for a capability before it ships.

### SLA headline figures

99.0% monthly availability (≥2 nodes, role-split intact) · ≥95% of month in
full-splitting mode · 4 UK business-hour support response (Mon–Fri 09:00–18:00) ·
canary expiry alert within 12 hours · incident notice within 24 hours ·
post-incident summary within 5 business days · 10% credit below 97%.
No 24/7 claims. Degraded modes never silently hidden — contractually as well
as architecturally.

### Swan timing (INTERNAL)

Approach post-audit + post-`/legend/verify` + ≥12 months unbroken canary history +
NUT-11/P2BK in production. Not earlier. Establish the CONTRIBUTING.md
contribution-back norm publicly now, before any fork is contemplated.

---

## 5 — Design system summary
*Source and authority: `legend-design-spec.md` (full spec). Canonical token values: `REFUELER-BRIDGE.md`.*

### Page architecture

Eleventy static shell (nav, footer, SEO, above-fold, below-fold acquisition content)
+ vanilla JS SPA mounted into a container div (all query logic, results, modals,
credentials). No page navigation on query; URL never changes; no visible seam.
`refueler.io/legend` — never a separate domain.

### Above the fold

Query input is the first element. No hero, no product description above the search
bar — the page opens the way a search engine opens. Batch icon on input focus only.
Below the fold: three static sections (what Legend does differently / who it's for /
how the free tier works).

### Design tokens — full list, both themes (canonical per BRIDGE CC-74 hex lock)

**Backgrounds:** Paper `--bg: #F5F0E8` · `--surface: #EDEAE4` · `--surface-raised: #E4E1DA`
— Carbon `--bg: #1A1A1A` · `--surface: #26282C` · `--surface-raised: #2E3035`
**Text:** Paper `--text-primary: #3D3A36` · `--text-secondary: #5A5751` · `--text-tertiary: #9A948D`
— Carbon `--text-primary: #E4E2DC` · `--text-secondary: #8A8680` · `--text-tertiary: #5A5751`
**Borders:** Paper `--border: #D6D1C8` · `--border-mid: #B8B2A8` · `--inset-rule: var(--border)`
— Carbon `--border: #35373B` · `--border-mid: #4A4D52` · `--inset-rule: #C8A96E`
**Accent (chrome only, never CTA):** `--accent: #C8A96E` · `--accent-hover: #E0C48A`
**CTA (consumer surfaces):** Paper `--accent-action: #D4690A` · Carbon `--accent-action: #F5820A`
**Canary expired (status page only, added Legend-4):** Paper `--canary-expired: #B8860B`
· Carbon `--canary-expired: #C9A227`
**Typography:** `--heading` Satoshi 700 · `--sans` DM Sans 300/400/500 ·
`--mono` IBM Plex Mono 400/500 (addresses, TXIDs, values, table cells)
**Structural:** border `0.5px` · card radius `10px` · button `8px` · modal `12px` ·
theme transition `0.35s` all token properties · theme detection `dataset.theme === 'carbon'`
only · `rs-theme` cookie scoped `.refueler.io`.
Note: the token block inside `legend-design-spec.md` predates the CC-74 hex lock on
`--bg` — the values above are canonical.

### Status page — `refueler.io/legend/status`

Static Eleventy page, footer-linked only (not in nav, not linked from banners).
Three sections top to bottom: privacy mode, node status, canary status.

**Privacy mode — four states, exact strings:**

| State | String | Colour |
|---|---|---|
| Full splitting | `Full splitting active — three nodes operational.` | `--text-primary` |
| Reduced splitting | `Reduced splitting — one node offline.` | `--text-primary` |
| Single node | `Single node — query splitting unavailable.` | `--text-secondary` |
| Rotation | `Credential rotation in progress — queries available, new credentials paused.` | `--text-tertiary` |

**Canary display rules:** canary section separated from node status by a full-width
rule — operational status and legal-compulsion status are different claims and must
not share visual language. Per-node typographic block (Mono small caps label,
signed-at block height, expiry countdown). Live canary: no colour — the expected
state needs no emphasis. Expired: `EXPIRED` appended after a second middot in muted
amber (`--canary-expired`); body lines shift to last-signed/expired-ago; no icon,
no red, no animation, no modal. Amber signals attention, not breach. One-line FROST
integrity note beneath the blocks in `--text-tertiary`.

**What the status page does not show (spec, not commentary):** query volumes,
traffic graphs, latency metrics, error logs, CPU/memory/storage utilisation,
historical uptime, canary history beyond current state. It knows three things —
reachability, chain height, canary state — shows them, and stops.

---

## 6 — Copy register summary
*Source and authority: `legend-ux-language.md`. All locked strings: Section 8 of that file. Do not reproduce them from here.*

**Register rule:** legal-document precision at reader-first readability — written so
a breach-notified holder with thirty seconds, a solicitor quoting it in a client
letter, and a brand-new Bitcoiner can all read the same string.

**Never-list:** anonymous · military/bank/enterprise-grade · Swiss-grade (or any
jurisdiction as quality signal) · end-to-end unless technically exact ·
zero-knowledge as v1 headline · PIR without qualification · exclamation marks ·
"secure" for a degraded mode (it is reduced, not broken) · "no logs" without the
structural qualifier · the Chainalysis/owner phrase in UI copy (articles and
presentations only) · implying Tor alone makes a free-tier query fully private.

**Denomination toggle labels (locked CLAUDE.md v1.5):**
`sats` · `BTC` · `USD at time of transaction` · `GBP at time of transaction`
Long-form fiat labels are deliberate: historical fiat ≠ current fiat, and the
distinction is accountant-legible. Default: sats. Session-persisted, no localStorage.
(GBP edit not yet applied inside `legend-ux-language.md` §4/§8 — see §8 below.)

Every other finalised string — landing hierarchy, query flow states, result anatomy,
all five modals, honest-scope statements, degraded mode banners — lives in
`legend-ux-language.md` Section 8 (locked copy index). Build sessions pull from that
table. If a string is not in it, it is not finalised.

---

## 7 — Scope by version
*Source and authority: `legend-scope.md`. Nothing builds without a version reference there.*

### Feature table

| Area | v1 | v2 | v3 |
|---|---|---|---|
| Bitcoin mainchain | Full | + provenance | + federated |
| Lightning | On-chain only | Own-node channel correlation | — |
| Liquid | — | — | Full |
| Silent Payments | Full scan (tweak index prerequisite) | — | BIP-352 contribution |
| CoinJoin | Detection flag | Extended recognition | — |
| PIR | Role-split approximation | Spiral PIR | — |
| ZK proofs | — | Balance proofs | Credential architecture (CL) |
| Cashu credentials | Infrastructure, ungated | — | Federated mints (evaluate) |
| Batch / breach response | Yes (≤50 addresses) | Yes | Yes |
| UTXO advisor | Qualitative flag | Quantitative score | — |
| Merkle proof | Cross-node SPV | Proof export artefact | Estate PI methodology |
| Script rendering | Type label only | Plain-language quorum | — |
| Compliance reporting | — | — | Yes |
| Estate tooling / `/legend/verify` | — | ZK proof stub | Full verification flow |
| Stablesats / DLC recognition | — | Yes | — |
| Cashu mint health check | — | Reserve check, blind | — |

Permanent out of scope (any version): non-Bitcoin chains, third-party surveillance
analytics, key handling/signing, server-side query storage, external
video/analytics, advertising or data monetisation, free-tier accounts,
"anonymous" claims for Lightning.

### Cashu NUT decisions

**In use (v1, locked):**
- NUT-00 — blind signatures, the core credential primitive
- NUT-01/02 — mint info / keysets, required vocabulary for mint operation
- NUT-06 — mint info endpoint; session start confirms mint window
- NUT-07 — credential validity check without account state
- NUT-11 — P2PK-bound Enterprise credentials; blocks theft and replay
- NUT-12 — DLEQ proofs, browser-side mandatory; proves honest mint signing
- NUT-19 — idempotency; no duplicate issuance on retry (batch reliability)
- NUT-29 — batched minting; 47 addresses → 47 credentials in one round-trip

**Permanently rejected:**
- NUT-27 (memos) — human-readable context on a credential destroys unlinkability
- NUT-09 (restore) — requires mint-held issuance state; breaks rotating-mint forward secrecy
- NUT-13 (deterministic secrets) — lets a compromised device reconstruct credential history

**Rejected v1 / deferred:** NUT-17 (WebSockets — persistent connection incompatible
with ephemeral sessions) · NUT-21/22 (monetary melt — mint is non-monetary,
structurally absent) · NUT-28 (P2BK hardening — v2) · NUT-24 (HTTP 402 — v2+,
only if abuse forces throttling).

### Merkle proof scope

| Scope | Version |
|---|---|
| Cross-node header fetch + in-browser SPV verification | v1 (locked Legend-0a) |
| Downloadable machine-verifiable proof export artefact | v2 (feeds £50 estate statement) |
| ZK-backed estate proof accepted by PI insurer methodology | v3 |

---

## 8 — Open items
*Source: `SESSIONS.md` carry-forwards + conflicts identified Legend-5*

- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **Custom FlokiNET quote** — open. Mandatory before provisioning node C.
- **`legend-incident-protocol.md`** — queued per `legend-node-plan.md` §9: node
  recovery, provider-switch protocol, FROST re-keying, attack-vector simulations.
  Two Opus sessions once infrastructure is live. Quarterly revision from v1 launch.
- **GBP denomination edit** — locked in CLAUDE.md v1.5 but not yet applied to
  `legend-ux-language.md` §4 and §8. Apply at next session touching that file.
- **Design-spec token block stale** — `legend-design-spec.md` Design tokens section
  still carries pre-CC-74 `--bg` values (`#F7F4EF`/`#1E1F22`). Canonical values are
  in BRIDGE and §5 above. Queue a single-line correction next time the spec is edited.
- **Licence statement divergence** — CLAUDE.md locks MIT for this repo;
  REFUELER-BRIDGE.md Legend section says Apache 2.0. CLAUDE.md is the repo
  authority (MIT, matching upstream). Correct BRIDGE at its next update.

---

## 9 — File index

| File | Version | Purpose | Loaded by |
|---|---|---|---|
| `CLAUDE.md` | 1.5 | Repo identity, locked decisions, locked phrases, build sequence | Every session |
| `SESSIONS.md` | rolling | Last 3–4 session log + carry-forwards | Every session |
| `MASTER.md` | 1.0 | This file — compression of the six planning documents | Every build session |
| `REFUELER-BRIDGE.md` | 2.0 | Cross-product platform context, canonical tokens | Cross-product and design sessions |
| `legend-node-plan.md` | 1.0 | Node topology, FROST, canaries, hardening, transport roadmap | Infrastructure and incident sessions |
| `legend-economics.md` | 1.0 | Cost model, scaling, break-even, self-hosting economics | Economics and commercial sessions |
| `legend-enterprise-pricing.md` | 1.0 | Tiers, contract terms, SLA, compliance pack, Swan (internal) | Sales, legal, onboarding sessions |
| `legend-design-spec.md` | 1.2 | Page architecture, query flow, modals, tokens, status page | UI/UX build sessions |
| `legend-ux-language.md` | 1.0 | Every locked user-facing string (Section 8 index) | Any session touching copy |
| `legend-scope.md` | 1.2 | Version-locked feature scope, NUT decisions, scope-creep rules | Every build session needing a version reference |

Standard build session load from here: `MASTER.md` + `SESSIONS.md` + one specific
detail file. `CLAUDE.md` loads alongside as always.

---

*"Nothing stops this train."*
