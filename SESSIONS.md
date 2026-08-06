# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-3B · 6 Aug 2026

**Phase:** 1 pre-build harness — UX language and information hierarchy
**Status:** Complete. `legend-ux-language.md` v1.0 produced.

### Completed

**legend-ux-language.md v1.0 produced. Eight sections:**

**Section 1 — Voice and register:**
- Governing register defined: legal-document precision at reader-first readability.
- Copy rules locked: precision over completeness, factual language over emotional language,
  honest scope before the reader asks, one idea per element.
- "Never" list: anonymous, military-grade, Swiss-grade, end-to-end (unless exact),
  zero-knowledge as v1 headline, PIR without qualification, exclamation marks.
- "Chainalysis works for the observer. Legend works for the owner." locked as article/presentation
  use only — not UI copy.

**Section 2 — Landing page hierarchy:**
- Headline locked: `Bitcoin, privately.` (carried from CC-77)
- Subhead: `The block explorer that cannot see what you searched.`
- Query input placeholder: `Address, transaction ID, or block height`
- Tertiary line: `Private query. No logs. No tracking.`
- Below-fold section titles: `How your query stays private` / `Built for the moment it matters` /
  `Free. No account. No catch.`
- Below-fold free-tier copy finalised from legend-economics.md §6 draft into register.

**Section 3 — Query flow copy:**
- All states: idle, focused, submitting (three-step progress text), result (intact, funds moved,
  not found standard, not found Silent Payments), error (generic, timeout, invalid input).
- Progress text: `Querying node 1 of 2…` / `Querying node 2 of 2…` / `Assembling result.`
- All strings exact.

**Section 4 — Result anatomy copy:**
- Status line format with middot separator. IBM Plex Mono / DM Sans split confirmed.
- UTXO consolidation advisor string locked: `Consolidating these UTXOs will permanently record
  co-ownership on-chain.` No further copy at result level.
- Denomination toggle labels: `sats` · `BTC` · `USD at time of transaction` (exact, third label
  is long form because historical fiat ≠ current fiat — accountant-legible).
- Silent Payments section: `Derived outputs` header, body copy, empty state.
- Batch result values `Intact` / `Activity detected` — explanation of why not Compromised or Swept
  included in the document (cause vs fact; partial vs total).

**Section 5 — Modal copy:**
- Onboarding modal: title, three lines, GitHub link, `Start querying` CTA.
- Credential status modal: free tier and Enterprise display strings. Footer architecture line
  uses precise v1 claim language.
- Batch input modal: title, placeholder, note, button labels, progress state.
- Privacy explainer modal: three sections (no session/no log; split across two nodes;
  IP and Tor), Article 16 link (hidden until published).

**Section 6 — Honest-scope statements:**
- Six canonical user-facing statements: ephemeral sessions, role-split querying (v1 precise
  language — "collusion-resistant query splitting with blind credential unlinkability"),
  Cashu credentials, no server-side logs, IP privacy, UK operator caveat.
- All consistent with legend-enterprise-pricing.md and legend-node-plan.md §6.
  User-facing versions are shorter; honesty is identical.

**Section 7 — Degraded mode notices:**
- N-1 banner and credential modal indicator. N-2 banner and credential modal indicator.
- Neither suppressed. Neither uses "secure" or "insecure". N-2 offers Tor as mitigation.

**Section 8 — Locked copy index:**
- Single table of every finalised string with surface, element, and section reference.
  50 entries. Build sessions pull from this table.

### Files produced

- `legend-ux-language.md` v1.0 — new file

### Carry-forward to Legend-4

- **Legend-4 is the node status page spec session.**
  Adds a node status page section to `legend-design-spec.md`.
  Covers: per-node reachability, chain sync height, canary status display, privacy mode
  indicator, what the status page does not show (no query volumes, no traffic graphs).
  Source: `legend-node-plan.md` §9 (redundancy and failover) and §6 (canary architecture).

- **CONTRIBUTING.md contribution-back norm** — still queued for first build session (Multi-8).

- **Custom FlokiNET quote** — still open. Required before provisioning node C.

- **Load for Legend-4:** `CLAUDE.md`, `SESSIONS.md`, `REFUELER-BRIDGE.md`,
  `legend-design-spec.md` (v1.1), `legend-scope.md` (v1.2), `legend-node-plan.md`,
  `legend-ux-language.md` (v1.0).

---

## Session Legend-3A · 6 Aug 2026

**Phase:** 1 pre-build harness — queued scope and spec edits
**Status:** Complete. No new file produced. All queued edits from Legend-0a and Legend-1 applied.

### Completed

**legend-scope.md → v1.2:**
- SP tweak index added as v1 build prerequisite under Silent Payments.
- Cashu NUT status table added. NUT-13 and NUT-09 permanently rejected with reasons.
  NUT-28 noted v2. NUT-24 noted v2+.
- Merkle proof scope table added: v1/v2/v3.
- Plain-language script rendering added as v2 item.
- Version summary table updated.

**legend-design-spec.md → v1.1:**
- €96 → €360 infrastructure cost correction applied.

### Files changed

- `legend-scope.md` v1.1 → v1.2
- `legend-design-spec.md` v1.0 → v1.1
- `SESSIONS.md` (this entry)

---

## Session Legend-2 · 5 Aug 2026

**Phase:** 1 pre-build harness — Enterprise packaging and commercial model
**Status:** Complete. `legend-enterprise-pricing.md` v1.0 produced. Pre-build harness closed.

### Completed (abbreviated)

- Family office pricing ladder locked: £1,500 (v1) / £2,500 (v2) / £3,500 (v2+).
- Five-client cap at v1. Invite-only.
- Contract: 3-month minimum, monthly rolling, 60 days' notice. Annual prepay at eleven months.
- SLA: 99.0% monthly / ≥95% full-splitting / 4 business-hour response /
  12-hour canary alerting / 10% credit below 97%.
- Merchant: £250 entity / £500 franchise flat (≤25 locations). Always add-on.
- Estate reports: £50 statement (v2) / £150 full report (v3).
- Swan approach timing: post-audit + post-/legend/verify + ≥12 months canary history.
- Compliance pack and PI insurer pack contents defined per tier.

### Files produced

- `legend-enterprise-pricing.md` v1.0

---

## Session Legend-1 · 5 Aug 2026

**Phase:** 1 pre-build harness — infrastructure costs and scaling economics
**Status:** Complete. `legend-economics.md` and `legend-enterprise-pricing.md` (v0.1) produced.

### Completed (abbreviated)

- V1 launch cost locked: ~€360/month midpoint, range €340–420/month.
- Hetzner AX52 (×2) + FlokiNET dedicated (×1). Previous estimates confirmed wrong.
- SP tweak index locked as v1 build requirement.
- Storage headroom to 2029 confirmed on 2 TB spec.
- Enterprise break-even: £1,500/month covers ~4.8 years of infrastructure.

### Files produced

- `legend-economics.md` v1.0
- `legend-enterprise-pricing.md` v0.1

---

*Next session: Legend-4 — node status page spec*
*Adds section to: `legend-design-spec.md`*
*Load: CLAUDE.md, SESSIONS.md, REFUELER-BRIDGE.md, legend-design-spec.md (v1.1),*
*legend-scope.md (v1.2), legend-node-plan.md, legend-ux-language.md (v1.0)*
