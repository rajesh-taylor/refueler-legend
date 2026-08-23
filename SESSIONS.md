# SESSIONS.md — refueler-legend
*Rolling log — last 3–4 sessions only. Archive older entries.*
*Session naming: Multi-[n] through Multi-8. Legend-[n] from first build session after Multi-8.*

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

### Carry-forward

- **`legend-ux-language.md`** — add bereavement register sub-section to §1;
  add seven new locked strings to §8 (distress/stewardship copy). Next ux-language session.
- **`legend-use-cases.md`** — expand UC-1 from brief to full spec (Distress +
  Stewardship anatomy, trigger locks, device/age table). → v1.2.
- **`legend-design-spec.md`** — add Distress Mode and Stewardship Mode sections
  with full anatomies; note estate-escalation reuse of breach architecture.
- **`legend-scope.md`** — no change required; UC-1 version assignments consistent
  with existing version summary table.
- **UC-2 (Opus)** — The Grandparent's Ledger. Natural pair with UC-1. Can run
  next or alongside Legend-6 build prep.

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

### Carry-forward

- **Check live render** in both Carbon and Paper at `refueler.io/legend` post-deploy.
- **`/notes/legend/` tag route** — add when three Legend articles exist. One session,
  trivial Eleventy collection filter. Parked until Article A, B, C drafted and published.
- **Legend-6** — Eleventy SPA shell build session. Rename `index-spa.njk` → `index.njk`,
  remove holding page CSS block, wire query input. Load: `CLAUDE.md` + `SESSIONS.md` +
  `MASTER.md` + `legend-design-spec.md`.
- **All UC-9 carry-forwards remain open** (see UC-9 entry above).
- **index-spa.njk removed from repo** to clear Cloudflare retry loop.
  Restore at Legend-6 with:
  `git show d2fcb80:src/legend/index-spa.njk > src/legend/index-spa.njk`
  
---

## Session UC-9 Opus · 23 Aug 2026

**Phase:** Use case scoping — Recovery Coordination Layer
**Status:** Complete. UC-9 block produced. legend-use-cases.md v1.1, legend-articles-list.md v1.4, SESSIONS.md updated.

### Completed

- **UC-9 scoped: The Recovery Coordination Layer.** Two parallel human tracks —
  Marco (individual victim, mass-compromise event) and Elena (sovereign treasury
  officer, national fiscal event) — developed as asymmetric tracks of the same
  five-primitive architecture. Marco is the design driver; Elena is the proof the
  primitives generalise upward. Not the same screen at two scales.

- **Coldcard Mk3 event (August 2026) absorbed as the load-bearing attacker model.**
  ~1,200 BTC swept in a coordinated no-dust attack. Vulnerable UTXO set identified
  by AI-computed RNG pattern across the production batch — five years after device
  shipping. No test transactions. No dust. Single coordinated sweep window.
  Design consequence: the "dust then sweep" early-warning model is not the
  primary attacker profile for UC-9. Pass notification may arrive after the
  sweep, not before. Honest copy required.

- **FROST decomposition corrected.** "Victims co-sign" replaced by the honest
  three-layer model:
  1. Blind membership credential (Pass Access-class) — asynchronous, anonymous,
     does not encode group ID in a redemption-linkable way
  2. Sealed individual component — each victim's claim encrypted to the trustee
     quorum; submitted via Share; no victim sees another's data
  3. Mixed attestation quorum — Legend + trustee + legal representative, 3-of-4
     FROST; signs the aggregate envelope only; no single party can forge or block

- **Mixed attestation quorum locked.** Not Legend's internal nodes alone — a mixed
  quorum with the appointed trustee and a legal representative holding shares.
  Addresses the single-operator IPA compulsion gap from the threat model:
  a UK order on Legend's operator does not lapse the quorum.

