# legend-design-spec.md — refueler-legend
> **Version:** 1.7 | **Created:** Multi-4 · 3 Aug 2026 | **Updated:** UC-4 Opus · 25 Aug 2026
> UI/UX design specification for Legend — privacy-first Bitcoin block explorer.
> Load in build sessions. Not by default.
> Covers: page architecture, query flow, result anatomy, credential UX, breach scenario,
> modal inventory, theme behaviour, navigation, funding model, status page, UI modes
> (Distress, Stewardship, Legacy print, Verification, Recipient View, Treasury Watch,
> Watch Recipient View), and session roadmap.

---

## Vision

Legend is professional infrastructure for a world where Bitcoin is a global payments rail,
the world's second currency, and the preferred long-term savings account. In that world,
accountants qualify in chain analysis, insurers price against UTXO provenance, banks check
chain history before allocating loans, and solicitors verify holdings for estates.

Legend is the only explorer built for that professional class — private by design, audited
in public, beautiful enough that a new Bitcoiner is not put off, precise enough that a
family office trustee trusts it with fiduciary responsibility.

The closest comparison is Blockstream's explorer, taken seriously as a design object,
rebuilt with privacy as the foundational constraint, and extended into the professional
market that does not yet have a tool built for it.

**Design register:** Legal document precision. Not developer dashboard density.
The same register Share established. Calm, authoritative, legible.

---

## Funding model

Legend is free at point of use. No account. No payment. No rate limit at launch.
No friction at the distress moment — ever.

**Why this works:**

The free tier is not a loss leader. It is the proof of concept that makes the Enterprise
sale credible. Every Bitcoiner who uses Legend privately and finds it works is a future
referral into a professional context. The pleb and the family office are the same person
at different scales and different moments in their Bitcoin journey.

**Revenue sources, in order of priority:**

1. **Enterprise contracts** — family offices, compliance teams, Bitcoin-backed lenders,
   wallet apps wanting a private Esplora endpoint. Contract rate covers: dedicated nodes,
   Tor API, Silent Payments scanning, compliance reporting, SLA, quarterly security review.
   What they're buying is not the software — the software is free. They're buying the
   institutional wrapper: the contract, the documentation, the support relationship,
   the accountability. A family office trustee does not self-host their Bloomberg terminal.

2. **Refueler merchant network pipeline** — merchants accepting BTC via Refueler POS
   already have sats moving through their business. Their customers earn sat rewards,
   self-custody via paynym. The merchant's accountant, solicitor, and family office
   are the warm Enterprise introduction. Legend is already trusted inside the ecosystem
   before the professional sale happens.

3. **Self-hosting** — open source, AI-assisted setup guide, Umbrel/Start9 packages.
   Distribution channel, not revenue. Every self-hosted instance is a user who trusts
   Legend enough to run it. This strengthens the Enterprise sale rather than undermining it.
   "Don't trust us, read the code" is a stronger statement to a sophisticated buyer
   than "trust our terms of service."

4. **Share as discovery** — no integration required, no gate. Curiosity does the work.
   Share subscribers encounter Legend naturally. Legend drives Share conversions upward
   without forcing the connection.

**Infrastructure cost:** ~€673/month planning estimate (~£566/month) ex-VAT (five dedicated nodes across five jurisdictions and three continents: Hetzner AX52 (DE), Frantech/BuyVM (LU), FlokiNET dedicated (IS), OVHcloud NA (CA), Infomaniak (CH)). Providers selected Legend-8, August 2026; confirmed quotes pending provider replies. One Enterprise client covers years of infrastructure. The free tier is sustainable before the first Enterprise contract closes.

**No voluntary tip jar. No 21 sats prompt. No friction at the distress moment.**

---

## Page architecture

`refueler.io/legend` — not a separate domain. Domain authority consolidates on main domain.

### Static shell (Eleventy)

Renders: Refueler nav, footer, SPA mount point, above-the-fold content.
Handles: initial page load, SEO, below-the-fold product explanation.
Does not handle: query logic, result states, modals, credentials, theme toggle.

### SPA (vanilla JS)

