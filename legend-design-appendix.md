# legend-design-appendix.md — refueler-legend
> **Version:** 1.0 | **Created:** Multi-[n] restructure · 25 Aug 2026
> **Extracted from:** `legend-design-spec.md` v1.7 (lines 988–1113)
> Post-B9 scope additions, session roadmap, and world-building context.
> Load only when working on roadmap, session planning, or post-B9 scope decisions.
> Not loaded by default in build or UC sessions.

---

## Post-B9 scope additions — locked CC-103 planning · 20 Aug 2026

### Design principle additions

**Legend is a block explorer. It is not a charting tool, a news aggregator, or a price terminal.**

No live price charting in the Legend UI. No news section. No market data feeds in the nav. Every feature decision is tested against one question: does this help a user understand what the chain says about an address, a transaction, or a block — privately, without logging, without a custodian in the chain?

The safety positioning is the product: **address lookup + Legend = the user always understands their privacy is intact.** FROST, non-logging, PIR-inspired sharding, Silent Payments, Payjoin-aware — these are not features listed in a sidebar. They are the reason Legend exists.

Legend never becomes:
- A news section
- A live price terminal (Glassnode exists and is well-funded)
- A portfolio tracker requiring account creation
- A social layer
- A comparison tool requiring third-party price feeds in the UI

Every one of these exists elsewhere. None has Legend's privacy architecture. The moment Legend becomes any of them, the differentiator evaporates.

### Scope addition 1 — Verified estate report: contextual metrics pages (v3 tier)

The £150 verified estate report (see `legend-enterprise-pricing.md`) gains a contextual metrics section generated at report creation time. Not live charting — a point-in-time signed document.

**Tone register:** FT Lex column applied to Bitcoin. Precise, dry, no hyperbole. A physicist or compliance officer reads it and finds nothing to object to on methodological grounds.

**Pages included in contextual section:**
- Current circulating supply vs 21M hard cap — sourced from Legend's own chain scan, not a third-party API
- Holdings as % of fixed supply
- Power law position at time of generation (log-log regression vs genesis block — Harold Christopher Burger methodology, cited and linked)
- 4-year, 8-year, 12-year return windows vs gold, S&P 500, UK gilts — sourced from a data API called at generation time, cited with source and timestamp in the report
- EO 6102 contextual note: one paragraph, factual — self-custodied Bitcoin and the historical precedent for gold confiscation (UK and US), and what cryptographic self-custody structurally changes about that calculus

**Architecture (non-negotiable):** The charting/returns data infrastructure does NOT live in Legend. It lives in a separate data API scoped in its own session, called at report generation time. Legend stays a block explorer. The report is a separate paid output layer. Separation of concerns is locked.

**Pricing:** unchanged — £150 full verified estate report. Contextual metrics pages included at that tier.

### Scope addition 2 — Haiku chain-state helper (paid tier, post-B9)

A Claude Haiku integration for paid Legend clients. Narrow, deliberate scope.

**What it does:**
- User pastes an address or txid into the Haiku panel
- Haiku receives chain-state output from Legend's own indexer — not a third-party API
- Returns plain-English explanation: last movement, holding period, KYC exchange interaction history (where detectable), power law context at current block height, Silent Payments compatibility, Payjoin flag where relevant
- Example register: *"This address last moved in 2019. It holds 0.847 BTC. It has never interacted with a known KYC exchange address. At current chain height this represents approximately 1 in 24 million addresses with a non-zero balance."*

**What it does not do:**
- Project prices or give investment advice
- Log the query (same non-logging guarantee as all Legend operations)
- Pull from any data source Legend does not already index
- Replace the explorer — it explains what the explorer found

**Privacy architecture:** Query goes user → Legend indexer → Haiku context window → response. Nothing persists. The address never leaves Legend's own infrastructure in a form correlatable by external API logs. Implementation detail to resolve at build session.

**Tier:** Paid clients only. Free tier gets the full explorer. Paid tier gets Haiku contextual explanation + estate report generation.

### The Alex profile — Legend's non-bitcoiner audience

Legend must work for the finance professional who holds gold, distrusts fiat, understands long-run asset returns, but has not yet made the step to Bitcoin. This profile (the Alex Williams archetype — physicist, fund manager, gold bug, sceptic of consensus) is a primary paid-tier target alongside Bitcoin-native family offices.

