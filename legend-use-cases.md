# legend-use-cases.md — Legend Use Case Library
> **Version:** 1.4 | **Created:** Multi-9 · 11 Aug 2026 | **Updated:** Multi-[n] restructure · 25 Aug 2026
> Civilisational use cases for Legend — UC-1 through UC-4 (complete specs).
> UC-5 through UC-9, Opus session structure, B9 gate note, and scope essay are in `legend-use-cases-2.md`.
> Load alongside `CLAUDE.md` and `SESSIONS.md` in any UC-1–4 reference session.
> For UC-5 onwards, load `legend-use-cases-2.md` instead.
> Companion: `legend-scope.md`, `legend-design-spec.md`, `legend-ui-modes.md`.

---

## Purpose of this document

Legend is described as a privacy-first Bitcoin block explorer. That description is
accurate and undersells it completely.

What Legend actually is: infrastructure for how value gets *read and trusted* at a
human scale — from a family in Bradford to a municipal treasury in Florence. The
scenarios in this document are not marketing exercises. They are design briefs.
Each one surfaces real architectural requirements, real interface decisions, and real
copy challenges that feed directly into build sessions.

**The governing design principle across all scenarios:**

> Design backwards from the human moment. Not forwards from the technology.

Every Opus use-case session takes one scenario, one named human, one specific moment,
and asks: what does this person see? What do they need to trust? What would make them
feel confident enough to act?

---

## Cross-scenario design principles

Five principles emerged from the founding session (Multi-9). They apply to every
scenario below and every interface decision that follows from them.

**1. Multiple principals, same data.**
The whale and the council look at the same address. Legend serves both without
conflating them. The interface knows who's asking by context, not by login.
No accounts. No tracking. Context inferred from query shape, not user identity.

**2. Verification as a first-class output.**
Not just display — exportable, independently verifiable artefacts. PDFs, shareable
links, Merkle proofs. Legend produces *documents*, not just pages. A third party
receiving a Legend verification link does not need to trust Legend — they can
verify the Merkle proof themselves. "Don't trust us, verify" extends to everything
Legend produces.

**3. Time and history matter as much as current state.**
The chain is a 40,000+ block permanent record. Most explorers show you *now*.
Legend shows *then, now, and pattern*. Fee spike history. Adoption curves.
Settlement rhythms. The chain as a civilisational ledger, not a live ticker.

**4. Plain language as a technical choice, not a dumbing-down.**
Every scenario involves a person who is not a developer. The interface speaks
their language without losing precision. "UTXO" → "unspent amount." "Block height"
→ "verified at block [number] · [date]." Precision and legibility are not in
tension. They require more craft, not less.

**5. Silence is information.**
An address that hasn't moved in five years is *saying something*. Legend surfaces
that — not as an alert, as a quiet affirmation. "Held since block 840,000.
No movement detected." That is estate planning data. Loan collateral data.
Generational wealth data. Most explorers show nothing when nothing has happened.
Legend tells you that nothing has happened, and means it.

---

## Scenario index

| # | Name | Human moment | UI mode | Article candidate | Opus session |
|---|---|---|---|---|---|
| 1 | The Bradford Inheritance | Estate lookup, phone, distress | Stewardship / Distress | No | UC-1 |
| 2 | The Grandparent's Ledger | Estate planning, laptop, unhurried | Stewardship / Legacy print | No | UC-2 |
| 3 | The Bitcoin-Backed Loan | Loan verification, tablet | Verification export | No | UC-3 |
| 4 | The Council and the Whale | Treasury oversight, desktop | Treasury watch | No | UC-4 |
| 5 | The Florentine District | Civic treasury + talent payment | Civic transparency | **Yes** | UC-5 |
| 6 | The Hanseatic Federation | Merchant federation settlement | Federation settlement | **Yes** | UC-6 |
| 7 | The Block War | Nation-state fee spike, global south | Chain context / historical | **Yes** | UC-7 |
| 8 | The UTXO Lottery | Provable movement, trustless draw | Lottery eligibility | **Yes** | UC-8 |
| 9 | The Recovery Coordination Layer | Mass-compromise, 2am distress / sovereign treasury | Distress → Recovery / Watch + Verification | **Yes** | UC-9 |