Mounts into container div on the Eleventy page. Owns everything interactive.
No page navigation occurs on query. URL does not change. User never leaves
`refueler.io/legend`.

From the user's perspective there is no seam between the static shell and the SPA.

### Above the fold

Query input is the first element. No hero. No product description above the search bar.
The page opens the way a search engine opens — with the thing you came to do.
Product context lives below the fold for first-time visitors and is irrelevant to
returning users.

### Below the fold (static, Eleventy)

Three short sections:
1. What Legend does differently
2. Who it's for
3. How the free tier works

These are acquisition content, not active-use content.

---

## Query flow

### Input — idle

Single text input, full width of content column.
Placeholder: `Address, transaction ID, or block height`
No submit button visible by default. Enter submits.
On focus only: batch icon appears (right edge of input field) — opens batch input modal.
Below input, tertiary text: `Private query. No logs. No tracking.`

### Input — focused

Batch icon appears. Nothing else changes. No autocomplete. No suggestions.

### Input — submitting

Input locks. Tertiary line beneath input becomes PIR progress:
`Querying node 1 of 4…` through `Querying node 4 of 4…` then `Assembling result.`
Five discrete states, each replacing the last. Text updates only — no spinner, no animation beyond text change. This is honest about what the infrastructure is doing. Exact strings in `legend-ux-language.md` §8.

### Result — funds intact

Calm. Address, balance in sats, UTXO count, last activity timestamp.
No alarming colour. Clean and tabular.
The result lands quietly. That is the correct emotional register for "nothing has moved."

### Result — funds moved

Immediate. Timestamp of last outbound transaction is prominent.
Destination addresses shown where available. Amount moved.
Plain statement: `Last outbound: [amount] on [date] at block [height].`
No red. No alarm iconography. Precision is the signal, not colour.

### Result — not found (standard address)

`No activity found for this address.`
Then: `This address has not appeared on-chain. If you're expecting a payment,
it may not have confirmed yet.`

### Result — not found (Silent Payments address)

Different copy because the reason is different:
`This is a Silent Payments static address (BIP-352). Outputs are derived per sender
and do not appear on-chain as this address. Legend scans every block for derived outputs.`
Then either: `No derived outputs found.` or the derived outputs listed correctly.

### Error state

Single plain sentence describing what happened and what to do.
No technical jargon. No stack traces. Written from the user's side of the screen.

---

## Result anatomy

Top to bottom for a confirmed result:

**Address bar**
Queried address in IBM Plex Mono, truncated with full address on hover.
Copy-to-clipboard icon. No other decoration.

**Status line**
`[N] UTXOs · [balance] sats · Last activity [relative time]`
Single line. IBM Plex Mono for values. DM Sans for labels.

**UTXO table**
Columns: TXID (truncated, Mono), value (sats), age (block height + relative time),
confirmations. All columns same font weight, same colour.
No lighter secondary-text treatment on any column. If it's in the table it has
the same authority as the rest of the table.

**Transaction history**
Collapsible. Inbound and outbound with direction indicator (arrow, not colour).
Amount, timestamp, block. Default collapsed for addresses with >20 transactions.

**UTXO consolidation advisor**
Appears only when address has 5+ UTXOs.
Single line in tertiary text:
`Consolidating these UTXOs will permanently record co-ownership on-chain.`
No score. No recommendation. A true statement. Honest scope — this is a qualitative
flag in v1, not a heuristic or a number.

**Silent Payments section**
Appears only when queried address is a BIP-352 static address.
Lists derived outputs found during block scanning with block height and value.

**Denomination toggle**
Sats / BTC / USD at time of transaction.
Persists across queries within session. Resets on new session. No localStorage.

---

## Breach scenario design

The Coldcard/Coinkite use case. A user with a list of potentially exposed addresses
needs to check all of them privately, at once. This is the most important user journey
in Legend and the one that must have zero friction.

### Entry point

Batch icon in query input (visible on focus). Opens batch input modal.

### Batch input modal

Title: `Check multiple addresses`
Body: textarea.
Placeholder: `Paste addresses, one per line. Up to 50 per batch.`
Note below textarea: `Each address is queried independently. Results below.`
Actions: `Cancel` and `Run batch query`
Run button disabled until at least two addresses present.
Single address routes through standard query flow, not batch.

