# legend-threat-model.md — refueler-legend

> **Version:** 1.0 | **Produced:** Adversarial-1 · 11 Aug 2026
> Targeted pre-build adversarial threat review of Legend **v1** transport and query architecture.
> Scope: the v1 role-split query model, the Cashu credential layer, the five-node topology,
> the warrant canary, and the FROST root of trust — against three attacker profiles.
> **Not** CryptoRoadmap-2. No Double Ratchet / ML-KEM / Spiral PIR / ZK depth here.
> Companion: `legend-scope.md`, `legend-node-plan.md`, `legend-incident-protocol.md`.

---

## 1. Executive summary

**What v1 protects well.** Against a pure analyst with no legal reach into the operator — the Chainalysis-tier firm that lives on on-chain graph correlation — v1 gives away almost nothing. Legend does not feed the chain-analysis pipeline: no query logs exist, the jurisdictional design specifically denies the easy US-nexus subpoena lever, and the nodes are operator-run bare metal that an outside analyst cannot simply join. The Cashu layer is sound in the way that matters most: because blinding factors live only in the browser, even full reconstruction of the mint key (3-of-4 node compromise) does **not** retroactively deanonymise past blind-signed credentials — rejecting NUT-09 and NUT-13 bought real forward secrecy. TLS 1.3 forward secrecy similarly defeats "collect now, decrypt later" against stored traffic. Where the design is strong, it is strong for structural reasons, not policy promises.

**What it structurally cannot protect.** The role-split model protects against exactly one adversary: an honest-but-curious *single* node. It does **not** protect against a network-position adversary or a two-node collusion, and the reason is singular and load-bearing — on the free tier the client IP is visible to both the stage-1 and stage-2 node, and IP+timestamp is a trivial join key that any TLS-terminating server sees by default, log policy notwithstanding. Done correctly, the honest privacy floor of v1 is *k-anonymity within a prefix bucket, keyed to your IP* — meaningfully weaker than the PIR it is a placeholder for, and entirely dependent on two things the current spec underspecifies: the bucket size *k*, and whether stage-2 is genuinely oblivious. If stage-2 fetches only the target row rather than the whole candidate set, the data-role node — which holds a **full** index — reverses the "opaque" identifier straight back to the address and the entire split collapses. That obliviousness check is the highest-priority item in this document.

**What is foreclosed.** Very little, and that is the good news for v2. Browser-direct role-split does not foreclose client-side transport hardening — each direct connection can independently be wrapped in Tor or OHTTP without a gateway. Full-index-per-node actively *enables* Spiral PIR as a v2 upgrade (single-server PIR needs the whole database on the server), served as an additive API alongside v1 rather than a mutation of it. HTTPS-now does not foreclose OHTTP-later; OHTTP (RFC 9458) is in fact the natural v2 answer to the free-tier IP-join problem and fits the no-gateway model cleanly. The two real forward-looking constraints are soft: rotating monthly mint instances foreclose any future *stateful* metered credential tier (irrelevant while the free tier is unlimited), and hardcoding Nostr as the sole canary path would foreclose channel diversity (fixable by abstracting publication now). No locked v1 decision requires architectural replacement to reach the planned v2.

---

## 2. Attacker findings

For each profile: clean design (perfect implementation), one node compromised, operator compulsion (UK, IPA), two of four FROST participants compromised.

### 2.1 Attacker 1 — Commercial surveillance firm (Chainalysis tier)

**Clean design.** Effectively nothing beyond the public chain. The firm's power is on-chain graph correlation; Legend does not contribute to it. The firm cannot join as a node (closed, operator-run bare metal), cannot compel non-US/UK nodes, and the design denies the US-nexus subpoena that anchors clusters to KYC — there is no US node and no US entity. Using Legend *as a client* reveals nothing about other users. **Net: Legend does not increase this attacker's power unless it acquires a network vantage point or a legal lever on a node/operator.**

**One node compromised.** For queries routed to that node, the firm learns the prefix bucket + client IP + timing. With node selection uniform over four nodes, any single node participates in roughly half of all queries over time (a node is one of the two picks in 3 of 6 possible pairs). So a single compromised node yields bucket-level visibility on ~50% of a user's queries, joined to that user's IP. Not the address — provided obliviousness and *k* > 1 hold — but the IP handle plus repeated buckets is a strong behavioural fingerprint.