---

## Scenario 1 — The Bradford Inheritance

**UC-1. Opus session: Multi-12 · 23 Aug 2026. Full spec below.**

### The moment

It is 2031. A family in Bradford has held Bitcoin since 2026. The father has died
suddenly. The youngest daughter — 24, not technical, frightened — has a piece of
paper with a hardware wallet address on it. She opens Legend on her phone. She has
never used a block explorer before. She types in the address.

What does she see?

### What Legend must do in this moment

She is in distress. She cannot afford friction. She cannot afford jargon. She needs
one answer in the first five seconds: *are the funds still there?*

Everything else is secondary. The balance. In sats, with GBP equivalent visible
without asking. A clear statement that the funds have not moved. The date of the
last transaction. Nothing technical until she asks for it.

If the funds *have* moved — if someone swept the wallet already — she needs to know
that clearly, without euphemism, and she needs to see *when* and *where* the
movement was, in plain language. "Funds were moved on [date] to an address we
cannot identify" is honest. "Transaction detected: output to 1A3B…" is not a
human sentence.

### Distress Mode — full anatomy (locked Multi-12)

**Trigger conditions (all three required):**
- Mobile viewport
- First query of session
- Single address input

Never named or selected by the user. Inferred from query shape. If any of the three
conditions is absent, Distress Mode does not engage. This is not a setting.

**Onboarding modal behaviour:** Suppressed in Distress Mode. She came to check, not
to be onboarded. The modal fires on the second visit.

**Information hierarchy — top to bottom:**

1. **Balance** — large, prominent, sats value
2. **GBP equivalent** — surfaced without tap; overrides sats default for Distress Mode only. She is in Bradford. She thinks in pounds.
3. **Held / moved statement** — one sentence, plain language:
   - Funds intact: `These funds have not moved. Held since [date].`
   - Funds moved: `Funds were moved on [date]. See below.`
4. **Plain-language history** — collapsed by default, revealed on tap. "Received [amount] on [date]." "Sent [amount] on [date]." No txids in primary view.
5. **Technical detail** — second tap. TXID, block height, confirmations. For the solicitor, not for her.
6. **Document CTA** — single tap to generate a dated summary. Appears below the history, always visible. `Save a copy for your records →`

**Reduced chrome:** No credential status icon. No denomination toggle. No batch icon. No navigation links beyond the Refueler wordmark. The page exists to answer one question.

### Batch escalation (estate pattern)

If she queries 3 or more addresses within the same session, Legend infers an estate
or breach pattern and escalates to batch results view with Distress Mode tone.
This reuses the breach architecture wholesale — no new component.

Batch results in estate context: same `Intact` / `Activity detected` values.
GBP equivalent appears in the balance column. No modification to the underlying
batch query flow — the register change is copy and visual hierarchy only.

### Age gap and device gap

| User | Device | State of mind | Mode | Primary denomination |
|------|--------|--------------|------|---------------------|
| Daughter, 24 | Mobile | Distress | Distress Mode | GBP first |
| Father, 58 (alive) | Laptop | Stewardship | Stewardship Mode | Sats first |
| Grandfather | Laptop | Curiosity | Standard view | Sats first |

Legend detects device. It does not detect age. Device is a reasonable proxy for context. The same address returns the same chain data; the information hierarchy differs by detected context.

### Solicitor document output

| Version | Output | Cost | Who commissions | Notes |
|---------|--------|------|-----------------|-------|
| v1 | Dated summary — address, balance, last movement, verification block height, Legend query timestamp | Free | Daughter, in browser | Browser-generated. No paywall. No Merkle proof. Adequate for initial solicitor conversation. |
| v2 | Merkle-anchored + FROST-signed verified estate report | £50 | Solicitor commissions directly | Independently verifiable without Legend's involvement. Solicitor can check the Merkle proof. |
| v3 | £150 report with contextual metrics + IHT methodology aid | £150 | Solicitor commissions | Power law context, supply metrics, EO 6102 note, IHT methodology citation. |