- **Recovery Mode defined as a distinct opt-in mode**, separate from Distress Mode.
  Distress Mode is read-only. Recovery Mode is a legal and social action.
  The mode boundary is the consent moment. Never triggered automatically.
  Elena never enters Recovery Mode — she uses Watch/Verification (UC-4 mode).

- **UI mode table locked (UC-9):**
  - Distress Mode: entry point for both tracks
  - Recovery Mode: Marco only, explicit opt-in
  - Watch/Verification: Elena, institutional desktop context

- **Share v3 design constraint flagged.** If N victims submit to one fixed
  endpoint, Share and a network observer can count the group and correlate timing.
  Content hidden; fact of submission visible. v3 requirement: rotating ephemeral
  drop points, timing jitter. Design in, not bolt on. Surfaces in Share
  architecture session before v3. Copy from v3 launch: "Share hides what you
  send, not that you sent something."

- **Pass credential hard constraint locked.** Credential must not encode group ID
  in a redemption-linkable way. Attests: "entitled to participate in a
  Legend-coordinated recovery." Binding to specific recovery via sealed component
  only. Standard mint rotation rule applies. NUT-12 DLEQ mandatory.

- **Sovereign positioning locked: standard, not dependency.** Legend defines an
  open, verifiable format (MIT-licensed, independently implementable). A treasury
  adopts a format that outlives the operator, not a dependency on one person's
  infrastructure. "A Bitcoin treasury without verified recovery protocols in place
  is operating below the standard of care." Not a dependency claim.

- **Lawyer sequencing confirmed.** No lawyers required before v2 or v3 ships.
  Before v3 launch: one UK/common-law forensic Bitcoin specialist (extending Opus-C
  gate — firms: Mishcon, AWO, Pinsent Masons) + one civil-law equivalent.
  Two jurisdiction relationships, not fifty. Format published before relationships
  confirmed. Legend names the class of trustee; victim engages own counsel.

- **v3 gates confirmed: three, not one.**
  1. Sealed-component format built and tested
  2. Mixed attestation quorum operational (Opus-C + two-operator milestone)
  3. At least one real trustee/jurisdiction relationship confirmed
  Honest copy before gate 3: "Coordination format published. Operational trustee
  relationships pending."