**Operator compulsion.** Only reachable via UK LE cooperation (they are the analyst, not the enforcer). If achieved, see 2.2 — the operator lever is a nation-state lever, and its most damaging form is prospective, not retrospective.

**Two of four FROST shares compromised.** Two shares is below the mint/canary threshold: no key reconstruction, no forged credentials, no forged canary. What two *nodes* (as distinct from two bare shares) yield is the collusion case: joined on IP+timestamp, two colluding nodes learn (IP, bucket) for every query where both were selected. With uniform selection over 6 pairs, exactly those two nodes are the pair for ~1/6 of queries (full two-node view), and for most other queries the attacker holds one of the two. Over an N-query session tied together by IP, ~N/6 queries are fully within the colluding pair. Still bucket-level, not address-level, *if* stage-2 is oblivious. If it is not, ~1/6 of queries are deanonymised outright and the rest half-exposed.

### 2.2 Attacker 2 — UK LE / intelligence (GCHQ / NCA tier)

**Clean design.** This is the profile v1 serves worst, and it is worth saying plainly: the free tier — the distress-moment tier — is the tier most exposed to the nation-state observer. Bulk interception at a UK IXP gives Attacker 2, for any UK free-tier user, the stage-1 and stage-2 connections in the clear at the metadata layer: source IP, destination node IP, SNI, timing, payload size. They cannot read the plaintext query (TLS 1.3, forward-secret — later key compromise does not retro-decrypt), but IP + which-nodes + timing aligns the two halves trivially, and payload size distinguishes a single lookup from a Silent Payments full-range scan (larger response, minutes-long connection). Result: (user IP, bucket-level interest, behavioural class, timestamp). The **Enterprise Tor tier defeats this IP linkage** at the transport layer; the free tier does not.

**One node compromised.** Via MLAT (Germany fast, Iceland slower, Switzerland slowest) or EI warrant. Same bucket+IP+timing leak as 2.1, but combined with bulk interception the state actor can align a compromised node's view with its IXP view to confirm identity and behaviour with high confidence.

**Operator compulsion — the central finding.** Under IPA s.49 (compelled decryption/key disclosure) and Part 3 (technical capability notice), the operator controls all nodes remotely and can be compelled to **modify node software to log prospectively** — turning the no-log nodes into logging nodes going forward. "No server-side query logs" is a *forward* architectural property, not an immutable one: it holds until a Part-3 notice compels its silent removal. It is not retrospective (past queries are genuinely unreconstructable), but from the notice onward the architecture's core guarantee can be quietly inverted. This is the worst case, and it compounds with the canary finding below.

**Two of four FROST shares compromised.** Below threshold — no forged canary, no key. But note the asymmetry the state actor exploits: it does not need to *forge* the canary, only to prevent the honest one from *lapsing* as a signal (see §2.4 / §3.4). Compelling one operator while three participants keep signing is cheaper and quieter than compromising a threshold.

### 2.3 Attacker 3 — Passive network observer (ISP / traffic analysis)

**Clean design.** Role-split provides this attacker close to nothing, because it was never a defence against an on-path observer. The ISP sees the same-IP stage-1→stage-2 pair, aligns them by IP+timing (the two-hop gap *is* the stage-1 compute duration — itself a fingerprint), sees which nodes, and reads payload sizes: single lookup vs SP scan vs 47-address batch are all size-distinguishable over TLS. Under IPA Part 4, this is retained for 12 months and available on a low-threshold production order. **The honest characterisation: role-split defends against a curious single node, not against anyone who can see both connections. Conflating the two would be the one dishonesty to avoid in all Legend copy.**

**One node / operator / two shares.** Not this attacker's mode — it is defined by position, not access. Its findings do not deepen with node access; they deepen only with *more* position (more IXPs, corporate network logging).

---

## 3. Attack surface findings

Findings only. Architecture is described in the prompt.

### 3.1 Browser direct-split queries