### Batch in progress

Modal stays open. Textarea replaced by progress:
`Checking address 1 of 47… 2 of 47…`
Determinate. Cancellable.

### Batch results modal

Full-screen modal on desktop, full-screen on mobile.
Table: address (truncated Mono), status, last activity date, sats balance.
Status values: `Intact` or `Activity detected` — only these two.
Not `Compromised`. Not `Swept`. We know what moved, not why.
Sortable by status — `Activity detected` surfaces first by default.
Single action: `Copy results` — plaintext, newline-separated, no metadata.
No CSV download. No filename in browser history. No third-party export.

---

## Modal inventory

### Onboarding modal

Trigger: first visit, no cookie `legend-seen`.
Dismiss: click outside or explicit close. Sets cookie on dismiss.
Title: `How Legend works`
Three lines maximum:
1. Queries are not logged and cannot be linked across sessions.
2. No account required. No session stored.
3. The code is open source. Read it.
GitHub link. Dismiss button: `Start querying`

### Credential status modal

Trigger: credential status icon (top-right of SPA chrome).
Shows: current tier, queries remaining (free) or `Unlimited` (Enterprise).
For Enterprise: PIR stack status, Tor API status.
No top-up flow in v1 — free tier is unlimited at launch.

### Batch input modal

Covered above in breach scenario design.

### Batch results modal

Covered above in breach scenario design.

### Privacy explainer modal

Trigger: `How is this private?` link below each result in the result view.
Not in onboarding — this is for users who query first and ask second.
Three sections: ephemeral sessions, query architecture, open source verification.
Plain prose. No diagrams in v1.
Link to Article 16 (`Why Mempool over Tor isn't enough`) when published.

---

## Credential status icon

Small circle, top-right of SPA container, adjacent to theme toggle.
Filled: credentials healthy (or unlimited).
Outlined: low (future use — not active at v1 launch).
Colour: gold accent (`--accent: #C8A96E`) only. Never orange.
Clicking opens credential status modal.
In v1 this icon simply confirms: `Private queries active. No logs.`

---

## Theme behaviour

Paper is default on arrival from `refueler.io` (cookie already set by main site).
Paper is default on direct arrival (no cookie).
Carbon toggle: single icon button, top-right of SPA chrome, adjacent to credential icon.
Setting: writes `rs-theme` cookie scoped to `.refueler.io`.
Transition: `0.35s` simultaneous on all token properties.
Detection: `dataset.theme === 'carbon'` only. Never `classList.contains`.

---

## Navigation

Legend consumes existing Refueler nav and footer from `refueler-io` unchanged.
No new nav items. No Legend-specific navigation chrome.

**Product label in nav:**
Small label beneath Refueler wordmark: `Legend`
Satoshi 400, tertiary text colour (`--text-tertiary`).
No separate brand identity. A product surface under the Refueler system.

**Legend wordmark in SPA:**
The word `Legend` appears once, above the query input.
Satoshi 700. Scale between h2 and h3.
Not a logo. Not a separate brand. A product name on a product page.

---

## Design principles specific to Legend

**Information density is a choice, not a default.**
Mempool.space is dense because it was built by engineers for engineers.
Legend is precise because it is built for professionals and distressed users alike.
Every element on screen earns its place.

**Calm is the correct register for most results.**
A UTXO that hasn't moved doesn't need drama. Precision is reassurance.
Reserve visual urgency for the rare case where something has actually gone wrong —
and even then, factual language over alarm iconography.

**New entrants must not be put off.**
Bitcoin is already intimidating. A block explorer that looks like a terminal from 1994
tells a new Bitcoiner this is not for them. Legend tells them it is.
Clean type, generous whitespace, plain language in result states, helpful copy
in not-found states. The professional user and the new entrant use the same interface.

**Branding sessions are first-class sessions.**
Every major build phase opens with a design and brand pass.
A tool people find beautiful is a tool people trust.
A tool people trust is a tool institutions eventually pay for.

---

## Design tokens (inherited from Refueler system)

All tokens inherited from REFUELER-BRIDGE.md. No Legend-specific tokens in v1.

