# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*

---
## Session 7 — Multi-7 · 3 Aug 2026

**Phase:** 1 — Working explorer by December
**Status:** Eleventy scaffold — Legend shell and SPA mount live in refueler-io

### Completed

- Architecture confirmed: Legend shell lives in `refueler-io`, not `refueler-legend`
  `refueler-legend` is the Rust indexer only — no Eleventy, no HTML
- Three files added to `refueler-io`:
  - `src/legend/index.njk` — static shell, includes shared nav/footer, SPA mount div,
    below-fold acquisition content (three columns: what it does, who it's for, free tier)
  - `src/assets/css/legend.css` — Legend design tokens, layout, SPA chrome, responsive
  - `src/assets/js/legend-spa.js` — SPA scaffold: mounts shell, input, batch icon,
    credential dot, all interactive stubs ready for Multi-8 query logic
- `src/_includes/nav.njk` updated: App → Legend, wordmark section variable
  (`{{ wordmarkSection or "Legend" }}`), App link replaced with `/legend/`
- `eleventy.config.js` updated: `addPassthroughCopy("src/assets")` added so
  CSS and JS reach `_site/`
- `REFUELER-BRIDGE.md` updated to v1.3: build status, cross-project actions section
  added covering nav CSS extraction, theme bug fix, API key rotation, and Legend
  sign-off checklist before Multi-8 opens
- All commits pushed: `refueler-io` (`fe6b88a`), `refueler-legend` (`e51474c`),
  `refueler-share` (BRIDGE only)

### Known issues — blocked on refueler-io housekeeping session

- Nav and footer render unstyled on Legend page — nav CSS is inline in `src/index.njk`
  only, not in a shared stylesheet. Fix documented in REFUELER-BRIDGE cross-project actions.
- Theme toggle non-functional on Legend page — same root cause.
- `head.njk` and `src/index.njk` use `localStorage` / `classList.add('carbon-mode')`
  in violation of locked spec. Fix documented in REFUELER-BRIDGE.

### Carry-forward to Multi-8

- Multi-8 does NOT open until refueler-io housekeeping session is complete and
  Legend sign-off checklist in REFUELER-BRIDGE is confirmed green.
- Multi-8: first query flow — address input → API call → result render (funds intact
  and funds moved states). No batch, no modals yet.

---

## Session 6 — Multi-6 · 3 Aug 2026

**Phase:** 1 — Working explorer by December
**Status:** Brand pass — Legend visual language confirmed

### Completed

- Phase 1 opens with brand pass per locked convention
- Wordmark confirmed: Satoshi 700, 1.75rem, --text-primary, no special treatment
- Query input confirmed: --surface bg, 0.5px --border, 8px radius, batch icon
  on focus in --accent (#C8A96E), tagline in --text-tertiary below
- Result typography hierarchy confirmed: five levels, UTXO table all --text-primary
  at same weight (no secondary-text treatment on any column), Intact / Activity detected
  in DM Sans 500 --text-primary with no colour differentiation
- Credential icon confirmed: 10px filled circle, --accent gold only, no glow,
  hover title, click opens modal
- Design register confirmed: calm, legal-document precision, refined — distinct from
  every existing explorer. Serves distressed Coldcard users and institutional clients
  with the same interface.
- No new design tokens. No new brand identity. Product surface under Refueler system.

### Carry-forward to Multi-7

- Multi-7: Eleventy scaffold for refueler.io/legend static shell + vanilla JS SPA
  mount point. No query logic yet — shell and mount only. ✓ (complete)

## Session 4 — Multi-4 · 3 Aug 2026

**Phase:** 0 — Foundation
**Status:** Design specification — Legend UI/UX, funding model, session roadmap

### Completed

- `legend-design-spec.md` written and committed — full UI/UX specification for Legend
- Page architecture confirmed: Eleventy static shell + vanilla JS SPA, no visible seam
- Query flow specced: idle, focused, submitting, three result states, error states
- Result anatomy locked: UTXO table, transaction history, consolidation advisor,
  Silent Payments section, denomination toggle (sats/BTC/USD)
- Breach scenario design complete: batch input modal, determinate progress,
  batch results modal with `Intact` / `Activity detected` language (not `Compromised`)
- Modal inventory complete: onboarding, credential status, batch input, batch results,
  privacy explainer. No top-up modal — free tier is unlimited at v1 launch.
- Cashu credential gate removed from free tier entirely. Distress moment is not
  a monetisation moment. This is locked.
- Funding model locked: Enterprise contracts (primary), merchant network pipeline,
  self-hosting as distribution, Share as discovery. No voluntary tip jar.
- Infrastructure cost established: ~€96/month for production Legend instance.
  One Enterprise client covers years of infrastructure.
- Design principles locked: calm register, information density as choice,
  new entrants must not be put off, branding sessions are first-class.
- All Refueler design tokens inherited. No Legend-specific tokens in v1.
- Session roadmap confirmed: 430 sessions across 9 phases to a world-class explorer.
  December target: ~session 21, end of Phase 1.
- Vision confirmed: Legend is professional infrastructure for a world where Bitcoin
  is global payments rail, second currency, preferred long-term savings account.
  Accountants train on it. Insurers price against it. Banks check it. Solicitors use it.
- London BitDevs identified as primary route to academic and peer reviewer relationships.
  No cold university outreach needed — show up, build in public, relationships follow.

### Key decisions locked (Multi-4)

- Free tier: unlimited queries at v1 launch. No rate limit. No account. No friction.
- Family office value proposition: institutional wrapper (contract, SLA, documentation,
  support, accountability) — not the software, which is free and open source.
- Open source strengthens Enterprise sale: "don't trust us, read the code" is a stronger
  statement to a sophisticated buyer than terms of service.
- Branding sessions added to every major phase. A tool people find beautiful is a tool
  institutions eventually pay for.
- Session roadmap: Phase 0 (6 sessions, complete), Phase 1–9 defined, ~430 total.
  Phases calibrated as build progresses.
- Share UI to be treated as diverged — Legend specced clean, not derived from Share.

### Carry-forward to Multi-5

- Multi-5: Legend product scope session — lock exactly which chains, protocols,
  and professional use cases are in scope for which version. Produces a locked scope
  document that every subsequent build session references.
- legend-articles-list.md: no changes this session
- notes-articles-list.md in refueler-share: no changes this session

---

## Session 3 — Multi-3 · 3 Aug 2026

**Phase:** 0 — Foundation
**Status:** Strategic planning — Legend economics, cryptographic foundations, architecture

### Completed

- Pleb tier query model resolved: 21 sats for 100 queries/day via Lightning (LNURL/Bolt12)
- Share-upload-to-Legend-queries link removed — was cross-product friction, not a reward
- Enterprise unlimited confirmed — PIR-sharded, Tor-native, Cashu credentialed
- Licence question closed: MIT stays, upstream fork compatibility non-negotiable
- Sparrow Wallet: custom Esplora endpoint, no plugin required, approach Craig Raw post-B9
- Sat rewards (170–2000 sats) confirmed Lightning-only, no on-chain, no Legend integration needed
- Blink/POS integration: not a Legend feature, clean separation confirmed
- Article pipeline architecture resolved: legend-articles-list.md lives in multi-core,
  built articles (HTML/NJK/styling) live in refueler-io — mirrors Share pattern
- Eleventy: landing page at refueler.io/legend only. Explorer interface is vanilla JS SPA.
  User sees no seam. Legend consumes existing Refueler nav and footer components.
- Paper as default theme. Carbon toggle. rs-theme cookie persists across all Refueler surfaces.
- Legend wordmark: product surface under Refueler design system, no separate identity yet.
- Modals confirmed useful: onboarding, credential status, batch query results.

### Cryptographic foundations locked

**The lineage:** Chaum 1982 → DigiCash 1989 → Bitcoin 2009 → Cashu 2022 → Legend 2026

- Chaum blind signatures (1982): direct ancestor of Cashu NUT-00 and Legend query credentials
- Pedersen commitments (1991): primitive for ZK balance proofs and UTXO set commitments
- Camenisch-Lysyanskaya credentials (2001): long-term architecture for Enterprise credential attributes
- Spiral PIR (MIT, 2022): single-server PIR for UTXO lookups
- Path ORAM: access pattern hiding, v2 target
- ZK balance proofs: Groth16/PLONK on bellman or arkworks
- Federated chain analytics: v3
- Silent Payments batch scanning optimisation: batch-verify via EC point aggregation, ~256x speedup

### Build sequence confirmed

**v1 (post-B9):**
PIR-inspired sharding, Cashu query credentials, ephemeral sessions, Silent Payments (BIP-352),
Tor API, fee estimation, UTXO consolidation qualitative flag, batch address monitoring,
Paper/Carbon themes, denomination toggle

**v2:** Spiral PIR, Path ORAM index, ZK balance proofs, UTXO provenance scoring,
stablesats/DLC recognition, homomorphic aggregate queries

**v3:** Federated chain analytics, Silent Payments BIP-352 contribution,
CL credential architecture

### Carry-forward

- Multi-4: Legend UI/UX design spec ✓ (complete)

---

## Session 2 — AP-7 ad-hoc · 2 Aug 2026

**Phase:** 0 — Foundation
**Status:** Strategic planning — Legend scope defined

### Completed
- Legend scope locked: privacy-first Bitcoin block explorer built on BLAKE3 Esplora foundation
- Product name confirmed: Legend
- URL confirmed: `refueler.io/legend`
- Licence confirmed: MIT
- REFUELER-BRIDGE.md updated with full Legend section
- CLAUDE.md updated to v1.1 reflecting dual scope
- README.md extended with Legend privacy layer section

### Carry-forward
- Multi-4: Legend UI/UX design spec ✓ (complete)

---

*Next session: Multi-8 — first query flow (address → result). Opens only after refueler-io housekeeping session confirms Legend sign-off checklist green.*