- **The obliviousness crux.** Everything rests on whether stage-2 requests the *entire* candidate set for the bucket (browser filters locally, node stays oblivious) or only the target row. Because every node holds a **full** index, a target-row request lets the data node map the "opaque" identifier back to the exact scriptPubKey. If stage-2 is target-row, the split is cosmetic. **This must be verified and enforced as oblivious-fetch-all-candidates before build.**
- **The prefix is the leak, and *k* is unspecified.** Stage-1 learns the prefix bucket. Bucket size *k* is the anonymity set. Too specific → *k* approaches 1 and stage-1 nearly identifies the address; too broad → stage-2 candidate download balloons. There is a fixed privacy/bandwidth tension and no locked minimum-*k*. Pick and document a floor for *k*.
- **IP is the join key.** Both nodes see the same client IP seconds apart. Collusion resistance is therefore *not* "they'd have to retain data they aren't designed to retain" — IP+timestamp is default TCP/TLS-layer data, capturable by pcap beneath the application's no-log layer. State the collusion-resistance claim in terms of *k*-anonymity-behind-IP, not query indivisibility.
- **Node-selection JS is fully known to the attacker.** It is shipped browser code. Selection must be uniform-random per query (not deterministic on query content, or the pair is predictable from the query). Uniform selection means security degrades sharply with the compromised-node fraction: 2 of 4 compromised → ~1/6 of queries fully within the pair.
- **SP full-range scan self-identifies.** Larger payload, minutes-long connection. High-signal behaviour (implies the user controls/watches an SP address), fingerprintable by size and duration even over TLS.

### 3.2 Cashu credential issuance

- **Batch size leaks, and it leads the query.** A 47-address batch mint (NUT-29) tells the mint "47 addresses about to be queried, now, from this IP" — before the queries happen. For the breach scenario specifically, batch size is itself the signal that this user was affected by a breach. Blind signatures hide *which*, never *how many / when / from where*.
- **P2PK binding is a deliberate linkability trade.** NUT-11 binds Enterprise credentials to a public key — a persistent identifier that links an Enterprise client's queries within a rotation window. This is correct for replay protection but means unlinkability is a free-tier property only, and even there only against a mint that ignores IP/timing. Say so.
- **The mint runs on the same FROST nodes.** So a high-volume Enterprise client's issuance pattern reveals query volume to the very nodes serving queries. Within-month volume is visible to the operator infrastructure.
- **Mint-key compromise does not deanonymise the past (strength).** Blinding factors are client-held; reconstructing the mint key forges *future* credentials but cannot unblind past issuance. Forward secrecy from the NUT-09/NUT-13 rejections holds.

### 3.3 Node endpoints

- **Public unauthenticated status endpoint.** Reveals chain height, canary status, reachability per node. Lets anyone map topology (already semi-public via documented providers and Bitcoin P2P) and, more relevantly, poll per-node canary state in real time — potentially detecting a single-node event faster than the Nostr channel, or observing a compelled node serving a stale-but-valid cached canary. Low-to-moderate; topology is not secret, but real-time per-node canary polling deserves a deliberate decision.
- **Stage-1 vs stage-2 distinguishability.** If the two roles use distinct endpoints/paths or distinct response-size profiles, a node knows its role per request and an on-path observer aligns the halves more cheaply. Prefer a uniform endpoint with role inferred from payload, and size bucketing.
- **Bitcoin Knots P2P exposure.** Each node broadcasts its IP on the Bitcoin P2P network. If the P2P IP equals the query-API IP, anyone can enumerate Legend's nodes by intersecting the two. This is node/operator exposure, not user privacy — but it hands topology mapping to any observer for free. Separate the P2P egress from the query-API address, or run P2P over Tor.

### 3.4 Canary publication (Nostr)