**Bridge-document requirement (inherited from UC-2):** The v1 summary must carry full
untruncated addresses. A solicitor receiving the v1 printout may need to commission
a v2 report without the daughter present. The full address is the key. Truncation
at v1 breaks the chain of custody from v1 to v2.

### Version assignment (locked Multi-12)

- **v1:** Distress Mode, plain-language history, single-address mobile view, browser-generated dated summary with full addresses
- **v2:** Merkle-verified PDF export, FROST-signed, solicitor-legible format, solicitor commission flow
- **v3:** `/legend/verify` estate integration, IHT methodology, contextual metrics

---

## Scenario 2 — The Grandparent's Ledger

**UC-2. Opus session: UC-2 Opus · 23 Aug 2026. Full spec below.**

### The moment

Arthur is 74. He has held Bitcoin since 2020. He has two hardware wallets and a
paper backup in a solicitor's safe. He opens Legend on his laptop, unhurried,
on a Sunday afternoon, to check that everything is as he left it and to prepare
a summary for his solicitor.

He will read the same screen three times. He may print it. He wants to get it right
because this is for people he loves.

### What Legend must do in this moment

Arthur is not in distress. He is in *stewardship mode* — methodical, careful,
intending to act on what he finds. He wants:

- Confirmation that the addresses hold what he thinks they hold
- A clean summary he can print or email to his solicitor
- No jargon he has to look up
- Confidence that what he's reading is accurate and verifiable

### Stewardship Mode — full anatomy (locked UC-2 Opus)

Full anatomy in `legend-ui-modes.md`. Trigger conditions: desktop viewport, 1–2 addresses,
no outbound activity in recent history. Three-reads stillness constraint applies.

**Information hierarchy — top to bottom:**

1. **Stillness affirmation** — `Held since block [n]. No movement detected in [X] years.`
2. **Aggregate summary** — total held. Sats primary, GBP and BTC visible without toggle.
3. **Verification anchor** — `Verified at block [height] · [date] · [time]`
4. **Per-address breakdown** — full untruncated addresses. IBM Plex Mono throughout.
5. **Transaction history** — collapsed. Plain-language labels. Technical detail on second click.
6. **Tor notice** — `This check travelled over your normal internet connection.` (screen only)
7. **Print affordance** — `Print this summary →` — first-class, top-right of summary panel.

### Legacy print layout (locked UC-2 Opus)

`@media print` stylesheet. Carbon → Paper silently. Full anatomy in `legend-ui-modes.md`.

**Document header string (exact):**
`Bitcoin holdings summary · Prepared with Legend · [date] · Block [height]`

**Privacy footer string (exact):**
`Prepared for your records. Legend keeps no copy.`

**Full-address requirement:** Every address in full, untruncated. Chain-of-custody requirement.
A solicitor receiving this printout may need to commission a v2 Merkle-verified report without
Arthur present. Truncation breaks the bridge.

### Solicitor output table

| Version | Document type | Cost | Who commissions | What the solicitor receives |
|---------|--------------|------|-----------------|----------------------------|
| v1 | Personal summary — browser-generated | Free | Arthur, in browser | Dated summary with full addresses, balance, verification block height. Starting point for probate file. |
| v2 | Merkle-anchored + FROST-signed verified estate report | £50 | Solicitor commissions directly | Independently verifiable Merkle proof. FROST signature. Arthur does not need to be present. |
| v3 | Full verified report with contextual metrics + IHT methodology aid | £150 | Solicitor commissions | v2 report plus power law, supply audit, return windows, EO 6102 contextual note, IHT methodology citation. |

