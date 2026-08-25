# legend-copy-index.md — refueler-legend
> **Version:** 1.0 | **Created:** Multi-[n] restructure · 25 Aug 2026
> **Extracted from:** `legend-ux-language.md` v1.3 (Sections 6–8)
> Honest-scope statements, degraded mode notices, and the locked copy index.
> Load in any session producing new locked strings or patching §8.
> The locked copy index is the string authority. If a string is not in §8, it is not finalised.
> Companion: `legend-ux-language.md` (voice/register), `legend-design-spec.md`, `legend-ui-modes.md`

---

## Section 6 — Honest-scope statements

These are the canonical user-facing versions of each architecture claim. They are shorter than the Enterprise contract versions. They are equally honest. Build sessions pull the exact strings from this section for the privacy explainer modal and the below-the-fold explainer.

### Ephemeral sessions

`No session is created when you query Legend. No cookie identifies you across queries. No server-side token links your first search to your second. Each query is structurally isolated from every other.`

### Role-split querying (v1 — precise language)

`Your query is split across two nodes. The first node sees a prefix of your query — enough to return a candidate set, not enough to identify your address. The second node sees opaque identifiers, not the original query. Neither node holds the full question. This is collusion-resistant query splitting: reconstructing what you asked requires both nodes to cooperate and to have retained data the architecture does not produce.`

`This is the v1 privacy architecture. It is not cryptographic single-server PIR. That arrives in v2 and does not change how you use Legend.`

### Cashu credentials

`Your query credential is a blind signature issued by a Cashu mint. The mint signs the credential without seeing the plaintext. The credential you present at query time cannot be linked to the issuance event. The mint knows a batch of credentials was requested. It cannot determine which credential authorised which query, or which queries came from the same session.`

### No server-side query logs

`There are no query logs on any Legend node. This is not a retention policy. A retention policy is a promise; promises can be broken. The architecture is the constraint: ephemeral sessions produce no persistent query record. There is nothing to log, nothing to subpoena, nothing to leak.`

### IP privacy

`Your IP address is visible to the Legend nodes you contact over standard HTTPS. The query architecture hides what you asked. It does not hide who is asking. If you need both, route through Tor. Enterprise clients use a Tor hidden service endpoint included in their contract.`

`Tor hides your IP from Legend. Legend hides your query from itself. You need both to be fully private. We give you the second. You supply the first if you need it.`

### Verification and disclosure

`A verification is the one moment Legend helps you disclose, not conceal. Legend keeps your query private from Legend. It cannot make private a balance you have chosen to show someone else.`

`A v1 shareable link proves these addresses held this balance, unmoved for this window, as of this block. It does not prove who controls them. It is not a lien. The funds can move at any time. Re-check before you rely on it.`

`At v1 and v2, the borrower reveals the address to the recipient. At v3, a ZK balance proof allows a holder to prove control of a threshold balance, unmoved for a window, without revealing any address to the lender.`

### The watch is not an alarm (locked UC-4 Opus)

`This is live only when you open it. Legend does not watch this address on your behalf and sends no alerts — no alert is not evidence of no movement. Legend cannot hold or freeze these funds. Check again on your own schedule.`

This appears as the honest-scope footer on the Watch Recipient View. It is the single most important copy discipline on that surface — what stops a pull link being read as a push alarm. It is not fine print; it is an honest statement of what the watch is, in the same register as everything above it.

The inverted failure mode this guards against: a reader treating a link they must open as an alarm that reaches them. Honest "future detection" (v3) is reactive (post-confirmation), delivered only through a blind unlinkable channel, and never preventive.

### UK operator caveat (user-facing register)

`All five Legend nodes are operated by one person in London. The UK Investigatory Powers Act 2016 permits compelled disclosure and non-disclosure orders served on the operator personally, regardless of where the hardware is located.`

`The geographic distribution of nodes across Germany, Luxembourg, Iceland, Canada, and Switzerland protects against legal orders served on the hosting providers in those jurisdictions. It does not protect against an order served on the operator under UK law.`

`This is stated here because a product that overstates its legal protections is weaker than one that understates them. The architecture does what it says it does. The legal boundary is this.`