**Backgrounds — Paper:** `--bg: #E8E2D8` · `--surface: #DAD4CA` · `--surface-raised: #D0C9BE`
**Backgrounds — Carbon:** `--bg: #1A1917` · `--surface: #242424` · `--surface-raised: #2E3035`
**Text — Paper:** `--fg: #1A1A1A` · `--fg-muted: #5A5550` · `--fg-subtle: #9A9590`
**Text — Carbon:** `--fg: #E8E2D8` · `--fg-muted: #B0AAA2` · `--fg-subtle: #6A6560`
**Text aliases (transition):** `--text-primary: var(--fg)` · `--text-secondary: var(--fg-muted)` · `--text-tertiary: var(--fg-subtle)`
**Borders — both themes:** `--border: rgba(...)` (theme-respective) · `--border-mid` solid · `--inset-rule: var(--border)` (neutral, never gold)
**Accent (restricted consumers only):** `--accent: #C8A96E` · `--accent-hover: #E0C48A`
**Abolished:** `--accent-action` (orange) · `#F7F4EF` · `#1E1F22` · `#F5820A` · `#D4690A` — must not appear in Legend CSS

Canonical values sourced from `REFUELER-BRIDGE.md` and `global.css`. `legend.css` inherits all tokens — no Legend-specific `:root` block.

**Typography:**
- `--heading: 'Satoshi', 'DM Sans', sans-serif` — wordmark, product name, key labels (700)
- `--sans: 'DM Sans', sans-serif` — UI, body copy (300/400/500)
- `--mono: 'IBM Plex Mono', monospace` — addresses, TXIDs, values, table cells (400/500)

**Structural:**
- Border weight: `0.5px`. Card radius: `10px`. Button radius: `8px`. Modal radius: `12px`.
- Theme toggle transition: `0.35s` simultaneous on all token properties.

---

## Status page — `refueler.io/legend/status`

### What the status page is

A public, unauthenticated page showing three things: per-node operational status,
current privacy mode, and per-node canary status. Nothing else. The page exists so
users can understand the current state of the infrastructure without querying the
infrastructure to find out.

It is not a monitoring dashboard. It is not a metrics surface. It does not show
query volumes because no query volumes are recorded. It does not show traffic graphs
because the architecture produces no traffic data to graph.

The established pattern is `share.refueler.io/status`. Legend's status page inherits
that structure and adds the privacy mode indicator and canary section beneath it.

---

### Page structure

Three sections, top to bottom:

1. **Privacy mode** — single line across the top; the most operationally relevant
   signal for a user who has arrived because something felt wrong
2. **Node status** — per-node reachability and chain sync height; the operational layer
3. **Canary status** — per-node canary health; a different kind of claim, visually separate

Privacy mode sits above node status because it is the user's question, not the
operator's. A user arriving at this page wants to know: *is this safe to use right now?*
Privacy mode answers that. Node status explains why. Canary status says something
categorically different and is separated accordingly.

---

### Section 1 — Privacy mode

Single line. DM Sans 400. `--text-primary`.

Five states, exact strings (updated Legend-7B for five-node topology):

| State | String | Colour |
|---|---|---|
| Full splitting | `Full splitting active — four nodes operational.` | `--text-primary` |
| Reduced splitting (one offline) | `Reduced splitting — one node offline.` | `--text-primary` |
| Reduced splitting (two offline) | `Reduced splitting — two nodes offline.` | `--text-primary` |
| Sub-quorum | `Reduced availability — credential issuance paused. Queries available.` | `--text-secondary` |
| Rotation in progress | `Credential rotation in progress — queries available, new credentials paused.` | `--text-tertiary` |

**The "Single node" state is retired.** With five nodes (four full participants plus a warm
standby), a single surviving node is a sub-quorum state, not a distinct UI mode with its own
string. The **Sub-quorum** state replaces it: fewer than three FROST share-holders operational.
Credential issuance halts at sub-quorum; queries on surviving nodes continue; Node D warm
standby promotes immediately if available.