**Direction of commissioning:** v1 flows *from* the holder *to* the solicitor. v2 and v3 are
commissioned *by* the solicitor directly from Legend's paid report flow. Arthur does not need
to understand Merkle proofs.

### Version assignment (locked UC-2 Opus)

- **v1:** Legacy view (desktop), Stewardship Mode, silence-as-affirmation display, print layout with full addresses, browser-generated dated summary
- **v2:** Merkle-anchored + FROST-signed PDF export, solicitor commission flow, `/legend/verify` stub
- **v3:** Estate solicitor integration, IHT methodology aid, contextual metrics, `/legend/verify` full endpoint

---

## Scenario 3 — The Bitcoin-Backed Loan

**UC-3. Opus session: UC-3 Opus · 23 Aug 2026. Full spec below.**

### The moment

James and Priya hold 2.4 BTC. They want it as collateral for a £180,000 loan to
extend their house. The lender — a Bitcoin-native lending firm — needs to verify
the address holds what they claim, that nothing has left it in 90 days, and that
James can prove he controls it. James opens Legend on a tablet at the kitchen table.
Ruth, an underwriter at the lender, has her own Legend link open on a desktop across
town. They are not in the same room. The share link is the bridge.

### What Legend must do in this moment

Two principals, one address, opposite needs. James must *generate* a verification —
holdings, holding period, freshness, control. Ruth must *consume* it independently,
trusting only the chain and the proof — not James, not Legend. Neither reveals
anything to Legend: Legend does not know a loan is happening.

### Shared artefact and commissioning distinction (locked UC-3 Opus)

UC-2 (estate) and UC-3 (loan) consume one canonical v2 artefact — Merkle-anchored,
FROST-signed verified holdings statement — from opposite sides.

**Canonical v2 artefact fields:**
- Address set, full and untruncated
- Balance at reference block height (sats; fiat-at-block-height noted, not current)
- Reference block height + block hash
- Last outbound block height per address (the holding-period basis)
- Merkle inclusion proof (header + path) — verifiable offline against the chain
- FROST 3-of-4 signature over the statement
- Legend query timestamp
- Verifier instructions

**Commissioning distinction:**

| | UC-2 estate | UC-3 loan |
|---|---|---|
| Who commissions | Verifier (solicitor) | Subject (borrower) |
| Why | Subject is absent (deceased) | Subject is present, controls the keys |
| Proof of control | Absent — will and probate establish the link | Present (v2) — signed message binds the living borrower |
| Attests | Holdings at a historical block (date of death) | Current holdings + holding-period window + control |

This canonical v2 artefact also underlies UC-4's watch attestation and UC-9's Chain Trace Report.
Its field format is a single locked spec; changes must be checked against all four consumers
(UC-2, UC-3, UC-4, UC-9).

### Verification Mode — borrower-side generation (locked UC-3 Opus)

**Legend's first explicitly-invoked mode.** Distress and Stewardship are inferred; Verification
Mode is a deliberate act, taken from a result via `Create a verification →`. Never in Distress Mode.

Full anatomy in `legend-ui-modes.md`.

**Actions by version:**
- **v1:** `Copy shareable link` — address + reference block in URL fragment only; never sent to a server.
- **v2:** `Download signed verification` (canonical artefact) and `Add proof of control` (BIP-322).

**v1 link mechanism:** URL fragment carries the address set and reference block height. Fragment is
never transmitted to any server. The recipient's browser resolves it locally and runs in-browser
cross-node SPV to verify. Log-free and storage-free. The borrower has chosen to reveal the address
— the copy says so plainly.

### Lender View / Recipient View — recipient-side consumption (locked UC-3 Opus)

Read-only, chrome-less surface a share link opens into. Full anatomy in `legend-ui-modes.md`.

Three-reads stillness constraint applies. GBP-first denomination. Full untruncated addresses.
Theme respects `rs-theme` cookie; cold link → Paper. `dataset.theme === 'carbon'` detection only.