---

## Section 7 — Degraded mode notices

These strings appear in the browser SPA. They are never suppressed. A degraded privacy claim is always surfaced to the user. Exact strings.

### N-1 — Two nodes operational (reduced splitting)

Banner, top of SPA chrome, non-dismissible until full splitting is restored:

`Operating on two nodes. Query splitting is active but reduced — both queries route through the same node pair. One node offline. Full splitting restores automatically when the third node returns.`

Privacy mode indicator (credential status modal): `Reduced splitting — one node offline`

### N-2 — One node operational (no splitting)

Banner, top of SPA chrome, non-dismissible:

`Operating on a single node. Query splitting is unavailable — this node can see your full query. If you need full privacy, consider routing through Tor while the other nodes are offline.`

Privacy mode indicator (credential status modal): `Single node — splitting unavailable`

**Copy notes:**

Neither notice uses "secure" or "insecure". Neither catastrophises. The N-2 notice offers a concrete mitigation (Tor). Both notices state when the degraded mode ends (automatically when the node returns; or, for N-2, implicitly — the user checks the status page).

The status page URL is not embedded in the banner. If the user wants the status page, they find it in the footer. The banner is a functional notice, not a navigation element.

---

## Section 8 — Locked copy index

Every finalised string in this document with its location. Build sessions pull from this table. If a string is not here, it is not finalised.

**Denomination rule (extended UC-3 Opus · 23 Aug 2026):**
Denomination hierarchy is contextual. Stewardship Mode: sats-first (returning owner).
Distress Mode: GBP-first (frightened newcomer). Recipient / Verification context:
GBP-first (professional counterparty). Watch Recipient View: GBP-first (institutional
counterparty). Unifying principle: denomination follows the reader's relationship to
the holding, not a user setting. All are correct in their context.

