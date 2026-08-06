# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

---

## Session Legend-4 · 6 Aug 2026

**Phase:** 1 pre-build harness — node status page spec
**Status:** Complete. `legend-design-spec.md` v1.1 → v1.2.

### Completed

- Status page section added to `legend-design-spec.md` covering: page purpose and scope,
  three-section structure (privacy mode / node status / canary status), per-section layout
  and typography spec, canary expired state (muted amber, typographic only, no icon),
  two new CSS tokens (`--canary-expired`), explicit list of what the page does not show,
  URL and navigation placement.
- Privacy mode: four states locked. Full splitting / Reduced splitting / Single node /
  Credential rotation in progress. Rotation state colour: `--text-tertiary` (routine ceremony,
  not a user action required). Single node: `--text-secondary`. Full and reduced: `--text-primary`.
- Canary section visually separated from operational section by full-width rule. Different
  claims must not share visual language.
- FROST integrity note added beneath canary blocks — one line, `--text-tertiary`, for
  non-technical users reading an expired canary notice.
- Token additions: `--canary-expired: #B8860B` (Paper) / `#C9A227` (Carbon). No other
  status-page-specific tokens.

### Files changed

- `legend-design-spec.md` v1.1 → v1.2
- `SESSIONS.md` (this entry)

### Carry-forward

- **CONTRIBUTING.md contribution-back norm** — queued for first build session.
- **Custom FlokiNET quote** — still open. Required before provisioning node C.
- **legend-incident-protocol.md** — queued per `legend-node-plan.md` §9 carry-forward.
  Node recovery, provider-switch protocol, FROST re-keying, attack vector simulations.
  Two Opus sessions when infrastructure is live.

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
**Status:** Complete. No new file produced.

### Completed (abbreviated)

- `legend-scope.md` v1.1 → v1.2: SP tweak index, Cashu NUT status table, Merkle proof scope table, plain-language script rendering (v2), version summary updated.
- `legend-design-spec.md` v1.0 → v1.1: €96 → €360 infrastructure cost correction.

---

*Legend-1 and Legend-2 archived. Decisions captured in `legend-economics.md` v1.0 and `legend-enterprise-pricing.md` v1.0.*

---

*Next session: Legend-5 — MASTER.md compression*
*Produces: `MASTER.md` (~200-line summary of all harness documents with pointers to detail files)*
*Load: all nine current harness files — last time this many load simultaneously*
*After Legend-5: standard session load drops to `MASTER.md` + `SESSIONS.md` + one specific file (3 total)*