The string changes when node reachability or mint state changes. No animation. No
colour change between full splitting, reduced splitting, or rotation states — all
are operationally normal. Sub-quorum acquires `--text-secondary` (not amber, not red)
because it is a factual reduction requiring attention, not a confirmed failure requiring
alarm. Rotation in progress acquires `--text-tertiary` — it is a background ceremony.

**Note for build:** the status page must surface **per-canary time-to-expiry** continuously
in sub-quorum state. The incident protocol (§3D) requires the operator to know the expiry
deadline in real time. The status page is the mechanism that surfaces this. No new design
token is required; the existing canary expiry countdown serves the purpose. Ensure the
countdown is rendered for all four full-participant nodes when sub-quorum is active, not
suppressed.

Beneath the privacy mode string, in `--text-tertiary`, a one-line description that
does not change with state:

`Your browser selects which nodes handle each stage of your query. This page reflects
current node availability.`

---

### Section 2 — Node status

Inherits `share.refueler.io/status` layout. Five node cards (four full participants A–D,
one cold standby E) in a row (desktop); stacked (mobile). Card border `--border`. Card
radius `10px`. Card background `--surface`.

**Node E** displays with a distinct label: `NODE E  ·  STANDBY`. Chain height shown;
no canary block in the canary section (Node E holds no FROST share and publishes no canary).
Node E's card uses `--text-tertiary` for its status label — it is always `Chain only`, never
`Live` (it does not serve queries). This distinguishes it visually from the four active nodes
without introducing a new token or alarming colour.

**Per-node card contents (active nodes A–D):**

```
Node A · Falkenstein, DE          [Live]
Chain height: 906,412
Last seen: 4 seconds ago
```

**Per-node card contents (Node E, cold standby):**

```
Node E · Standby                  [Chain only]
Chain height: 906,412
Last seen: 4 seconds ago
```

**Label:** Node letter and city only (or `Standby` for Node E). No IP addresses. No
provider names in the user-facing status page (provider names appear in the privacy
explainer modal and contract materials — not on a page a user checks under stress).

**Status badge:** `Live` or `Unreachable`. DM Sans 500.
- `Live` — `--text-primary`. No background. No colour treatment.
- `Unreachable` — `--text-secondary`. No background. No red. Factual, not alarming.

**Chain height:** comma-formatted integer. IBM Plex Mono 400.

**Last seen:** relative time string. `Just now` / `N seconds ago` / `N minutes ago`.
Updates on page refresh only — the status page is not a live dashboard.
No auto-refresh. A user who wants fresh data refreshes the page.

**What the card does not show:**
- Query response time / latency
- Memory or CPU utilisation
- Inbound or outbound traffic
- Number of active connections
- Any metric that could reveal query load patterns

These are absent because they do not exist as logs, and because their absence is
part of the product's honest scope. The status page does not pretend they exist
and choose not to display them.

---

### Section 3 — Canary status

Visually separated from Section 2 by a full-width rule (`--border`, `0.5px`).
A typographic section header above the rule:

`Warrant canaries`
DM Sans 500. `--text-secondary`. Not a heading hierarchy element — this is a label.

The canary section is categorically different from the operational section. Node
reachability is about whether the infrastructure is running. Canary status is about
whether the infrastructure is under legal compulsion. These are different claims.
Sharing visual language between them would imply that a dead canary is an operational
incident. It is not. Separation is the correct design.

**No custom icon at v1.**

**Per-node canary display:**

Each full-participant node's canary is displayed as a single typographic block. Four blocks
(Nodes A, B, C, D), same left-to-right order as the node cards above. Node E has no canary
block — it holds no FROST share and publishes no canary. This is structural, not an omission;
the status page does not display a "missing" canary for Node E.

```
NODE A  ·  CANARY
Signed at block 906,412
Expires in 51 hours
```

- `NODE A  ·  CANARY` — IBM Plex Mono 400. Small caps rendering via
  `font-variant: small-caps`. `--text-secondary`. This is the label line.
  The middot is the same separator used in the result status line — consistent
  with the rest of Legend's typographic system.
- `Signed at block [height]` — IBM Plex Mono 400. `--text-primary`.
  Block height comma-formatted.
