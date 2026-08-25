# SESSIONS-ARCHIVE.md — refueler-legend
> Archived session log. Sessions prior to UC-3 Opus · 23 Aug 2026.
> Current session log: `SESSIONS.md`
> Archived: Multi-[n] restructure · 25 Aug 2026

---

## Session UC-2 Opus · 23 Aug 2026

**Phase:** Use case scoping — UC-2 The Grandparent's Ledger
**Status:** Complete. Stewardship Mode anatomy locked. Legacy print layout locked. Five new locked strings drafted.

### Completed

- **Stewardship Mode anatomy locked.** Trigger conditions confirmed: desktop viewport,
  1–2 addresses, not the 3+ batch escalation pattern. Full information hierarchy defined
  top to bottom: stillness affirmation → aggregate → verification anchor → per-address
  breakdown → history below fold → Tor notice → print affordance.

- **Three-reads stillness constraint locked.** Arthur reads the screen three times.
  No motion. No collapsing state. No timed elements. No auto-refresh. The interface
  is stable because the chain at the queried block height is stable.

- **Legacy print layout spec locked.** `@media print`, Paper-only (Carbon users silently
  rendered to Paper palette for print — no user-facing notice). Stylesheet strips: nav,
  footer, chrome, privacy explainer links, Tor notice. Stylesheet keeps: Legend wordmark,
  document header, per-address table (full untruncated addresses), aggregate summary,
  verification anchor, privacy footer. Page break rules for long address lists.

- **Bridge-document requirement identified.** The v1 print must carry full untruncated
  address strings. A solicitor receiving Arthur's printout may need to commission a v2
  Merkle-verified report without Arthur present. The full address is the only key they
  have. Truncation at v1 breaks the chain of custody from v1 to v2.

- **Solicitor output table confirmed.** UC-2 consumes UC-1's three-version ladder from
  the receiving side. Same artefact ladder; different commissioning direction.
  v1 commissioned by the holder (browser-generated, free, no Merkle proof — starting
  point for probate file). v2 commissioned by the solicitor (£50, Merkle-anchored,
  FROST-signed, independently verifiable without Arthur present). v3 commissioned by
  the solicitor (£150, v2 plus contextual metrics and IHT methodology aid).

- **UC-2 and UC-3 shared artefact noted.** UC-2's solicitor summary and UC-3's lender
  verification both consume the same Merkle-anchored v2 export artefact from opposite
  sides — solicitor receives it from the estate; lender receives it from the borrower.
  Same artefact, opposite commissioning direction. Feeds UC-3 session framing.

- **Stewardship register locked** as a sibling to the bereavement register (Multi-12).
  Denomination rule stated: Stewardship Mode keeps sats-first (returning owner); Distress
  Mode surfaces GBP-first (frightened newcomer). Both correct; rule is contextual, not global.

- **Five new locked strings drafted** for `legend-ux-language.md` §8 (stewardship register):
  1. `Held since block 840,000. No movement detected in 5 years.`
  2. `Nothing has happened here since [date]. For a long-term holding, that is what you want to see.`
  3. `This summary reflects the chain as of block [n], checked [date]. Anyone can verify it against the Bitcoin network independently.`
  4. `Prepared for your records. Legend keeps no copy.`
  5. `This check travelled over your normal internet connection.`

- **UC-1 expansion applied in same patch pass.** UC-1 was still brief version in
  `legend-use-cases.md`. Full spec (Distress Mode anatomy, trigger locks, device/age table,
  solicitor output, bridge-document requirement) applied to v1.2 alongside UC-2 expansion.

### Files changed

- `legend-use-cases.md` v1.1 → v1.2 (UC-1 full expansion + UC-2 full spec)
- `legend-design-spec.md` v1.4 → v1.5 (Stewardship Mode + Legacy print layout sections added)
- `legend-ux-language.md` v1.0 → v1.1 (bereavement register + stewardship register + five new locked strings + denomination rule)
- `legend-scope.md` v1.4 → v1.5 (estate-tooling v1 note: full-address requirement for bridge-document)
- `SESSIONS.md` (this entry)

### Carry-forward (closed)

All carry-forwards from this session resolved in UC-3 and UC-4.

---

## Session Multi-12 · 23 Aug 2026

**Phase:** Use case scoping — UC-1 The Bradford Inheritance
**Status:** Complete. UC-1 fully scoped. Two stale-copy flags identified and patched.

### Completed

- **UC-1 scoped: The Bradford Inheritance.** Full interface design produced for
  Distress Mode and Stewardship Mode. Eight standard use-case questions answered.
  Device/age split locked. Solicitor document output defined across v1/v2/v3.

- **Distress Mode trigger conditions locked.** Mobile viewport + first query of
  session + single address input. All three required. Never named or selected
  by the user — inferred from query shape.

- **Distress Mode anatomy locked.** Balance → GBP (surfaced without tap, overrides
  sats default) → held/moved statement → plain-language history (tap) → technical
  detail (second tap) → document CTA. Onboarding modal suppressed in Distress Mode.

- **Stewardship Mode trigger conditions locked.** Desktop viewport, unhurried
  single or small address set. Full width used for explanation. Silence-as-affirmation
  prominent. Print affordance first-class.

- **Estate escalation locked.** 3+ addresses in session → batch results view with
  Distress Mode tone. Reuses breach architecture wholesale — no new component.