- **Article 24 full outline produced.** Two-phase publish unchanged. Phase 1
  beats (Coldcard event, Distress Mode, Chain Trace, Share) at v2. Phase 2 beats
  (Recovery Mode, sealed-component model, Elena's arc, sovereign standard) at v3.
  Opening line locked: "When Mt. Gox collapsed, the Bitcoin was gone but the chain
  still knew where it went. The victims had no tool to read it. We're building
  that tool now."

- **legend-use-cases.md v1.1 produced.** UC-9 appended. Scenario index updated.
  "On the scope of Legend" closing section extended to include San Salvador.

- **legend-articles-list.md v1.4 produced.** Article 24 replaced with UC-9
  output. Article 14 updated to reference Coldcard Mk3 event. Version and date
  updated. Note added distinguishing Article 17 (UTXO consolidation) from
  Article 24 (Mt. Gox / Recovery).

- **Guardian relationship framing established.** Legend is the tool that makes
  the chain legible at every moment of a Bitcoiner's life — calm days,
  distress days, loss days. The format and the infrastructure are the guardian;
  the operator provides them. Distinction matters legally and in user copy.

- **Legend page — immediate pre-build action agreed.** Current state (blank
  result on address query) is unprofessional. Agreed: build a holding page
  at `refueler.io/legend` before the next build session. Either a slick
  "coming soon" using locked one-liners from planning documents, or a password
  gate. Build session to follow UC-9 immediately.

### Files changed

- `legend-use-cases.md` v1.0 → v1.1
- `legend-articles-list.md` v1.3 → v1.4
- `SESSIONS.md` (this entry)

### Carry-forward

- **Legend holding page** — immediate pre-build action. Before Legend-6 (Eleventy
  shell build session). Options: slick coming-soon using locked one-liners, or
  password gate. Decision and build in next session.
- **MASTER.md update** — UC-9 block, Share v3 constraint, mixed quorum, guardian
  framing, Coldcard attacker model. Update at next session that opens MASTER.md.
- **Opus-C solicitor gate extended** — now covers: IPA compelled-continuation
  question (existing) + sealed-component/attestation format receivable in UK
  insolvency context (UC-9 addition). Two questions, one engagement.
- **Two-operator milestone** — unchanged from Multi-10 carry-forward. Now also
  gates the mixed attestation quorum (UC-9 v3 gate 2).
- **FROST ceremony session** — unchanged. Required before v1 canary or mint live.
  Now also establishes the signing infrastructure that v3 Recovery quorum extends.
- **Share architecture session** — must address rotating ephemeral drop points
  and timing jitter before v3 Recovery Coordination Layer ships.
- **legend-node-plan.md header** — still reads "three nodes / FROST 2-of-3."
  Body reflects five-node / FROST 3-of-4 correctly. Fix header at next session
  touching that file.
- **All prior carry-forwards from Multi-10 remain open** (stage-2 obliviousness
  + k floor → Legend-6 opener; canary semantics → Opus-C; frost-secp256k1 audit
  pin; Nostr relay jurisdiction check; provider quote replies; two-operator
  milestone; stale figures in economics/enterprise-pricing/ux-language/design-spec).

---

## Session Multi-10 · 11 Aug 2026

**Phase:** Adversarial threat review — uncounted session
**Status:** Complete. `legend-threat-model.md` v1.0 produced and committed (`addd8af`).

### Completed

- **`legend-threat-model.md` v1.0 produced and committed.** Pre-build adversarial threat
  review of Legend v1 transport and query architecture. Three attacker profiles
  (Chainalysis tier, GCHQ/NCA tier, passive network/ISP observer), five attack surfaces,
  three structured questions (what is extractable, what is structurally unprotected, what
  is foreclosed). Commit: `addd8af`, 152 insertions. Prompt: `adversarial-1-opus-prompt.md`.

- **Stage-2 obliviousness identified as the load-bearing correctness check.** The v1 privacy
  claim rests entirely on whether stage-2 fetches the full candidate set (oblivious) or only
  the target row (not oblivious — collapse of the role-split). Must be verified and locked as
  a protocol invariant before query code is written. Minimum bucket size *k* also unspecified —
  must be documented. Both items → Legend-6 opener.

- **IP-join finding: collusion-resistance overstated as currently framed.** Both nodes see
  the same client IP seconds apart. IP+timestamp is default TLS-layer data. The honest v1
  floor is *k*-anonymity within a prefix bucket, keyed to IP. Free-tier distress users are
  the most exposed cohort. OHTTP (RFC 9458) is the natural v2 fix; nothing in v1 forecloses it.

- **Canary signalling gap documented.** Under 3-of-4, single-operator IPA compulsion does
  not lapse the canary — the other three participants keep it valid. "FROST resolves Boltz"
  is half true. Requires honest documentation in Enterprise materials and canary UX copy.
  Blocked on solicitor (Opus-C). Canary semantics → Opus-C / solicitor block.

- **Prospective logging risk under IPA Part 3 documented.** A Part-3 notice can compel
  silent addition of prospective logging. Architecture is not retrospectively reversible
  but can be inverted going forward. Also blocked on solicitor.

- **Nothing forecloses v2 transport improvements.** Full-index-per-node enables Spiral PIR
  additively. Browser-direct role-split does not foreclose OHTTP or per-connection Tor.
  No architectural replacement required for any planned v2 feature.

- **Hardening candidates absorbed into existing sessions — no new standalone block.**
  Three gating items: stage-2 obliviousness + *k* → Legend-6; canary semantics → Opus-C;
  2-of-4 unsignable confirmation → FROST ceremony session (already queued).

- **Two-operator milestone identified.** A second operator in a different jurisdiction,
  holding their own FROST share, is required before the first Enterprise contract is signed.
  Not a build blocker — an operational maturity milestone. Logged as carry-forward.

- **Single-node-first development sequence confirmed.** Node A (Hetzner, ~€77/month)
  is sufficient for the full v1 development stack. Five-node production topology
  provisioned when production privacy claims are made — not before.

- **Refueler IP honesty standard established as platform-wide principle.** Added to
  REFUELER-BRIDGE.md. Applies to all current and future Refueler products (Share, Legend,
  Pass, merchant terminal, ticketing). No product claims anonymity where IP is visible.
  All products recommend Tor for high-sensitivity use. All products plan OHTTP or equivalent
  as a v2 structural fix. This is a competitive advantage that cannot be retrofitted by
  competitors whose architectures were not designed with it in mind.

### Files changed

- `legend-threat-model.md` v1.0 (new file, committed `addd8af`)
- `SESSIONS.md` (this entry)
- `MASTER.md` (threat model block added)
- `REFUELER-BRIDGE.md` (Refueler IP honesty standard added)

### Carry-forward

- **Stage-2 obliviousness + minimum *k* floor** — lock in Legend-6 opener as the first
  output of that session, before any query code is written.
- **Canary signalling semantics** — what the canary does and does not signal under single-operator
  IPA compulsion. Fold into Opus-C / solicitor block. Cannot be resolved without legal input.
- **2-of-4 unsignable confirmation** — verify on real hardware in FROST ceremony session.
  Only one acceptable answer: 2-of-4 cannot produce a valid signature under the 3-of-4 scheme.
- **Two-operator milestone** — second operator, different jurisdiction, own FROST share,
  required before first Enterprise contract is signed. Operational milestone, not build blocker.
- **Nostr relay jurisdictions** — verify hosting jurisdiction of damus, nostr.band, nostr.wine.
  Cannot confirm without web access. Required before canary publication design is finalised.
- **`frost-secp256k1` audit status** — pin to a confirmed-audited version. Required before
  any node goes live.
- **All prior carry-forwards from Multi-9 remain open** (legend-enterprise-pricing.md break-even,
  legend-design-spec.md stale infrastructure figure, legend-ux-language.md §4/§8 GBP edit,
  FROST ceremony session, provider quote replies, solicitor engagement).

---

## Session Multi-9 · 11 Aug 2026

**Phase:** Planning and vision — use case library, session sequencing, B9 gate review
**Status:** Complete. `legend-use-cases.md` v1.0 produced and committed (`6e3eb82`).

### Completed

- **B9 gate formally lifted.** Legend runs its own independent node. Legend v1 has
  no Lightning dependency. The Cashu mint for query credentials is non-monetary and
  requires no Lightning node. B9 (Share Lightning node) is no longer a prerequisite
  for any Legend build or planning session. Decision logged here and in
  `legend-use-cases.md` §B9 gate section and `MASTER.md`.

- **`legend-use-cases.md` v1.0 produced and committed.** Eight civilisational use
  cases, each a full design brief with named human moment, interface implications,
  article candidate flag, and preliminary version assignments. Commit: `6e3eb82`,
  688 insertions.

- **Eight use cases defined (UC-1 through UC-8):**
  - UC-1: The Bradford Inheritance — estate lookup, distress mode, phone
  - UC-2: The Grandparent's Ledger — stewardship mode, legacy print, laptop
  - UC-3: The Bitcoin-Backed Loan — verification export, lender view, tablet
  - UC-4: The Council and the Whale — treasury watch, FROST attestation, desktop
  - UC-5: The Florentine District — civic treasury mode + article (dual output)
  - UC-6: The Hanseatic Federation — federation settlement mode + article (dual output)
  - UC-7: The Block War — human cost calculator, historical fee context + article
  - UC-8: The UTXO Lottery — movement eligibility, trustless draw, cryptographic sketch + article

- **Five cross-scenario design principles locked:**
  1. Multiple principals, same data — context inferred from query shape, not login
  2. Verification as a first-class output — exportable, independently verifiable artefacts
  3. Time and history matter as much as current state — chain as civilisational ledger
  4. Plain language as a technical choice — precision and legibility are not in tension
  5. Silence is information — "Held since block 840,000. No movement detected."

- **Four new article candidates identified:**
  - "The Florentine Protocol" — city-states, talent attraction, SP as Medici stipend envelope
  - "The Hanseatic Protocol" — merchant federation, Lightning settlement, on-chain bill of lading
  - "Who Gets Priced Out" — block war, human cost calculator, global south frame
  - "The UTXO Lottery" — movement velocity as monetary property, trustless draw

- **Opus session structure for use cases locked:**
  - One scenario per Opus session. Maximum two if thematically paired.
  - Natural pairs: UC-1+UC-2 (estate/inheritance device contrast), UC-5+UC-6 (civic/institutional)
  - UC-8 runs solo — half UX session, half cryptographic design
  - UC-9 runs solo — two parallel tracks, single architecture
  - Eight standard questions per session (see `legend-use-cases.md` §Opus session structure)
  - Standard outputs: interface spec → `legend-design-spec.md`, copy → `legend-ux-language.md`,
    version assignments → `legend-scope.md`, article outline → `legend-articles-list.md`

- **Session sequence confirmed** — see Session queue below.

- **Design vision discussion logged in `legend-use-cases.md`.** Key framing: Legend is
  infrastructure for how value gets read and trusted at civilisational scale. "Pandora's box
  rather than block explorer" — everything inside is useful. Design backwards from the human
  moment, not forwards from the technology.

- **"Absence is correct" principle named** — a display pattern for wherever privacy-preserving
  technology produces invisible-but-real activity. Show the signal. Explain the silence.
  Applies across Silent Payments display, Fedimint settlement, SP-paid civic stipends.

- **UTXO Lottery cryptographic sketch drafted** — Merkle lineage proof of movement history,
  block hash at predetermined height as entropy source, deterministic winner function
  (BLAKE3 of UTXO outpoint + draw block hash). Full design in UC-8 Opus session.

- **`legend-articles-list.md`** — Articles A, B, C added (queued since Multi-8, now done).
  Four new article candidates (UC-5, UC-6, UC-7, UC-8) added. Publishing sequence updated.

### Files changed

- `legend-use-cases.md` v1.0 (new file, committed `6e3eb82`)
- `legend-articles-list.md` (Articles A, B, C + four new candidates added)
- `MASTER.md` (B9 gate, session queue, use case block updated)
- `SESSIONS.md` (this entry)

### Carry-forward

- **Duplicate `### Carry-forward` heading** in Legend-7B entry — remove at
  next session touching SESSIONS.md directly. *(Still open.)*
- **legend-enterprise-pricing.md** — break-even floor still shows ~£385/month. Update to
  ~£566/month at next session touching that file.
- **legend-design-spec.md** — €96/month figure still stale. Replace with ~€673/month
  planning estimate at next session touching that file.
- **legend-ux-language.md** §4/§8 — GBP denomination edit still not applied. Apply at next
  session touching that file.
- **Design-spec token block** — pre-CC-74 stale `--bg` values still in spec. Fix at next
  design session.
- **FROST 3-of-4 ceremony** — blocked on nodes provisioned. Measure DKG duration on real
  hardware; replace "minutes" placeholder in SLA materials with confirmed figure.
- **Provider quote replies** — expected 10–11 Aug 2026. Reconvene when all five received
  to confirm final cost table and unlock `legend-economics.md` §6 published copy.
- **Solicitor (Opus-C)** — IPA compelled-continuation question. Firms: Bristows, Mishcon
  de Reya, AWO. Blocked until engaged.

---

*Legend-1 through Legend-3A archived. Decisions captured in `legend-economics.md` v1.0,
`legend-enterprise-pricing.md` v1.0, `legend-scope.md` v1.2, `legend-design-spec.md` v1.1.*

---

## Session queue

### Immediate — no gate

**Legend holding page (Sonnet build)**
Current `refueler.io/legend` returns blank result on address query — unprofessional.
Replace with either: (a) slick coming-soon using locked one-liners from planning documents,
or (b) password gate. Decision and build before Legend-6.
Load: `CLAUDE.md` + `SESSIONS.md` + `legend-design-spec.md`

**UC-1 (Opus) — The Bradford Inheritance**
Single scenario. The youngest daughter, phone, distress mode, father's estate.
Eight standard questions from `legend-use-cases.md`. Full interface design for
distress mode and stewardship mode. Device/age split. Solicitor document output.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Legend-6 (Sonnet build)**
Eleventy shell at `refueler.io/legend`. SPA mount point, Paper/Carbon theme wired,
query input idle state. CONTRIBUTING.md. Feeds from UC-1 output if UC-1 runs first.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-design-spec.md`

**UC-2 (Opus) — The Grandparent's Ledger**
Single scenario. Arthur, laptop, Sunday afternoon, stewardship mode.
Legacy view, stewardship summary, print layout, silence as affirmation.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Legend-7 (Sonnet build)**
Query flow, result states. Feeds from UC-1 output.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-design-spec.md`

**UC-3 (Opus) — The Bitcoin-Backed Loan**
Single scenario. James and Priya, tablet, lender verification.
Verification export flow, lender view, fiat prominence, holding period display.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**Articles draft block (Sonnet)**
Draft Articles A, B, C (queued). No publish. 2–3 weeks sitting time.
Outlines for Articles 14, 15 also worth roughing in during this block.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-articles-list.md`

**UC-4 (Opus) — The Council and the Whale**
Single scenario. West Midlands council, housing development collateral.
Treasury watch, FROST attestation, institutional language layer.
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-5 + UC-6 (Opus, paired) — The Florentine District + The Hanseatic Federation**
Two scenarios, thematically paired — both civic/institutional, both article candidates,
both use the "absence is correct" display pattern.
Outputs: civic treasury UI mode spec, federation settlement UI mode spec,
two article outlines ("The Florentine Protocol", "The Hanseatic Protocol").
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-7 (Opus) — The Block War**
Single scenario. Fee spike, global south, nation-state mempool attack.
Human cost calculator, historical fee context layer, Lightning correlation panel.
Article outline: "Who Gets Priced Out."
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md`

**UC-8 (Opus) — The UTXO Lottery**
Single scenario + cryptographic design session. Merkle lineage proof, block hash
entropy, deterministic winner function. Stress-test miner manipulation economics.
Article outline: "The UTXO Lottery."
Load: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + `legend-use-cases.md` + `legend-scope.md`

### Phase 1 build — Legend-6 through Legend-~15 (target: December 2026)

Eleventy shell → SPA query flow → result states → batch query / breach scenario →
Silent Payments display → Article 14 → `refueler.io/legend` live.
Each session: `CLAUDE.md` + `SESSIONS.md` + `MASTER.md` + one detail file.
UC Opus sessions run in parallel with build sessions — Opus thinks, Sonnet builds.

### CryptoRoadmap block — target: January 2027
*Runs after Phase 1 working explorer is live. Before any v2 build session
touches the PIR or ZK layers. Three Opus sessions + three Sonnet sessions.*

**CryptoRoadmap-1 (Opus) — Primitives audit**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-scope.md`
Cover: Ristretto255 vs secp256k1 for Legend and Share (ecosystem compatibility cost,
WebCrypto acceleration, Safari memory profile, honest performance figures — no
speculative multipliers); FROST + blind signatures against Crites-Komlo-Maller 2023
formalisation; threshold blind signatures as a combined primitive; Checklist PIR
(Henzinger et al. 2023) vs Spiral PIR 2022 for Legend's UTXO query pattern —
including the client hint payload size question which is the real adoption risk.
Output: `legend-crypto-primitives.md`

**CryptoRoadmap-2 (Opus) — Transport and scanning layer**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-node-plan.md`
Cover: Signal Sealed Sender as a query-sender-hiding model; ML-KEM-768 hybrid
against NIST FIPS 203 final spec and `ml-kem` Rust crate maturity; Double Ratchet
session initialisation latency on the Enterprise API; Fuzzy Message Detection
(Beck et al. 2021) for Silent Payments block subscription without scan key
revelation — false-positive rate at production block volume; post-quantum blind
signatures honest timeline.
Output: `legend-crypto-transport.md`

**CryptoRoadmap-3 (Opus) — ZK architecture and estate/lending use case**
Load: `CLAUDE.md`, `SESSIONS.md`, `MASTER.md`, `legend-scope.md`,
`legend-enterprise-pricing.md`
Cover: Bulletproofs+ (Eagen et al. 2022) vs Groth16 vs PLONK for ZK balance proofs
— proof size (solicitor emails a file), verification time (lender runs this), prover
time (client-side generation); `bellman` vs `arkworks` Rust ecosystem maturity 2026;
Camenisch-Lysyanskaya credentials for v3 Enterprise attributes; `/legend/verify`
endpoint architecture for a non-cryptographer solicitor; academic contribution
pathway and citation credibility requirements.
Output: `legend-crypto-zk.md`

**CryptoRoadmap-4 (Sonnet) — Primitives decisions**
Translates `legend-crypto-primitives.md` into version assignments and build
complexity estimates. Feeds back into `legend-scope.md` version tables.
Output: version table amendments, queued edits to `legend-scope.md`.

**CryptoRoadmap-5 (Sonnet) — Transport decisions**
Translates `legend-crypto-transport.md` into version assignments.
Output: queued edits to `legend-node-plan.md` transport section.

**CryptoRoadmap-6 (Sonnet) — ZK decisions**
Translates `legend-crypto-zk.md` into version assignments and the `/legend/verify`
build spec stub.
Output: `/legend/verify` spec stub, queued edits to `legend-scope.md`.

### Preliminary research filed (Legend-5)

The following claims from initial research are noted for CryptoRoadmap-1 calibration.
Do not treat as confirmed until the Opus session produces honest figures:

- Ristretto255 batch verification latency reduction: mechanism confirmed, multiplier
  (3–5×) is speculative — calibrate in CryptoRoadmap-1.
- Checklist PIR "controls 4 of 5 nodes" framing: overstates the guarantee for our
  three-node topology. The two-server security model needs precise mapping to three
  nodes before this claim appears anywhere.
- "secp256k1 not a natural fit for ZK": overstated. Correct version: Ristretto255
  eliminates cofactor edge cases, making ZK implementation harder to get wrong —
  not that secp256k1 cannot do ZK.
- Share/Safari performance claims (WebWorker throttling, memory footprint, hardware
  acceleration): directionally correct, require browser benchmarks before being
  stated as product claims.

### Phase 2 and beyond — post CryptoRoadmap block

v2 PIR architecture, Spiral or Checklist PIR (decided in CryptoRoadmap-4),
ZK balance proofs, post-quantum transport hardening, `legend-incident-protocol.md`,
Enterprise onboarding.

### refueler-ecash-lab

Internal research and test environment (NUT-12, NUT-14, NUT-18, NUT-20).
Red folder — not a GitHub repo yet. Make it a repo when B9 is stable and
Legend's mint design is locked enough to define what the lab needs to test.
Do not conflate with production mint architecture. Findings feed `mintInterface.ts`.

---

*"Nothing stops this train."*