**Information hierarchy — top to bottom:**
1. Header: `Verified holdings · Prepared with Legend`
2. Context: `Shared with you by the holder. Verified against the Bitcoin network in your browser.`
3. **Balance** — GBP primary; sats secondary
4. **Holding period** — prominent; front and centre; the load-bearing collateral-quality signal
5. **Verification anchor (live)** — in-browser cross-node SPV result
6. **Borrower reference** — freshness delta
7. **Proof of control (v2)** — present or honest absence note
8. **Independent verification** — `Verify this yourself →`
9. **Per-address table** — full untruncated; IBM Plex Mono
10. **Honest-scope footer** — not a lien, re-check note

### Proof of control (v2 — locked UC-3 Opus)

BIP-322 signed-message verification. Legend checks the signature; never touches keys.
BIP-322 only — never BIP-137 (legacy; P2WPKH-only; not Taproot-compatible; not multi-sig-compatible).
A signed message proves control at the moment of signing — not ongoing.

### Honest scope — three absences, stated plainly

1. **Point in time, not a lien.** A verification confirms holdings when checked. Not a lien, not an escrow. Re-check before relying.
2. **Existence vs control.** A v1 export proves the coins exist and are still — not who controls them. Control binding is the v2 BIP-322 signed message.
3. **Disclosure vs privacy.** At v1/v2 the borrower chooses to reveal the address. Legend keeps the query private from Legend; it cannot make private a balance the borrower has chosen to show.

### Version assignment (locked UC-3 Opus)

| Version | What ships | Notes |
|---------|-----------|-------|
| **v1** | Verification Mode (generation), shareable link via URL fragment, Lender/Recipient View surface, holding-period display, in-browser cross-node SPV on recipient side, GBP-first Recipient View | Honest as a point-in-time re-check. Not a signed instrument. Stated explicitly as such in the UI. |
| **v2** | Canonical Merkle-anchored + FROST-signed downloadable artefact; proof of control via BIP-322 | The formal instrument the lender relies on. |
| **v3** | `/legend/verify` full endpoint + lender API + ZK balance proof — prove control of ≥ threshold without revealing the address | Address-hiding verification. The v3 sales argument activates here. |

---

## Scenario 4 — The Council and the Whale

**UC-4. Opus session: UC-4 Opus · 25 Aug 2026. Full spec below.**

### The moment

A county council in the West Midlands has accepted Bitcoin as collateral security
from a property developer — a Bitcoin whale — against a £40 million housing
development. The whale must show the collateral is real and unencumbered. The
council needs assurance, for the life of the covenant, that it has not moved.
Neither party fully trusts the other. Both want a trusted third party that is
not a bank.

Legend is that third party — and holds nothing, stores nothing, and knows
neither party. The chain is the witness.

The whale opens Legend on his laptop and creates a *watch* over the collateral
address. Legend produces a live watch link, and (v2) a FROST-signed watch
attestation he can hand over as a filed document. He shares the link with the
council.

The council's officer opens the link on a Monday morning. She sees, in pounds:
the collateral intact, verified live against the Bitcoin network in her own
browser as she opens it; whether it has moved; when the whale created the watch
and whether he proved control; and whether Legend itself is under legal
compulsion (canary). She has revealed nothing to Legend, and neither has he.

### What Legend must do in this moment

Two principals, one address, a standing covenant. The whale must *create* a watch —
holdings, movement status, control. The council must *consume* it repeatedly over
months, trusting only the chain and the proof — not the whale, not Legend. Legend
must never let the officer mistake a link she has to open for an alarm that reaches
her.

### The verification/watch boundary (locked UC-4 Opus)

A verification (UC-3) and a watch (UC-4) share one machinery: a fragment-encoded
address set, resolved by the recipient's browser via in-browser cross-node SPV
against the current chain, with nothing stored on Legend. They differ only in
temporal purpose, and the honesty burden inverts accordingly.