- **Solicitor document output defined across versions.**
  v1: free dated summary, browser-generated, no paywall.
  v2: £50 Merkle-anchored + FROST-signed verified estate report (solicitor commissions).
  v3: £150 report with contextual metrics + IHT methodology aid.

- **Seven new locked strings drafted** for `legend-ux-language.md` — bereavement
  register sub-section and Section 8 additions. To be patched in next ux-language session.

- **Two stale-copy flags resolved:**
  - `legend-ux-language.md` §3, §6, §8: node count updated from 3→5, progress
    strings updated from `node 1 of 2` to `node 1 of 4` through `node 4 of 4`.
    Commit `04739b7`.
  - `legend-design-spec.md` query flow prose: shard framing replaced with node
    framing, §8 named as string authority. Commit `329ab13`.

### Files changed

- `legend-ux-language.md` — §3, §6, §8 node count and progress string corrections
- `legend-design-spec.md` — query flow submitting prose corrected

---

## Session Multi-11 · 23 Aug 2026

**Phase:** Legend holding page — pre-build action from UC-9 carry-forward
**Status:** Complete. Commit `d2fcb80` pushed to `rajesh-taylor/refueler-io`.

### Completed

- **Hero line locked:** "Public block explorers are surveillance tools with a search bar.
  Legend is the alternative." Discussed and rejected: "Every address lookup tells a server
  what you own" (inaccurate — user may be watching, not owning). Rejected "servers" plural
  (diffuses the threat). Settled on category reframe over query-logging angle.

- **Holding page structure decided:** Option 1 — no live input, no false affordance.
  `legend-wordmark` + hero line + launch line. No email capture (no mailing lists, ever).
  Notes pointer only. Link to `/notes/` until three Legend articles exist, then
  `/notes/legend/` (trivial Eleventy tag filter, one session when ready).

- **Launch line locked:** "Explorer launching soon — follow progress in Notes →"
  Dash variant chosen over two-sentence structure.

- **"Bitcoin" omitted from hero line** — URL, nav, and wordmark carry the context.
  Adding it reads as a disclaimer, not a confident statement.

- **Files delivered:**
  - `src/legend/index.njk` — new holding page (no SPA script, no broken input)
  - `src/legend/index-spa.njk` — current SPA shell preserved verbatim for Legend-6
  - `src/assets/css/legend.css` — holding page block appended (lines 128+);
    all existing rules untouched; removal note for Legend-6 included

- **Repo path corrected:** `~/Documents/refueler.io` (dot, not hyphen).

### Carry-forward (closed)

All carry-forwards resolved. Legend-6 build and `/notes/legend/` route remain open items in current `SESSIONS.md`.

---

## Session UC-9 Opus · 23 Aug 2026

**Phase:** Use case scoping — Recovery Coordination Layer
**Status:** Complete.

### Completed

- UC-9 scoped with two human tracks: Marco (individual victim) and Elena (sovereign treasury officer).
- Coldcard Mk3 event absorbed as the load-bearing attacker model.
- FROST decomposition corrected: three-layer model (blind membership credential + sealed individual component + mixed attestation quorum).
- Mixed attestation quorum locked: Legend + trustee + legal representative, 3-of-4 FROST.
- Recovery Mode defined as a distinct opt-in mode, separate from Distress Mode.
- Pass coordination credential architecture locked.
- Version assignment locked: v1 foundation, v2 chain trace + Share + Pass, v3 sealed format + quorum + trustee relationship.
- Lawyer sequencing locked: no lawyers before v3; two relationships at v3 approach.
- B9 gate lifted for Legend build sessions — Legend runs its own Lightning node.
- Article 24 reserved: "The Coordination Layer" — opens at v3 unlock.

### Files changed

- `legend-use-cases.md` v1.0 → v1.1
- `legend-articles-list.md` v1.3 → v1.4
- `SESSIONS.md`

---

## Sessions prior to UC-9 (summary index)

These sessions are fully applied to the document set. Session details are not retained.

| Session | Date | Key output |
|---|---|---|
| Multi-10 | Aug 2026 | CC-103 planning: post-B9 scope additions to legend-design-spec.md (Haiku helper, Alex profile, Sparrow, verified estate report contextual metrics). Legend-design-spec.md v1.3 → v1.4. |
| Multi-9 | 11 Aug 2026 | UC library founded. Nine scenarios scoped as briefs. Cross-scenario design principles locked. B9 gate lifted. |
| Multi-8 | Aug 2026 | Legend-8: Provider selection. Five nodes confirmed (Hetzner, Frantech/BuyVM, FlokiNET, OVHcloud CA, Infomaniak). Providers locked in legend-node-plan.md. |
| Legend-7B | 7 Aug 2026 | Five-node topology section added to legend-scope.md. Node D/E roles defined. FROST 3-of-4 threshold noted. PIR sharding row updated. |
| Legend-3B | 6 Aug 2026 | legend-ux-language.md created (all sections 1–8, v1.0). SP tweak index, Cashu NUT status, Merkle proof scope, plain-language script rendering added to legend-scope.md. |
| CC-77 | Jul 2026 | "Bitcoin, privately." headline locked. Landing page hierarchy established. |
| Multi-5 | 3 Aug 2026 | "Chainalysis works for the observer. Legend works for the owner." coined. legend-design-spec.md v1.0 created. legend-economics.md created. |
| Multi-4 | 3 Aug 2026 | Legend scope and query credit model locked. |
| CC-64 | 8 Jul 2026 | CLAUDE.md initialised. |

---

*"Nothing stops this train."*