| String | Surface | Element | Section |
|---|---|---|---|
| `Bitcoin, privately.` | Landing page | Headline (above fold) | 2 |
| `The block explorer that cannot see what you searched.` | Landing page | Subhead (above fold) | 2 |
| `Address, transaction ID, or block height` | Query input | Placeholder | 2, 3 |
| `Private query. No logs. No tracking.` | Query input | Tertiary line (idle and focused) | 2, 3 |
| `How your query stays private` | Landing page | Section 1 title (below fold) | 2 |
| `Built for the moment it matters` | Landing page | Section 2 title (below fold) | 2 |
| `Free. No account. No catch.` | Landing page | Section 3 title (below fold) | 2 |
| `Querying node 1 of 4…` | Query input | Progress state 1 | 3 |
| `Querying node 2 of 4…` | Query input | Progress state 2 | 3 |
| `Querying node 3 of 4…` | Query input | Progress state 3 | 3 |
| `Querying node 4 of 4…` | Query input | Progress state 4 | 3 |
| `Assembling result.` | Query input | Progress state 5 | 3 |
| `No activity found for this address.` | Result | Not found — primary | 3 |
| `This address has not appeared on-chain. If you're expecting a payment, it may not have confirmed yet.` | Result | Not found — secondary | 3 |
| `This is a Silent Payments static address (BIP-352).` | Result | SP not found — primary | 3 |
| `Outputs derived from this address do not appear on-chain as this address. Legend scans every block for derived outputs.` | Result | SP not found — secondary | 3 |
| `No derived outputs found yet.` | Result | SP not found — tertiary | 3 |
| `Something went wrong retrieving that result. Try again, or check that the address or transaction ID is correct.` | Result | Error — generic | 3 |
| `The query took too long to complete. The nodes may be under load. Try again in a moment.` | Result | Error — timeout | 3 |
| `That doesn't look like a Bitcoin address, transaction ID, or block height.` | Result | Error — invalid input | 3 |
| `Consolidating these UTXOs will permanently record co-ownership on-chain.` | Result | UTXO consolidation advisor | 4 |
| `sats` · `BTC` · `USD at time of transaction` · `GBP at time of transaction` | Result | Denomination toggle labels | 4 |
| `Derived outputs` | Result | Silent Payments section header | 4 |
| `Payments to this Silent Payments address produce unique derived outputs per sender. Legend scans every block to find them.` | Result | Silent Payments section body | 4 |
| `No derived outputs found for this address. Scanning is complete to the current block height.` | Result | Silent Payments — no outputs | 4 |
| `Intact` | Batch results | Status value | 4 |
| `Activity detected` | Batch results | Status value | 4 |
| `How Legend works` | Onboarding modal | Title | 5 |
| `Queries are split across separate nodes. Neither node receives your complete question.` | Onboarding modal | Body line 1 | 5 |
| `No session is created. No query is stored. Nothing links one search to the next.` | Onboarding modal | Body line 2 | 5 |
| `The code is open source. If you'd rather not take our word for it, read it.` | Onboarding modal | Body line 3 | 5 |
| `View source on GitHub` | Onboarding modal | Link | 5 |
| `Start querying` | Onboarding modal | Dismiss CTA | 5 |
| `Private queries active` | Credential status modal | Free tier title | 5 |
| `You're querying privately. No account required. No query limit.` | Credential status modal | Free tier body | 5 |
| `Architecture: collusion-resistant query splitting with blind credential unlinkability.` | Credential status modal | Footer line (both tiers) | 5 |
| `Check multiple addresses` | Batch input modal | Title | 5 |
| `Paste addresses, one per line. Up to 50 per batch.` | Batch input modal | Textarea placeholder | 5 |
| `Each address is queried independently through separate node pairs. Results are assembled in your browser.` | Batch input modal | Note below textarea | 5 |
| `Run batch query` | Batch input modal | Submit button | 5 |
| `Stop` | Batch input modal | Cancel during progress | 5 |
| `Batch results` | Batch results modal | Title | 5 |
| `Copy results` | Batch results modal | Action | 5 |
| `Results are assembled in your browser. Nothing in this list was sent to a single server.` | Batch results modal | Footer line | 5 |
| `How is this private?` | Result | Privacy explainer trigger link | 5 |
| `Why this query is private` | Privacy explainer modal | Title | 5 |
| `No session, no log` | Privacy explainer modal | Section 1 title | 5 |
| `Split across two nodes` | Privacy explainer modal | Section 2 title | 5 |
| `Your IP and Tor` | Privacy explainer modal | Section 3 title | 5 |
| `Why Mempool over Tor isn't enough →` | Privacy explainer modal | Article 16 link (hidden until published) | 5 |
| `Close` | Privacy explainer modal | Dismiss CTA | 5 |
| `Operating on two nodes. Query splitting is active but reduced — both queries route through the same node pair. One node offline. Full splitting restores automatically when the third node returns.` | SPA chrome | N-1 degraded mode banner | 7 |
| `Reduced splitting — one node offline` | Credential status modal | N-1 privacy mode indicator | 7 |
| `Operating on a single node. Query splitting is unavailable — this node can see your full query. If you need full privacy, consider routing through Tor while the other nodes are offline.` | SPA chrome | N-2 degraded mode banner | 7 |
| `Single node — splitting unavailable` | Credential status modal | N-2 privacy mode indicator | 7 |
| `Held since block 840,000. No movement detected in 5 years.` | Stewardship Mode result | Stillness affirmation (primary) | 1 |
| `Nothing has happened here since [date]. For a long-term holding, that is what you want to see.` | Stewardship Mode result | Stillness affirmation (secondary) | 1 |
| `This summary reflects the chain as of block [n], checked [date]. Anyone can verify it against the Bitcoin network independently.` | Stewardship Mode result | Verification anchor (expanded) | 1 |
| `Prepared for your records. Legend keeps no copy.` | Legacy print layout | Print footer | 1 |
| `This check travelled over your normal internet connection.` | Stewardship Mode result | Tor notice (screen only; omitted from print) | 1 |
| `Create a verification →` | Result view (standard + Stewardship) | CTA — Verification Mode entry | 1 |
| `Create a verification` | Verification modal | Title | 1 |
| `This produces a shareable proof of what these addresses hold and how long they have held it. Your recipient verifies it against the Bitcoin network — they do not have to trust Legend, or you.` | Verification modal | Framing body | 1 |
| `Sharing this reveals the address and its balance to whoever you send it to. Legend cannot recall a verification once it is shared.` | Verification modal | Disclosure warning | 1 |
| `Sign a message from this address in your wallet to prove you control it. Legend checks the signature — it never sees your keys.` | Verification modal | Proof-of-control prompt (v2) | 1 |
| `Copy shareable link` | Verification modal | Action (v1) | 1 |
| `Download signed verification` | Verification modal | Action (v2) | 1 |
| `Verified holdings · Prepared with Legend` | Recipient View | Header | 1 |
| `Shared with you by the holder. Verified against the Bitcoin network in your browser.` | Recipient View | Context line | 1 |
| `No outbound movement across these [k] addresses in [N] days · [M] blocks.` | Recipient View | Holding-period statement (plural) | 1 |
| `No outbound movement from this address in [N] days · [M] blocks.` | Recipient View | Holding-period statement (singular) | 1 |
| `Verified against the Bitcoin network at block [height] · [date].` | Recipient View | Verification anchor (live) | 1 |
| `The holder generated this verification at block [height] · [date].` | Recipient View | Borrower reference (freshness delta) | 1 |
| `Control of this address was demonstrated by signature · [date].` | Recipient View | Proof-of-control present (v2) | 1 |
| `This verification confirms the holdings. It does not prove who controls them. Ask the holder for a signed message if you need that.` | Recipient View | Proof-of-control absent | 1 |
| `Verify this yourself →` | Recipient View | Independent-verification affordance | 1 |
| `This confirms what these addresses held when checked. It is not a lien or an escrow. The holder can move these funds at any time. Re-check before you rely on it.` | Recipient View | Honest-scope footer | 1 |
| `A verification is the one moment Legend helps you disclose, not conceal. Legend keeps your query private from Legend. It cannot make private a balance you have chosen to show someone else.` | §6 honest-scope | Verification/disclosure register anchor | 6 |
| `Create a watch →` | Result view | Treasury Watch entry CTA | UC-4 |
| `Create a watch` | Watch creation modal | Title | UC-4 |
| `Copy watch link` | Watch creation modal | Action (v1) | UC-4 |
| `Download signed watch attestation` | Watch creation modal | Action (v2) | UC-4 |
| `Collateral watch · Prepared with Legend` | Watch Recipient View | Header | UC-4 |
| `Shared with you by the holder. Verified live against the Bitcoin network in your browser each time you open this.` | Watch Recipient View | Recipient context | UC-4 |
| `No outbound movement recorded on this collateral as of block [height] · [date].` | Watch Recipient View | Movement status — intact (plural) | UC-4 |
| `No outbound movement from this address as of block [height] · [date].` | Watch Recipient View | Movement status — intact (singular) | UC-4 |
| `Movement recorded. [amount] left on [date] · block [height]. See below.` | Watch Recipient View | Movement status — moved | UC-4 |
| `Checked against the Bitcoin network at block [height] · [date], in your browser.` | Watch Recipient View | Verification anchor (live) | UC-4 |
| `The holder created this watch at block [height] · [date].` | Watch Recipient View | Watch reference | UC-4 |
| `Control of this collateral was demonstrated by signature when the watch was created · [date]. A signature proves control only at the moment of signing.` | Watch Recipient View | Proof of control (v2, present) | UC-4 |
| `This confirms the collateral exists and is unmoved. It does not prove who controls it. Ask the holder for a signed message if you need that.` | Watch Recipient View | Proof of control (absent) | UC-4 |
| `All four Legend canaries are current — what this means →` | Watch Recipient View | Canary summary (current) | UC-4 |
| `One or more Legend canaries have expired — see status →` | Watch Recipient View | Canary summary (expired) | UC-4 |
| `This is live only when you open it. Legend does not watch this address on your behalf and sends no alerts — no alert is not evidence of no movement. Legend cannot hold or freeze these funds. Check again on your own schedule.` | Watch Recipient View | Honest-scope footer | UC-4 |

---

*"Nothing stops this train."*