The four metrics that move this profile, in order:
1. **Supply audit** — verify the 21M cap independently, without trusting a Reuters feed. Gold's 197,000 tonne figure is World Gold Council data. Bitcoin's supply is verifiable by anyone running Legend or a node. That is not a marketing claim — it is a technical fact that a physicist immediately grasps.
2. **Power law** — log-log linear regression against time since genesis. A physicist sees an emergent scaling law, not speculation. Legend's estate report surfaces this with source and methodology cited.
3. **Return windows** — 4/8/12-year rolling performance vs every other asset class, every window positive. Presented as data with honest caveats, not marketing copy.
4. **EO 6102** — Roosevelt's 1933 US gold confiscation, UK equivalent. The question this raises for any gold holder: can a government do the same to Bitcoin held in self-custody? Legend's architecture is part of the honest answer.

The Haiku helper and estate report contextual pages are the tools that bridge this audience from curiosity to conviction — without Legend making any investment claim. Legend provides the data. The user draws the conclusion.

### Sparrow Wallet integration (Phase 8 — flagged CC-103)

Sparrow Wallet already allows users to configure a custom Esplora server endpoint. Legend's Esplora-compatible API (post-B9) means a Sparrow user can point their wallet at Legend with one URL paste — private address lookups with no logging, direct from their wallet, no browser required.

This is a distribution channel and a trust signal. A Sparrow user who is already running their own node and thinking about address privacy is exactly the Legend profile. Integration cost for the user is near zero. The Enterprise API tier's Sparrow-compatible endpoint is noted in `legend-enterprise-pricing.md`; the self-hosting guide section 6 covers the paste procedure.

Sparrow formalisation is Phase 8 (session roadmap). Do not build before Phase 2 privacy architecture is stable.

---

## Session roadmap — 430 sessions to a world-class explorer

Phases are calibrated as build progresses. Branding sessions are first-class throughout.

| Phase | Description | Sessions | Branding |
|---|---|---|---|
| 0 | Foundation — planning, architecture, scope lock | 6 | Multi-2 (complete) |
| 1 | Working explorer by December — Eleventy, SPA, query flow, Silent Payments, batch, Article 14 | 15 | Build-1 opens with brand pass |
| 2 | Privacy architecture — PIR sharding, ephemeral sessions, Tor API, first cryptographic review | 30 | Mid-phase brand review |
| 3 | Professional market layer — UTXO provenance, ZK balance proofs, compliance reporting, family office onboarding | 40 | Opens with brand pass for compliance audience |
| 4 | Lightning, Liquid, CoinJoin — channel correlation, confidential transactions, mixing recognition | 60 | Mid-phase brand review |
| 5 | Academic and audit layer — external cryptographic audit, responsible disclosure, academic paper co-authorship, reading programme | 50 | N/A |
| 6 | Enterprise infrastructure — API for accountants/insurers/lenders, professional credential architecture, institutional onboarding | 60 | Opens with brand pass for institutional audience |
| 7 | Federated analytics and v3 — federated chain analytics, CL credentials, Silent Payments BIP-352 contribution | 80 | Mid-phase brand review |
| 8 | Ecosystem contribution — Sparrow formalised, BIP contributions, Nostr integration, protocol work | 40 | Closing brand pass |
| 9 | Ongoing — security review, annual audit cycles, incident response, world events | 20/year | Annual brand health check |

**Total: ~430 sessions. Four to six years at current pace.**

The December working explorer lands at approximately session 21 (end of Phase 1).
The tool that sits on an accountant's desk alongside Bloomberg lands at approximately session 200.
The tool that redefines the category — that gets cited in regulatory working groups — lands at session 430.

You will present Legend at events before it is finished. Session 21 is the first presentation.
Session 430 is not the finish line. It is the point at which the category has been redefined.

---

## The world Legend is built for

Bitcoin is the global payments rail. The world's second currency. The preferred long-term
savings account. In that world:

- Accountants qualify in chain analysis the way they qualify in IFRS today
- Insurers price policies against UTXO age, provenance, and mixing history
- Banks check chain history before allocating loans
- Solicitors verify estate holdings with a tool their PI insurance accepts
- Compliance officers need an audit trail that holds up in court

Legend is the only explorer built for that world, private by design, audited in public.

The free tier is how Legend earns the trust of that world before the institutions arrive.
Every distressed Bitcoiner who checks their addresses and finds their funds intact —
without telling anyone what they were looking at — is a future referral into a
professional context.

---

*"Nothing stops this train."*