| | UC-3 verification | UC-4 watch |
|---|---|---|
| Supports | One decision (underwrite, disburse) | A standing covenant (18 months) |
| Temporal frame | Point-in-time snapshot | Live on open; dead between openings |
| Honest risk | Reader relies on a stale snapshot | Reader treats a pull link as a push alarm |
| Footer guards against | "This may be stale — re-check" | "Legend does not watch for you — no alert is not no movement" |

**Locked principle:** *A verification is a snapshot. A watch is a live link. Neither is an alarm.*

**Honest "future detection."** A watch that genuinely pushes notice of movement requires Legend
to hold the watched address and a contact channel — state Legend structurally refuses at v1. The
only honest push is v3 only, delivered through a blind, unlinkable channel (Pass Access-class
credential, per UC-9), reactive (post-confirmation), never preventive. Movement alerting is
delivered through a blind credential or it is not offered.

### Shared artefact — fourth consumer (locked UC-4 Opus)

The v2 watch attestation is the canonical Merkle-anchored, FROST-signed verified-holdings
statement (same fields as UC-3) framed as a watch, plus a `watch created at block [height]` line.

The field format is now shared across **four consumers — UC-2, UC-3, UC-4, UC-9.**
It is a single locked spec; any change must be checked against all four.

### Treasury Watch Mode — creator side (locked UC-4 Opus)

Legend's **second explicitly-invoked mode.** Appears on standard and Stewardship results;
**never in Distress Mode.** Reuses the verification/batch modal chrome — no new component.

Full anatomy in `legend-ui-modes.md`.

- **v1:** `Copy watch link` — URL fragment only; no server-side storage; re-resolves live on every open.
- **v2:** `Download signed watch attestation` and `Add proof of control` (BIP-322, at creation time only).

### Watch Recipient View — council side (locked UC-4 Opus)

Variant of the UC-3 Recipient View: same read-only, chrome-less surface, with institutional
register applied. Full anatomy in `legend-ui-modes.md`.

Key differences from the Lender View:
- **Live, not snapshot.** Re-runs in-browser cross-node SPV against the current tip on every open.
- **Movement status** is the load-bearing element (replaces holding period).
- **Legend canary summary line** — first canary inside a recipient surface. Reuses four-node canary state. No new machinery.
- **No-alert footer** — the load-bearing new copy discipline, inverting the UC-3 footer.
- **Institutional vocabulary** — collateral address, verified balance, movement status.
- **GBP-first**, per the locked contextual denomination rule.

### Honest scope — four absences, stated plainly

1. **A watch is not an alarm.** Live only when opened. Legend does not watch on anyone's behalf and, at v1/v2, sends no alerts. No alert is not evidence of no movement.
2. **Not a lien, escrow, or custody.** Legend holds nothing and cannot stop the collateral moving.
3. **Control is proven once.** A BIP-322 signature (v2) proves control at the moment of signing — not ongoing.
4. **The canary has limits.** A current canary means no warrant-based compulsion has been signalled by silence. It is not a guarantee against every legal instrument.

### Reuse note — UC-9 Elena track

The Watch Recipient View and Treasury Watch Mode are the upstream components Elena
(sovereign treasury officer) reuses in UC-9. Changes to these surfaces must be
checked against the UC-9 Elena track.

### Version assignment (locked UC-4 Opus)

| Version | What ships | Notes |
|---------|-----------|-------|
| **v1** | Treasury Watch Mode (create live watch link via URL fragment); Watch Recipient View (chrome-less, live re-verification, institutional register, GBP-first, canary summary line, no-alert footer) | Honest as a live, re-checkable link — not a push alarm, not a signed instrument. |
| **v2** | Canonical Merkle-anchored + FROST-signed watch attestation; proof of control via BIP-322 at watch creation | The filed document a council can cite in its records. |
| **v3** | Movement alerting via Pass blind credential (reactive, post-confirmation, unlinkable); council API; multi-signatory watch | The only honest "future detection". |

---

*"Chainalysis works for the observer. Legend works for the owner."*

---

*"Nothing stops this train."*