- **3-of-4 routes around a single compelled operator — the canary does not signal single-operator compulsion.** To *continue* a valid canary, any 3 of 4 sign every 72h. If the London operator is compelled and silenced, the other three (LU, IS, CA) still reach threshold and keep publishing "all fine." The canary therefore lapses only on ≥2-participant failure or genuine sub-quorum — **not** on the exact scenario the operator most fears (a personal IPA notice). The belief that "FROST resolves the Boltz ambiguity" is only half true: FROST stops a *seized node forging* a canary; it does **not** make a single compelled operator's silence visible. This is a design-honesty gap, not a bug, and it must be documented as what the canary does and does not signal.
- **Sub-quorum must be confirmed unsignable.** The open "can 2 remaining sign under 3-of-4" question has only one acceptable answer: **no.** If 2 could produce a valid signature, the threshold is not 3 and the sub-quorum expiry gives a false "all fine." Confirm on real hardware that 2-of-4 cannot sign before any node goes live. (This is the correct resolution of the flagged open question — it is not genuinely open; it is a required invariant.)
- **Three relays is a thin off-node guarantee.** "At least one off-node channel" is met by damus / nostr.band / nostr.wine, but a relay can silently drop an event, and if the three share a compulsion jurisdiction or single MLAT reach, the geographic diversity of the relays does not match the diversity of the nodes. *Verify each relay's hosting jurisdiction* (I cannot confirm these without web access — treat as an open item), and add relays the operator does not control across distinct jurisdictions, plus a second channel *type* (Bitcoin-anchored OpenTimestamps, or signed git mirrors across ≥2 independent hosts) so suppression requires compelling multiple unlike systems.
- **Publication schedule is a double-edged fingerprint.** A predictable cadence makes deviation detectable (the point) but also tells an adversary exactly when the signing ceremony must run — the window when FROST participants coordinate. Minor, but keep the ceremony window unpredictable relative to the publication cadence.

### 3.5 FROST ceremony

- **DKG coordination channel is the soft spot, not the shares.** One compromised participant during DKG yields one share + metadata (participant identities, coordination channel, timing) — not the key. The channel must be authenticated and encrypted with out-of-band verification of participant identity; a MITM on an unauthenticated coordination channel is the realistic DKG attack.
- **Shares are exposed by *live* compromise, not just disk theft.** "Encrypted at rest" protects a stolen disk; a running node must hold the share usable in memory to sign, so a live node compromise yields the share. Three simultaneous live compromises across three jurisdictions reconstruct the key — hard, but a defined nation-state bar (DE fast + CA bilateral + one more), not an impossibility.
- **Crate audit status is unverified here.** `frost-secp256k1` (Zcash Foundation lineage; FROST is now RFC 9591) should be pinned to a specific reviewed version with its audit status confirmed. I cannot verify current audit state without web access — treat as a required check, not a settled fact.

---

## 4. Architecture gaps (silent-but-shouldn't-be; claims needing qualification)

1. **"No logs" is unqualified about the network layer.** True at the application layer; silent on the fact that IP+timing is visible and pcap-capturable beneath it, and that a Part-3 notice can compel prospective logging. The copy rule already demands the structural qualifier — extend it: *no logs* means *no application-layer query logs*, and even that is a forward property under UK compulsion.
2. **"Collusion-resistant query splitting" overstates the collusion bar.** The real bar for two colluding nodes is retaining IP+timestamp — default data, not exotic. Restate as *k*-anonymity-behind-IP.
3. **Timing correlation across sequential same-IP queries is unaddressed.** Nothing in v1 decorrelates a user's query stream; the IP ties it together and the inter-query timing is itself structure. Silent, should not be.
4. **The distress-tier / exposure inversion is unstated.** The users most likely to be targeted (breach, distress) are on the free tier with the least transport protection. This should be an explicit, documented design tension, with the in-product Tor recommendation elevated accordingly (see §6).
5. **The canary's signalling semantics are unstated** (per §3.4). Users and Enterprise materials should not be left to infer that a single-operator compulsion would lapse the canary; it will not.
6. **Minimum *k* and stage-2 obliviousness are undocumented invariants.** Both are correctness-critical to the privacy claim and neither is currently a locked number/behaviour.

---

## 5. Foreclosed v2 transport options

- **Browser-direct role-split (no gateway): not foreclosing.** Each direct connection can independently be wrapped in Tor or OHTTP with no gateway. What it forecloses is *server-side cross-user query mixing* (a gateway could mix; browser-direct cannot) — acceptable, since a gateway was rejected precisely for being a single surveillance point.
- **Full index per node: enabling, not foreclosing.** Single-server PIR (Spiral) requires the whole database server-side — full-index-per-node is a prerequisite, not an obstacle. v2 Spiral is an *additive* API served alongside v1 role-split on the same nodes; v1 clients simply won't get PIR without a client update. No breaking change to the node data model.
- **HTTPS free tier: not foreclosing OHTTP.** RFC 9458 Oblivious HTTP fits the no-gateway model cleanly — an oblivious relay sees IP-not-content while the node sees content-not-IP; the relay is not a plaintext-seeing gateway. **OHTTP is the natural v2 free-tier answer to the IP-join finding** and nothing in v1 blocks it. This is the strongest single forward recommendation in this document.
- **Rotating monthly mint instances: soft foreclosure.** Forecloses any future *stateful* metered credential tier (cross-month balances, stateful anonymous rate-limiting) — irrelevant while the free tier is unlimited and unmetered. Flag, low urgency.
- **Nostr as canary channel: soft foreclosure only if hardcoded.** Additive by nature; the risk is tooling that treats Nostr as the sole verification path. Abstract publication/verification behind a channel interface now so Bitcoin-anchored timestamps and git mirrors can be added without touching verification tooling.

