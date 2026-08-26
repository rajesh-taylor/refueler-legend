# legend-ui-modes.md — refueler-legend
> **Version:** 1.0 | **Created:** Multi-[n] restructure · 25 Aug 2026
> **Extracted from:** `legend-design-spec.md` v1.7 (lines 633–987, 1114–1310)
> UI mode specifications for Legend — all inferred and explicitly-invoked modes.
> Load in build sessions that touch a UI mode spec. Not by default.
> Companion: `legend-design-spec.md`, `legend-copy-index.md`, `legend-scope.md`

Modes covered:
- Stewardship Mode (inferred — desktop, 1–2 addresses)
- Legacy print layout (`@media print`)
- Verification Mode (explicitly invoked — `Create a verification →`)
- Recipient View / Lender View (share link surface)
- Treasury Watch Mode (explicitly invoked — `Create a watch →`)
- Watch Recipient View (watch link surface)

Distress Mode anatomy is in `legend-use-cases.md` UC-1. Its trigger conditions
(mobile, first query, single address) are the inverse of Stewardship Mode's.

---

## Stewardship Mode — UI mode spec

**Locked: UC-2 Opus · 23 Aug 2026**
**Source scenarios:** UC-1 (Bradford Inheritance) and UC-2 (Grandparent's Ledger)

### What Stewardship Mode is

Stewardship Mode is the display register for a holder who is methodical, unhurried,
and intending to act on what they find. Arthur on a Sunday afternoon. Not frightened.
Not in distress. Preparing something for people he loves.

It is not a named mode the user selects. It is inferred from query shape.

### Trigger conditions (all required)

- Desktop viewport
- 1–2 addresses queried in session
- The 3+ address pattern is *not* present — three or more addresses triggers batch/estate
  escalation regardless of desktop context

The trigger conditions are identical to the initial framing locked in Multi-12 for UC-1's
Stewardship Mode. UC-2 confirms them and adds the stillness constraint below.

### The three-reads stillness constraint

Arthur will read the screen three times. The interface must accommodate that.

- **No motion.** No animation on result arrival. No pulsing elements.
- **No collapsing state.** The result does not re-render, reflow, or partially hide itself.
- **No timed elements.** No auto-advancing displays. No "loading more" that triggers unprompted.
- **No auto-refresh.** The data is stable. The chain at the queried block height does not change.
  A refresh is only initiated by the user.

This is not an accessibility rule (though it has accessibility benefits). It is a design
principle for the stewardship context. An interface that moves after the user stops reading is
an interface the user does not trust.

### Information hierarchy — top to bottom

**1. Stillness affirmation**
The first element Arthur reads. Prominent. Calm. Exact string in `legend-copy-index.md` §8:
`Held since block 840,000. No movement detected in 5 years.`
(Values substituted at render time. Format locked.)

This is the most important sentence on the screen. Most explorers show nothing when
nothing has happened. Legend says it clearly.

**2. Aggregate summary**
Total held across all entered addresses. Three denomination lines visible simultaneously
(sats, BTC, GBP) — no toggle required in Stewardship Mode. Arthur thinks in multiple
denominations. The toggle persists for other modes; Stewardship Mode surfaces all three.

Reuses the batch-summing logic from the breach architecture — no new component.

**3. Verification anchor**
One line. IBM Plex Mono. Tertiary colour. Calm.
`Verified at block [height] · [date] · [time]`
This is the line Arthur quotes to his solicitor. It must be present. It must be findable
on print without hunting. It is not a secondary detail.

**4. Per-address breakdown**
One row per address. Full untruncated address (see Legacy print layout below for the
bridge-document requirement). Balance in sats. Last activity date in plain language.
IBM Plex Mono throughout. No colour coding. No status badges.

The table Arthur will print. Every row equally weighted — no primary/secondary treatment.

**5. Transaction history**
Below the fold. Collapsed by default. Revealed on click.
Plain-language labels: "Received [amount] on [date]" / "Sent [amount] on [date]".
Technical detail (TXID, block height) available on second click.
Reuses the UC-1 history component — no new component.

**6. Tor notice**
Below the per-address table. Tertiary text. DM Sans 300.
`This check travelled over your normal internet connection.`
Present in screen view. Omitted from print (see Legacy print layout).

**7. Print affordance**
First-class. Single button. Top-right of the summary panel. Not buried. Not secondary.
`Print this summary →`
Arthur came here to print something. Print is a first-class output, not an afterthought.

### Estate escalation reuse

If the session crosses the 3+ address threshold, Stewardship Mode yields to batch
escalation. The batch results view with Distress Mode tone engages — reusing the breach
architecture wholesale. No new component. The register change is copy and visual hierarchy
only, not new machinery.

---

## Legacy print layout — `@media print` spec

**Locked: UC-2 Opus · 23 Aug 2026**

### What the Legacy print layout is

A distinct CSS `@media print` stylesheet that fires whenever a user prints from
Stewardship Mode. Arthur prints his summary. The solicitor receives a document.
The document must look like a document, not a screenshot of a web page.

### Theme rendering on print

Carbon users are silently rendered to the Paper palette for print. No user-facing
notice of this rendering decision. Carbon tokens (dark backgrounds, light text) do not
print well on white paper. The document must be legible in a solicitor's office,
in daylight, printed on standard A4. Paper palette renders correctly in all these conditions.

This is a background CSS decision, not a UX decision. The user's theme choice is respected
on screen. Print output is always Paper-palette.

### What the stylesheet strips

- Refueler nav and footer
- Theme toggle button
- Credential status icon
- Batch icon
- Privacy explainer trigger links (`How is this private?`)
- Tor notice (present in screen view; honest on screen; not relevant to the printed
  document — the document is the output, not an explanation of how it was produced)
- SPA chrome (loading states, progress indicators)
- Any element with `data-no-print` attribute

### What the stylesheet keeps

- Legend wordmark — small, top-left corner
- **Document header string** (exact, locked):
  `Bitcoin holdings summary · Prepared with Legend · [date] · Block [height]`
- Per-address table — full untruncated addresses (see full-address requirement below)
- Aggregate summary — sats, BTC, and GBP values
- Verification anchor — block height, date, time
- **Privacy footer string** (exact, locked):
  `Prepared for your records. Legend keeps no copy.`
- Page break rules: insert page break before each address's detail section when
  printing more than three addresses, to prevent table rows from splitting across pages

### Full-address requirement

Every Bitcoin address in the print layout renders in full, without truncation. No
`bc1q...3f7a` abbreviation. The complete string.

**This is a chain-of-custody requirement, not a design preference.**

A solicitor receiving Arthur's v1 printout may need to commission a v2 Merkle-verified
report without Arthur present. The full address is the only key they have to do this.
If the address is truncated in the v1 document, the bridge from Arthur's living
instructions to the solicitor's independent verification is broken.

IBM Plex Mono. Font size reduced to `0.75rem` on print only if necessary to fit a
standard address on one line without wrapping. One address per row. No ellipsis.

### v1 / v2 visual distinction

The v1 and v2 print documents are deliberately different in appearance.

**v1 — personal record:**
Clean. Dated. Undecorated. A document Arthur printed himself. The Legend wordmark is
small and secondary. The data is primary. It looks like a careful person's careful record.
No cryptographic proof appendix. No signature block. No verifier instructions.

**v2 — commissioned verification:**
Same data, with a Merkle proof appendix, a FROST signature block, and verifier instructions
for the solicitor. The document looks like something commissioned by a professional. It is.

The v1 document does not pretend to be a v2 document. It does not include placeholder
proof blocks or signature fields. Honest scope: the v1 document is what it is — a dated
summary, browser-generated, not independently cryptographically verifiable without
re-querying Legend. The solicitor can verify it by running the same query. That is the
honest v1 claim.

---

## Verification Mode — generation spec

**Locked: UC-3 Opus · 23 Aug 2026**
**Source scenario:** UC-3 — The Bitcoin-Backed Loan

### What Verification Mode is

Legend's first explicitly-invoked mode. Distress Mode and Stewardship Mode are inferred
from query shape and never named by the interface. Verification Mode is a deliberate act —
the one moment Legend helps a user disclose rather than conceal.

It is triggered by a CTA on an existing result, not by query shape. It produces a
document — not a display.

### Entry point

`Create a verification →` — appended to the result view in standard and Stewardship
contexts. Never appears in Distress Mode (trigger conditions absent: mobile, first visit,
single address). Never appears on error or not-found states.

### Verification modal

**Title:** `Create a verification`

**Body — in order:**
1. What will be included (address set, balance at reference block, holding period since last outbound)
2. What it proves (these addresses held this balance, unmoved for this window, as of this block)
3. What it does not prove (who controls these addresses — that requires a signed message, v2)
4. Disclosure warning

Reuses batch-modal chrome (title, body, action buttons, cancel). No new component.

**Actions by version:**

| Version | Action |
|---------|--------|
| v1 | `Copy shareable link` |
| v2 | `Download signed verification` + `Add proof of control` |

### v1 share-link mechanism

The shareable link carries the address set and reference block height in the **URL
fragment only** — the `#` portion. The fragment is never transmitted to any server
by any HTTP client. No server-side record of what was shared.

The recipient's browser reads the fragment locally and runs **in-browser cross-node
SPV** to verify the claimed state against the chain. The verification is reproducible —
anyone with the link can re-run it. The borrower has chosen to reveal the address
to the recipient; the copy says so plainly (string 4 in the locked strings).

Optional QR code render on the modal for tablet-to-phone handoff. No additional
component beyond the QR library already in scope.

### v2 additions

`Download signed verification` — generates the canonical Merkle-anchored, FROST-signed
artefact on demand. PDF + machine-verifiable proof file + verifier instructions.

`Add proof of control` — prompts the in-wallet BIP-322 signing step. On completion,
Legend verifies the signature and records `Control demonstrated by signature · [date]`
in the artefact header. Legend never handles keys. BIP-322 only — never BIP-137
(legacy; P2WPKH-only; not Taproot-compatible; not multi-sig-compatible).

---

## Recipient View / Lender View — spec

**Locked: UC-3 Opus · 23 Aug 2026**
**Source scenario:** UC-3 — The Bitcoin-Backed Loan (loan instance)
**Reused by:** UC-2 solicitor receiving view, UC-4 Watch Recipient View (watch variant), UC-9 Elena track

### What the Recipient View is

The read-only, chrome-less surface a share link opens into. The loan instance is named
the Lender View; the pattern is the Recipient View reused across scenarios with
context-appropriate framing. Same stripped surface, same artefact substrate.

### Three-reads stillness constraint

Applies here as in Stewardship Mode. The recipient is making a decision from this screen.

- No motion. No collapsing state. No timed elements. No auto-refresh.
- The result is stable. The chain at the verified block height does not change.

### What is stripped

- Refueler nav and footer
- Query input
- Batch icon
- Credential status icon
- Onboarding modal (entirely — no cookie check needed; this is a read-only surface)
- Theme toggle
- SPA chrome (loading states, progress indicators)
- Privacy explainer trigger (`How is this private?`)
- Any element with `data-no-print` attribute

### What remains

- Legend wordmark (small, top-left)
- Header line and recipient context line
- Balance (GBP primary, sats secondary)
- Holding-period statement (prominent)
- Verification anchor (live: in-browser cross-node SPV result)
- Borrower reference line (freshness delta)
- Proof-of-control line (v2) or honest absence note
- Independent verification affordance
- Per-address table (full untruncated addresses)
- Honest-scope footer

### Theme

Respects `rs-theme` cookie if the recipient's browser has one (existing Refueler user).
Cold link with no cookie → Paper default. Detection via `dataset.theme === 'carbon'`
only. Never `classList.contains`.

### Denomination

GBP-first. The counterparty — lender, solicitor, council officer — thinks in pounds,
not sats. Sats visible, secondary. Per the contextual denomination rule (see
`legend-copy-index.md` §8): denomination follows the reader's relationship to the
holding. The owner reads in sats. The non-owner professional counterparty reads in fiat.

### Information hierarchy — top to bottom

**1. Header**
`Verified holdings · Prepared with Legend`
DM Sans 500. `--text-primary`.

**2. Recipient context**
`Shared with you by the holder. Verified against the Bitcoin network in your browser.`
DM Sans 300. `--text-tertiary`. One line. Sets the relationship and the verification claim.

**3. Balance**
GBP primary — large. IBM Plex Mono. `--text-primary`.
Sats secondary — one line below. IBM Plex Mono. `--text-secondary`.

**4. Holding period — the load-bearing element**
`No outbound movement across these [k] addresses in [N] days · [M] blocks.`
(Singular variant: `No outbound movement from this address in [N] days · [M] blocks.`)
DM Sans 500. `--text-primary`. Prominent. Front and centre.
This is the collateral-quality signal. "Silence is information" rendered for an
underwriter. No other explorer surfaces this as a plain-language front-and-centre claim.

**5. Verification anchor (live)**
`Verified against the Bitcoin network at block [height] · [date].`
IBM Plex Mono 400. `--text-secondary`. One line.
This is the line Ruth quotes to her compliance team.

**6. Borrower reference**
`The holder generated this verification at block [height] · [date].`
DM Sans 300. `--text-tertiary`. Freshness delta — how long ago the holder checked.

**7. Proof of control**
Present (v2): `Control of this address was demonstrated by signature · [date].`
Absent: `This verification confirms the holdings. It does not prove who controls them. Ask the holder for a signed message if you need that.`
DM Sans 400. `--text-secondary`.

**8. Independent verification affordance**
`Verify this yourself →`
v1: explains and re-runs in-browser cross-node SPV.
v2: downloads Merkle proof + verifier instructions file.

**9. Per-address table**
Full untruncated addresses. IBM Plex Mono throughout. One row per address: address,
balance (sats), last activity. Same chain-of-custody requirement as UC-2 print:
Ruth may need to re-verify or commission a further report from nothing but this link.

**10. Honest-scope footer**
`This confirms what these addresses held when checked. It is not a lien or an escrow.
The holder can move these funds at any time. Re-check before you rely on it.`
DM Sans 300. `--text-tertiary`. Always visible. Never hidden or collapsed.

### Honest-scope footer — design note

This footer is the most important copy discipline on the Recipient View. It is not
a legal disclaimer in small print at the bottom. It is an honest statement of what
Legend can and cannot certify, in the same register as everything else on the screen.
The font is small because it is secondary; the copy is unambiguous because the
recipient needs to understand the limit before they rely on the document.

---

## Treasury Watch Mode — generation spec

**Locked: UC-4 Opus · 25 Aug 2026**
**Source scenario:** UC-4 — The Council and the Whale

### What Treasury Watch Mode is

Legend's **second explicitly-invoked mode** (after Verification Mode). Distress and
Stewardship are inferred from query shape and never named by the interface. Treasury Watch
Mode is a deliberate act — creating a live, re-checkable covenant link rather than a
point-in-time snapshot.

Like Verification Mode, it appears on standard and Stewardship results. **Never in
Distress Mode** — a bereaved heir on a phone is not administering a collateral covenant.

### The verification/watch distinction

| | Verification (UC-3) | Watch (UC-4) |
|---|---|---|
| Temporal mode | Point-in-time snapshot | Live on open; dead between openings |
| Honest risk | Reader relies on a stale snapshot | Reader treats a pull link as a push alarm |
| Footer purpose | "This may be stale — re-check" | "Legend does not watch for you — no alert is not no movement" |
| Artefact | Verification link or signed PDF | Watch link or signed watch attestation |

**Locked principle:** *A verification is a snapshot. A watch is a live link. Neither is an alarm.*

### Entry CTAs

Two distinct CTAs must be visibly distinguishable on the result view:

`Create a verification →` — snapshot (UC-3)
`Create a watch →` — live link (UC-4)

Never on error or not-found states. The two CTAs must be visibly distinct: the
snapshot/live distinction is the whole point of the mode and must not blur at the button.

### Watch creation modal

**Title:** `Create a watch`

**Body — in order:**
1. What a watch is (a live link; the counterparty's browser checks the address on
   each open and shows current state)
2. How it differs from a verification (a verification is one moment; a watch is
   checkable at any time and always shows the latest state)
3. What it discloses (sharing a watch reveals the address to whoever holds the
   link — same disclosure logic as a verification)
4. What it does not do (Legend monitors on no one's behalf, sends no alerts, and
   cannot hold or freeze funds)

Reuses the verification/batch modal chrome. No new component.

**Actions by version:**

| Version | Action |
|---------|--------|
| v1 | `Copy watch link` |
| v2 | `Download signed watch attestation` + `Add proof of control` |

### v1 watch-link mechanism

Identical to the UC-3 verification link: address set + reference block in the URL
**fragment only**; never transmitted to any server; no server-side record. The
recipient's browser re-resolves the fragment and re-runs in-browser cross-node SPV
**on every open**, against the current chain tip. The only difference from a
verification link is intent and the recipient-side copy — the mechanism is the same.

Optional QR render for tablet-to-desktop handoff. No component beyond the existing
QR library.

### v2 additions

`Download signed watch attestation` — generates the canonical Merkle-anchored,
FROST-signed artefact, framed as a watch (adds a `watch created at block [height]`
line). A signed document is a snapshot by definition; the attestation is
point-in-time and filable. PDF + machine-verifiable proof + verifier instructions.

`Add proof of control` — the in-wallet BIP-322 signing step, at watch creation
only. Legend verifies the signature and records `Control demonstrated by signature
· [date]`. Legend never handles keys. BIP-322 only — never BIP-137.

---

## Watch Recipient View — spec

**Locked: UC-4 Opus · 25 Aug 2026**
**Source scenario:** UC-4 — The Council and the Whale
**Base surface:** Recipient View (UC-3). This is the watch variant.
**Reused by:** UC-9 Elena track (sovereign treasury watch/verification)

### What the Watch Recipient View is

The UC-3 Recipient View, live rather than snapshot, with the institutional register
applied. Everything stripped in the Recipient View is stripped here. Everything kept
there is kept here, with the differences noted below.

### Three-reads stillness constraint

Applies. The officer makes oversight decisions from this screen over many months.
No motion, no collapsing state, no timed elements, no auto-refresh. The view is
live *on open* — it resolves once, when opened, and then holds still. It does not
poll or tick. A monitor that moves on its own is a monitor she cannot read three
times, and worse, one that implies it is watching when it is not.

### Denomination

GBP-first. Sats secondary. Per the contextual denomination rule — the non-owner
professional counterparty reads in fiat.

### Theme

Respects `rs-theme` cookie if present; cold link → Paper. `dataset.theme ===
'carbon'` detection only, per nav lock. Never `classList.contains`.

### What differs from the Lender View

1. **Verification anchor is present-tense and live** ("checked … in your browser"),
   re-run on every open against the current tip.
2. **Movement status replaces holding period** as the load-bearing element.
3. **A Legend canary summary line is added** — the first canary summary inside a
   recipient surface. Reuses the four-node canary state from the status page (§Canary
   status); no new machinery beyond the existing `--canary-expired` token.
4. **The honest-scope footer inverts** — from "may be stale, re-check" to "not an
   alarm, no alert is not no movement".
5. **Institutional vocabulary** throughout: collateral address, verified balance,
   movement status.

### Information hierarchy — top to bottom

**1. Header**
`Collateral watch · Prepared with Legend`
DM Sans 500. `--text-primary`.

**2. Recipient context**
`Shared with you by the holder. Verified live against the Bitcoin network in your
browser each time you open this.`
DM Sans 300. `--text-tertiary`. One line. Sets the relationship and the *live* claim.

**3. Movement status — the load-bearing element**
Intact (plural): `No outbound movement recorded on this collateral as of block
[height] · [date].`
Intact (singular): `No outbound movement from this address as of block [height] ·
[date].`
Moved: `Movement recorded. [amount] left on [date] · block [height]. See below.`
DM Sans 500. `--text-primary`. Prominent, front and centre. Non-euphemistic in the
moved case — the same honesty discipline as Distress Mode's funds-moved sentence.
No alarming colour; the sentence carries the weight, not the palette.

**4. Verified balance**
GBP primary — large. IBM Plex Mono. `--text-primary`.
Sats secondary — one line below. IBM Plex Mono. `--text-secondary`.

**5. Verification anchor (live)**
`Checked against the Bitcoin network at block [height] · [date], in your browser.`
IBM Plex Mono 400. `--text-secondary`. The line the officer quotes to her committee.

**6. Watch reference**
`The holder created this watch at block [height] · [date].`
DM Sans 300. `--text-tertiary`. The covenant's origin point.

**7. Proof of control**
Present (v2): `Control of this collateral was demonstrated by signature when the
watch was created · [date]. A signature proves control only at the moment of
signing.`
Absent: `This confirms the collateral exists and is unmoved. It does not prove who
controls it. Ask the holder for a signed message if you need that.`
DM Sans 400. `--text-secondary`.

**8. Legend canary summary**
Current: `All four Legend canaries are current — what this means →`
Expired: `One or more Legend canaries have expired — see status →`
DM Sans 400. `--text-secondary`. Expired variant uses `--canary-expired` for the
link text value only. The face-line is factual; the honest limits (3-of-4 does not
catch single-operator compulsion; a technical capability notice may not lapse a
canary) live in the explainer the link opens. The face-line never claims Legend "has
not been legally compromised." Reuses the canary state from the status page. No new
token beyond the existing `--canary-expired`.

**9. Independent verification affordance**
`Verify this yourself →`
v1: explains and re-runs in-browser cross-node SPV.
v2: downloads Merkle proof + verifier instructions.

**10. Per-address table**
Full untruncated addresses. IBM Plex Mono throughout. One row per address: address,
verified balance (sats), last activity. Same chain-of-custody logic as UC-2 print —
the officer may need to re-verify or commission an attestation from nothing but
this link.

**11. Honest-scope footer — the load-bearing copy discipline**
`This is live only when you open it. Legend does not watch this address on your
behalf and sends no alerts — no alert is not evidence of no movement. Legend cannot
hold or freeze these funds. Check again on your own schedule.`
DM Sans 300. `--text-tertiary`. Always visible. Never hidden or collapsed.

This footer is the single most important discipline on the Watch Recipient View. It
is what stops a pull link being read as a push alarm. It is not fine print; it is an
honest statement of what the watch is, in the same register as everything above it.

### Design note — what is deliberately absent

- **No "last checked by you" element.** Legend keeps no record of the officer's
  visits; such a line would either be a lie or require state Legend refuses to hold.
- **No "next scheduled check."** Legend schedules nothing. The cadence is the
  council's to set and keep.
- **No traffic-light "all clear" badge.** Implies active monitoring. The movement
  status sentence carries the signal; no badge is needed or honest.

---

## Civic Treasury View — spec

**Locked: UC-5 Opus · Multi-14 · 26 Aug 2026**
**Source scenario:** UC-5 — The Florentine District
**Base surface:** Recipient View (UC-3). New family member — *not* a Watch Recipient View variant.
**Reused by:** UC-6 Federation Settlement View (variant of this surface).
**Recipient-View family (all four members):** Lender View (UC-3) · Watch Recipient View (UC-4) · Civic Treasury View (UC-5) · Federation Settlement View (UC-6). One substrate; four load-bearing elements. Changes to the substrate must be checked against all four.

### What the Civic Treasury View is

The read-only, chrome-less surface for a declared public institutional Bitcoin treasury.
It answers one question for the public observer: is this treasury real, funded, and paying
out consistently? It does not answer who the recipients are (Silent Payments design),
whether the declared set is complete (it may not be), or who controls the treasury
(Legend attests no identity — only chain data).

This is the Recipient-View substrate applied to voluntary *public* institutional disclosure
to an unbounded audience. The failure mode is the inverse of the Lender View: not a reader
relying on a stale snapshot, but a reader mistaking a declared, partial disclosure for a
complete audit — and mistaking Legend's rendering for an identity endorsement.

### Three-reads stillness constraint

Applies. A cryptographer evaluating an offer from a canton reads this screen more than
once before deciding. No motion, no collapsing state, no timed elements, no auto-refresh.
The view resolves once on open and holds still.

### Denomination

BTC-first. Sats secondary. Fiat-at-time available via toggle.

This is the fourth contextual denomination rule (locked UC-5 Opus): a declared
institutional reserve states itself in BTC, not fiat. Leading in pounds undercuts
the declaration the institution is making. The unifying principle ("denomination
follows the reader's relationship to the holding") holds — the *institution's*
relationship to its own reserve is in BTC; the public observer reads it the same way.

### Theme

Respects `rs-theme` cookie if present; cold link → Paper. `dataset.theme === 'carbon'`
detection only. Never `classList.contains`.

### Information hierarchy — top to bottom

**1. Header**
`Civic treasury · Prepared with Legend`
DM Sans 500. `--text-primary`.

**2. Publisher context — the identity caveat (load-bearing honesty)**
`Published by [declared name]. Legend verifies the chain data, not the identity behind it.`
DM Sans 300. `--text-tertiary`. One line. This is the first sentence. It sets the
epistemic limit before the reader draws any conclusion. Never omitted, never collapsed.

**3. Declared balance**
`[X] BTC` — large, IBM Plex Mono, `--text-primary`.
`[Y] sats` secondary — one line below. IBM Plex Mono. `--text-secondary`.
`~[fiat] at [date]` — tertiary. DM Sans 300. `--text-tertiary`.

**4. Activity summary — the load-bearing element**
`[N] outflows and [M] inflows recorded on this declared treasury across [period].`
DM Sans 500. `--text-primary`. Prominent, front and centre.

This is the consistency signal — what the cryptographer came to read. A raw address
balance tells her the treasury exists. This sentence tells her whether it operates.
No other explorer surfaces declared treasury cadence as a plain-language front-and-centre claim.

Followed immediately by the monthly aggregate panel:

| Month | Inflows | Outflows | Net |
|---|---|---|---|
| [month] | [amount] BTC | [amount] BTC | [net] BTC |

IBM Plex Mono values throughout. BTC denomination. One row per month, most recent first.
No colour coding. No bar chart. Plain table, equally weighted columns — per the
editorial convention from `legend-articles-list.md` applied here: if it's in the table,
it has the same authority as the rest of the table.

This is the one genuinely new v1 build component on this surface.

**5. Silent Payments integrity**
`Outflows are paid using Silent Payments. Each recipient receives at a unique address
that cannot be linked to them on-chain. You can see that the treasury pays out — how
much, and how often. You cannot see who is paid. This is by design.`
DM Sans 400. `--text-secondary`. Always present on a declared treasury that uses
Silent Payments. Never replaced with "outflows are hidden" — the recipients are
unlinkable, not the outflow amounts.

**6. Verification anchor (live)**
`Checked against the Bitcoin network at block [height] · [date], in your browser.`
IBM Plex Mono 400. `--text-secondary`. Reuses UC-3 string verbatim.

**7. Legend canary summary**
Reuses UC-4 strings and four-node canary state verbatim. No new machinery.
Current: `All four Legend canaries are current — what this means →`
Expired: `One or more Legend canaries have expired — see status →`

**8. Independent verification affordance**
`Verify this yourself →`
Reuses UC-3/UC-4 string and mechanism verbatim.

**9. Per-address table**
Full untruncated addresses. IBM Plex Mono throughout. One row per address: address,
verified balance (sats), last activity. Same chain-of-custody logic as UC-2 print.

**10. Declared-not-exhaustive footer — the load-bearing copy discipline for this surface**
`This view shows the addresses this treasury has chosen to declare. It is not a
complete audit of everything it holds or spends. Legend renders what was declared
and checks it against the chain — nothing more.`
DM Sans 300. `--text-tertiary`. Always visible. Never hidden or collapsed.

This footer is the single most important discipline on the Civic Treasury View. It
prevents a voluntary, partial, self-declared disclosure being read as a complete
institutional audit. It is not fine print; it is an honest statement of what the
view is, in the same register as everything above it.

### Design note — what is deliberately absent

- **No "verified organisation" badge.** Legend verifies chain data; it attests no identity.
  Any badge implying "this is the Canton of X" would be a claim Legend cannot make.
- **No "complete holdings" representation.** The declared set may omit addresses.
  The footer says so. The interface does not imply otherwise.
- **No "audit" or "certified" language anywhere.** This is a self-declared public view,
  rendered honestly. It is not a financial audit. The word "audit" never appears on this surface.

---

## Civic Treasury Mode — generation spec (v2)

**Locked: UC-5 Opus · Multi-14 · 26 Aug 2026**
**Source scenario:** UC-5 — The Florentine District
**Note:** This is the specification for the parked "Public-body transparency module"
from `legend-scope.md` §v2. UC-5 gives that module its shape. The two are the same feature.

### What Civic Treasury Mode is

Legend's **third explicitly-invoked mode** (after Verification Mode and Treasury Watch Mode),
arriving at v2. The first two explicitly-invoked modes produce documents or links for a
known recipient. Civic Treasury Mode produces a public link for an unbounded audience —
the institution declares its treasury to the world, not to a counterparty.

### Entry point

`Publish a treasury view →` — on standard and Stewardship results. Never in Distress Mode.

### Publication modal

**Title:** `Publish a treasury view`

**Body — in order:**
1. What it publishes (a permanent public link; the Civic Treasury View; anyone with the link
   can read the declared treasury and verify it against the chain)
2. How it differs from a verification or watch (no single recipient; public; indefinite;
   the declared set is fixed at publication, though the chain data it renders is always live)
3. What Legend attests (chain data on each open — *not* the identity of the institution;
   the name is shown as provided by the publisher, not verified by Legend)
4. What it does and does not reveal (Silent Payments recipient addresses stay unlinkable;
   every declared address and its full transaction history becomes publicly visible via this link)

**Actions (v2):**
`Copy treasury-view link` — URL fragment mechanism (address set + reference block in `#`
only; never transmitted to any Legend server; same mechanism as UC-3/UC-4).
`Download monthly summary` — plain CSV export of the monthly aggregate panel.

**⚠ Parked — v2 decision item:**
An optional `Download signed civic attestation` would make the Civic Treasury View a
fifth consumer of the canonical FROST-signed artefact (currently four: UC-2/3/4/9).
Field-format check across all four consumers required before this is added. Do not
implement without that check. Logged as v2 decision; not in scope for this patch.

**v1 note:** At v1, the treasury officer shares the declared address directly — she
queries it in Legend, and the resulting Civic Treasury View URL (address set in the
fragment) is what she copies and distributes. No Civic Treasury Mode modal at v1;
that publication flow arrives at v2. The read surface (the Civic Treasury View itself)
ships at v1.

**v2 planning note — Share × Legend integration:**
At v2, Share may provide the *transport* layer (encrypted file transfer to a specific
recipient or group) where Legend provides the *view*. This is the one place Share and
Legend touch without Share becoming a Legend dependency. Requires a dedicated planning
session before v2 build begins on either product's side. Do not bake into v1 or v2
spec without that session.

---

## Federation Settlement View — spec

**Locked: UC-6 Opus · Multi-14 · 26 Aug 2026**
**Source scenario:** UC-6 — The Hanseatic Federation
**Base surface:** Civic Treasury View (UC-5). Variant of that surface — shares the
substrate, differs in load-bearing element and absence note.
**Recipient-View family position:** fourth member (Lender View · Watch Recipient View
· Civic Treasury View · Federation Settlement View).

### What the Federation Settlement View is

The Civic Treasury View applied to a declared merchant federation — a network settling
aggregated Lightning payments in periodic on-chain UTXOs. The load-bearing element
shifts from outflow *pattern over time* (UC-5) to settlement *cadence and amount*
(most recent first). The key absence shifts from SP recipient unlinkability to
inside-federation payment invisibility.

The view is assertive (names the federation, explains the underlying absence plainly)
only when the address is *declared* by the federation. On an undeclared address,
Legend describes cadence and offers the conditional absence note — it does not assert provenance.

### Provenance-agnosticism rule

See `legend-ux-language.md` §1. Legend states on-chain facts and observable patterns.
It never asserts provenance or cause. "Regular settlement cadence detected" is
descriptive and permitted. "This UTXO is a Fedimint federation settlement" is a
provenance assertion and forbidden unless the address is *declared* by the federation.
This rule generalises the batch-result decision ("Activity detected", never "Compromised").

### Three-reads stillness, theme, denomination

All identical to Civic Treasury View. BTC-first (same institutional-reserve rule).

### Information hierarchy — top to bottom

**Declared federation view (address declared by the federation):**

**1. Header**
`Federation settlement · Prepared with Legend`
DM Sans 500. `--text-primary`.

**2. Publisher context**
`Published by [declared name]. Legend verifies the settlements on-chain, not the
federation behind them.`
DM Sans 300. `--text-tertiary`. Identity caveat applied.

**3. Load-bearing element — settlement cadence**
`This address records a regular settlement: most recently [amount] on [date] · block [height].`
DM Sans 500. `--text-primary`. The treasurer's confirmation sentence: amount, date, height.

**4. Settlement history**
Plain list, most recent first:
`[amount] BTC · [date] · block [height]`
IBM Plex Mono. Three to six rows visible; older entries collapsed on click.
Pattern is the information. Single entries are not.

**5. Absence note (assertive — declared address)**
`The individual payments inside each settlement are handled by the federation
off-chain and are not visible here. That is how the federation works — it is not
missing data.`
DM Sans 400. `--text-secondary`.

**6. Verification anchor, canary summary, independent verification, per-address table,
declared-not-exhaustive footer:** reuse Civic Treasury View verbatim.

---

**Undeclared address — settlement-pattern display enhancement on the standard result:**

This is a descriptive enhancement only. It fires when a regular high-value,
low-frequency outbound cadence is detected. It does not name the pattern's cause.

Cadence header: `Regular settlement cadence detected on this address:`
Cadence list: `[amount] BTC · [date] · block [height]` — most recent three rows,
IBM Plex Mono.

Conditional absence note (always present when cadence header is shown):
`Some addresses aggregate many payments into a single periodic on-chain settlement
— a Fedimint federation, for example, or a batching service. If that is the case
here, the payments behind each settlement are not recorded on-chain, by design.
Legend cannot tell you from the chain alone whether this address works that way.`
DM Sans 300. `--text-tertiary`.

Nothing in this enhancement names provenance. The conditional absence note is
accurate, helpful, and non-classificatory.

---

*"Nothing stops this train."*