- `Expires in [N hours/minutes]` — DM Sans 400. `--text-tertiary`.
  Counts down to the 72-hour expiry window. No seconds display. Hours until
  fewer than 1 hour remain; then minutes.

**Live canary — no colour.** A valid canary is the expected state.
The expected state needs no visual emphasis. Its absence is the signal.

**Expired canary — muted amber only.**

When a canary has expired, the label line becomes:

```
NODE A  ·  CANARY  ·  EXPIRED
```

The word `EXPIRED` appended after a second middot. IBM Plex Mono. Small caps.
Colour: `--canary-expired: #B8860B` (Paper) / `#C9A227` (Carbon).
These are muted amber values — neither orange nor yellow. Amber signals
attention required. It does not signal breach.

The body lines change to:

```
Last signed at block 906,412
Expired [N hours] ago
```

`Expired [N hours] ago` in the amber value. Body lines only — the typography
shifts; nothing else does. No icon. No border change. No background highlight.
No red. No pulse animation. No modal triggered.

**Why amber and not red:**
A dead canary is not confirmed bad news. It may be a node maintenance event,
a publication failure, an operator error. Red communicates certainty about a
negative outcome. Amber communicates: something has changed; pay attention.
The status page states what it knows. The user draws the conclusion.

**FROST integrity note (status page only):**
Beneath the three canary blocks, in `--text-tertiary`, DM Sans 400, one line:

`Each canary is signed by a 3-of-4 threshold signature across separate nodes.
A single compromised node cannot produce a valid canary statement.`

This is the one piece of canary architecture that belongs on the status page
for non-technical users who want the plain version. The full technical detail
is in the privacy explainer modal (Section 5 of this spec) and in Enterprise
contract materials.

---

### Token additions for canary expired state

Two new tokens, scoped to `.theme-paper` and `.theme-carbon`:

```css
/* Paper */
--canary-expired: #B8860B;

/* Carbon */
--canary-expired: #C9A227;
```

No other status-page-specific tokens. All other values inherit from the existing
Refueler token set documented in the Design tokens section of this spec.

---

### What the status page explicitly does not show

This list is part of the spec, not editorial commentary. Build sessions must
not add these without a new spec session:

- Query volumes — no logs exist to count them from
- Traffic graphs — no traffic data is retained
- Response time or latency metrics — these would reveal query load patterns
- Error rate logs — errors are transient; none are stored
- Node CPU, memory, or storage utilisation — operational tooling only, not public
- Historical uptime percentages — not calculated, not stored
- Canary publication history beyond current state — the GitHub mirror is the
  historical record; the status page shows current state only

The status page knows three things: reachability, chain height, canary state.
It shows those three things. It stops.

---

### URL and navigation

`refueler.io/legend/status` — static Eleventy page.
Linked from the Legend page footer only. Not in nav. Not linked from the SPA chrome.
The degraded mode banners (Section 7 of `legend-ux-language.md`) do not link to
it — a banner is a functional notice, not a navigation element.

The status page is for users who know to look for it, not a surface pushed
at every user. Enterprise clients receive the URL in their contract onboarding.
Free-tier users find it in the footer.

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
The first element Arthur reads. Prominent. Calm. Exact string in `legend-ux-language.md` §8:
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
`legend-ux-language.md` §4): denomination follows the reader's relationship to the
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

## Treasury Watch Mode — generation spec

**Locked: UC-4 Opus · 25 Aug 2026**
**Source scenario:** UC-4 — The Council and the Whale

### What Treasury Watch Mode is

Legend's second explicitly-invoked mode, after Verification Mode. It produces a
*watch* — a live, re-checkable link — rather than a *verification* — a
point-in-time snapshot. Distress and Stewardship are inferred and never named;
Treasury Watch is a deliberate act. "Treasury" denotes the institutional register
it is primarily built for; the underlying capability is general.

### Entry point

`Create a watch →` — appended to the result view in standard and Stewardship
contexts, alongside (not replacing) `Create a verification →`. Never appears in
Distress Mode (trigger conditions absent: mobile, first visit, single address).
Never on error or not-found states. The two CTAs must be visibly distinct: the
snapshot/live distinction is the whole point of the mode and must not blur at the
button.

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

*"Nothing stops this train."*