---

## 6. Hardening candidates (ranked by privacy impact ÷ cost)

Effort is build-effort units (AI-assisted), not developer-months.

1. **Verify and enforce stage-2 oblivious-fetch (all candidates) + lock a minimum bucket size *k*.** ~2–5 units. **Critical** — this is the difference between the v1 privacy claim being true or false. Characterise the *k* vs bandwidth curve and set a floor.
2. **Elevate Tor from docs to in-product notice now; plan OHTTP for v2 free tier.** In-product notice at the query input ~0.5 unit; OHTTP relay + client ~15–25 units (v2). **High** — directly addresses the IP-join, which is the core free-tier weakness and the distress-tier exposure inversion.
3. **Uniform node endpoint + response-size bucketing/padding.** ~3–6 units. **Moderate–high** — defeats cheap role-alignment and SP-scan/batch-size fingerprinting for the on-path observer.
4. **Document the canary's true signalling semantics + decide whether a separate operator-liveness attestation is warranted.** ~1–2 units design (plus the legal question in §7). **High on honesty** — closes the "FROST resolves Boltz" half-truth.
5. **Confirm 2-of-4 cannot sign on real hardware (canary soundness invariant).** ~1 unit (part of the DKG ceremony session). **High** — a false "all fine" at sub-quorum would be the worst possible canary failure.
6. **Separate Bitcoin Knots P2P egress from the query-API address (or P2P over Tor).** ~2–4 units. **Moderate** — removes free node-topology enumeration via the P2P network.
7. **Decouple credential minting timing from query bursts (pre-fetch / jitter / steady-rate top-up).** ~2–4 units. **Moderate** — stops the mint learning "47 addresses about to be queried right now," the sharpest breach-scenario leak.
8. **Add jurisdictionally diverse canary relays + a second channel type (OTS or git mirrors).** ~3–5 units. **Moderate** — thickens the off-node guarantee beyond three same-ecosystem relays.
9. **Authenticated, encrypted DKG coordination channel with out-of-band participant verification; pin `frost-secp256k1` to a confirmed-audited version.** ~2–4 units. **Moderate** — protects the root of trust at its realistic weak point (the channel, not the shares).

---

## 7. Carry-forward items

**Requires legal input (blocks resolution):**
- **IPA compelled-canary-continuation (Opus-C, solicitor).** Can a UK operator be compelled to *continue* participating in canary signing under a gagging order — and separately, compelled to add prospective logging under a Part-3 technical capability notice? The §2.2 prospective-logging finding and the §3.4 signalling-semantics finding both terminate here. Firms already noted: Bristows, Mishcon de Reya, AWO.

**Required implementation confirmations before any node goes live:**
- Stage-2 obliviousness + minimum *k* (§3.1, hardening #1).
- 2-of-4 unsignable invariant on real hardware (§3.4, hardening #5) — fold into the FROST 3-of-4 ceremony session.
- `frost-secp256k1` audit status + version pin (§3.5).
- Nostr relay hosting jurisdictions verified (§3.4) — I could not confirm these without web access.

**Relevant to v2/v3 (one line each, do not elaborate here):**
- OHTTP (RFC 9458) as v2 free-tier IP privacy — natural upgrade, nothing forecloses it (§5).
- Spiral PIR served additively on the existing full index (§5); v1 clients deprecated, not mutated.
- Rotating-mint design forecloses a future stateful metered tier (§5) — revisit only if a metered tier is ever proposed.
- Response-padding / cover-traffic design for SP-scan fingerprinting is a v2 transport topic (CryptoRoadmap-2).

---

*Adversarial-1 complete. This review may generate a hardening work block — items 1, 4 and 5 are gating for build honesty and should land before Legend-6 opens.*

*"Nothing stops this train."*
