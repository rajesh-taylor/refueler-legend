# legend-incident-protocol.md — refueler-legend
> **Version:** 1.1 | **Created:** Legend-6 · 7 Aug 2026 | **Updated:** Legend-7B · 7 Aug 2026
> Operational incident runbook for Legend — privacy-first Bitcoin block explorer.
> Committed to `rajesh-taylor/refueler-legend`. Read by the operator, Enterprise clients,
> and legal counsel. Load in every incident and infrastructure session.
> Companion documents: `legend-node-plan.md`, `legend-enterprise-pricing.md`,
> `legend-design-spec.md`, `legend-ux-language.md`, `legend-scope.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Orientation

Legend operates five nodes: Node A (Hetzner, Falkenstein, DE), Node B (provider TBD, jurisdiction
TBD), Node C (FlokiNET, Reykjavik, IS), Node D (provider TBD, fourth jurisdiction — full
participant and warm standby), Node E (provider TBD, fifth jurisdiction — chain-only cold
standby). One operator, domiciled in London, UK. FROST 3-of-4 threshold signing governs both
credential issuance and warrant canary signing across the four full-participant nodes (A, B, C, D):
any three of four shares are required, the full key is never assembled, and a seized node yields
one share that is computationally useless alone. Node E holds no FROST share. Each full-participant
node publishes one independently meaningful canary daily to three channels (node-hosted
`canary.json`, GitHub mirror, per-node Nostr npub), expiring after 72 hours. Silence reads as
death, by design.

This document states what happens, what clients see, and what the operator does when something
goes wrong. It is written against the architecture in `legend-node-plan.md` and the SLA in
`legend-enterprise-pricing.md`. Where a claim depends on a legal question or an implementation
detail not yet fixed, it is flagged in `## Opus-A notes` at the end, not smoothed over.

The grounding example throughout is the Boltz Exchange canary lapse of early August 2026: a
single PGP-signed canary on one website, no multi-signature, no independent publication, no
automated renewal, a 60-day window, and an expiry during an active AI-assisted attack that the
community — correctly — read at face value. Legend's architecture exists to make the operator's
after-the-fact words unnecessary. Every procedure here is judged against that standard.

---

## Section 1 — Canary failure modes and classification

Four scenarios produce an expired or degraded canary. They are distinguished by cause, not by
appearance — and two of them (C and D) can be identical from outside. The classification exists
so that the operator responds correctly and clients are told, in advance, how to read what they
see.

The governing rule is fixed: **an expired canary is a dead canary. Clients read it at face value
until a fresh FROST-signed statement resurrects it.** The operator's explanation does not change
that. What the architecture provides is not a way to argue an expiry away — it is a set of
structural signals (which canaries expired, whether the query API is still answering, whether a
pre-signed operational notice accompanied the expiry) that let clients reason without trusting
the operator's word.

| Scenario | Canary behaviour | Client observes | Operator action | Client should conclude |
|---|---|---|---|---|
| **A — Operational lapse** (neglect, illness, travel, technical failure; no legal compulsion) | Affected canary expires. No pre-signed operational notice accompanies it if the lapse was genuinely unplanned. | One or more canaries expired on the status page and across GitHub/Nostr. Query API may still answer. | Resurrect with a fresh FROST-signed canary as soon as reachable. If the lapse was planned, the maintenance notice (§7 #1) should already be published. | Treat the expiry at face value until resurrection. An expired canary with no accompanying pre-signed notice is indistinguishable, at the moment of expiry, from compulsion — and should be read as such. |
| **B — Single-node seizure** (one node seized by host-jurisdiction law enforcement; FROST 3-of-4 intact) | The seized node's canary expires. The other three canaries continue — they are signed by the three surviving shares. | One canary expired; three live. Status page shows Reduced splitting. Query API answers on three nodes. | Publish the node-seizure statement (§7 #3) via the three surviving nodes. Re-key to exclude the seized share (§4). Engage counsel. | One node is lost; query splitting is reduced, not absent. No query data existed on the seized node to expose (ephemeral sessions). Three live canaries across three jurisdictions remain a meaningful signal. |
| **C — Legal compulsion (UK IPA 2016)** (operator personally served with a disclosure and/or non-disclosure order; reaches all four full-participant nodes) | All four canaries eventually expire. The operator cannot renew under a non-disclosure order, and the monthly DKG dependency (§4) caps the time to observable expiry at roughly one mint window. Compelled *continuation* of publication is the untested exception — see below. | All four canaries expire, at most within one mint window plus the 72-hour expiry margin. | None available that a client should trust. Under a non-disclosure order the operator may be legally barred from stating a warrant was served. | **Highest-risk state.** Simultaneous quadruple-node expiry should be treated as compromise of the service. Cease reliance and migrate. Do not wait for or trust an operator statement. |
| **D — AI-assisted attack / service suspension** (sustained automated exploit campaign forces query-API suspension; operational exhaustion) | Canaries **continue**, because canary signing is decoupled from the query API (§5). The query API can be suspended entirely while canaries keep signing and publishing. | Query API suspended or degraded, **but canaries live.** Status page shows suspension with a live canary state. | Defend or suspend the query API. Keep the canary signing service and monthly DKG running — they take priority over query-API restoration. Publish the attack-suspension statement (§7 #4). | A suspension accompanied by **live canaries** is operational, not legal. The live canary is the structural evidence that distinguishes D from C. If canaries also expire, escalate to reading it as C. |

**The C/D distinction is the whole point.** The Boltz canary lapsed *during* an active attack,
which is exactly what made the event unreadable from outside: the community could not tell neglect
from coercion. Legend removes that ambiguity by keeping canary continuity independent of query-API
firefighting (§5). Under scenario D, the correct observable is a suspended query API with living
canaries. Under scenario C, the observable is expiring canaries. The operator's exhaustion cannot,
by architecture, convert D into the appearance of C — provided the canary signing service and the
monthly DKG survive the attack. The one residual path by which D degrades into C is a monthly DKG
ceremony falling inside a multi-day attack and not being run; §4 and §5 address that directly.

**The compelled-continuation exception (C).** UK IPA 2016 arguably permits an order compelling the
operator to continue publishing a canary while under a non-disclosure order. This is legally
untested in UK courts (see `legend-node-plan.md` §6 and `## Opus-A notes`). If such an order were
both lawful and served, all four canaries could continue to publish while the service is in fact
compromised. Clients are told this plainly in Enterprise materials: the canary is a strong signal
about orders served on hosting providers and a **limited** signal about orders served on the
operator personally. No architecture defeats a lawful compelled-continuation order against a sole
operator. The honest position is to state the limit, not to imply it away.

---

## Section 2 — Operator incapacitation and dead-man's switch

Legend is operated by one person. This is a stated constraint, not a hidden weakness. This section
sets out what happens when that person is unavailable, and which mitigations are architecturally
sound enough to implement at v1.

### Short-term incapacitation (hours to days)

Daily canary publication is an automated FROST 2-of-3 signing round between whichever two nodes are
online, running during the monthly mint window. It requires no operator action day to day.
A 48-hour operator absence, on its own, changes nothing observable: canaries continue to sign and
publish, and the query API continues to answer.

The absence becomes observable only if it coincides with an event that needs a human: a node
failure requiring manual restart (§3A), or a monthly DKG ceremony (§4). Absent those, the
architecture does not detect a short absence because there is nothing to detect — the service runs.

The design margin is deliberate. Publication cadence is 24 hours; expiry is 72 hours. A single
missed automated publication cycle leaves at least a 48-hour window before any canary reaches
expiry. This margin exists precisely to prevent a brief, benign interruption from being read as
death.

### Extended incapacitation (weeks)

For a sole operator, the honest options and their trade-offs:

| Option | Architecturally sound? | Trade-off | v1 decision |
|---|---|---|---|
| **Automated daily canary signing** (node-to-node, during the mint window; operator-independent) | Yes | None. Removes false-positive expiry from short absence. Depends on the signing service running independently of the query API. | **Implement.** |
| **Pre-signed operational notices** (maintenance §7 #1, incapacitation §7 #2), date-bounded, FROST-signed, held in escrow, released by a *publication delegate* | Yes, with a minor trust assumption | The publication delegate can release a pre-written notice but holds no key share and can forge nothing. This is a far weaker assumption than a signing delegate. | **Implement (optional).** Recommended if the operator wants incapacitation coverage. |
| **FROST share escrow** with a trusted third party (counsel, associate) | No | Escrowing one share to a third party means that party plus one node forms a 2-of-3 signing quorum. It converts a structural guarantee into trust in the escrow holder. | **Do not implement.** State the incapacitation risk instead. |
| **"No-compulsion" auto-firing dead-man's switch** (publishes an all-clear unless the operator cancels within a window) | No | Under compulsion the operator simply stops cancelling, and the switch fires a false all-clear. A timer cannot assert absence of compulsion. | **Do not implement.** |
| **Pre-signed *canaries*** (freshness-proofed statements signed far in advance) | No | A canary embeds a recent block hash to prove it was not pre-signed. A pre-signed canary defeats its own freshness oracle. The concept is self-contradictory. | **Architecturally impossible. Do not implement.** |

The distinction that resolves this section: a **notice** states a condition as of a preparation
date and says so; a **canary** is a freshness-proofed claim about the present. Notices can be
pre-signed and held. Canaries cannot. The incapacitation notice (§7 #2) is honest because it is
explicitly historical — "no legal compulsion has occurred as of the date this statement was
prepared: [date]" — and because it is released by a human on confirmation of incapacitation, not
by a blind timer that could fire during a compulsion the operator is concealing.

### Permanent incapacitation

The honest statement to Enterprise clients, which belongs in the contract materials:

Legend has a single point of human failure. If the operator is permanently incapacitated, the
service ends. FROST key shares are destroyed at each monthly window close and cannot be
regenerated without the operator running the next DKG ceremony. Within at most one mint window,
credential issuance and automated canary signing halt, and all four full-participant node canaries expire.

**Permanent incapacitation and UK IPA operator-level compulsion (scenario C) are indistinguishable
from outside** — both present as simultaneous quadruple-node canary expiry with no trustworthy
operator statement. This indistinguishability does not matter operationally, because the correct
client action is identical in both cases: treat the service as ended, cease reliance, and migrate.
The quadruple-expiry note (§7 #5) is the client-facing document for exactly this state, and it
does not depend on the operator being alive or free to publish it.

---

## Section 3 — Node recovery procedures

### 3A — Node goes offline (provider issue, hardware failure, no seizure)

1. **Detect.** The automated reachability and canary monitor flags the node unreachable. The
   status page reflects the reduced node count within the monitoring interval.
2. **Confirm cause is not seizure.** Check the provider status page, the operator's own probes,
   and — where lawful and available — a provider support ticket. If seizure cannot be excluded,
   switch to §3B and treat the node as lost.
3. **Assess FROST share state.** A node that restarted has lost its in-memory share for the
   current mint window. The mint continues on the surviving shares — with four full-participant
   nodes (A, B, C, D) and a 3-of-4 threshold, a single offline node does not interrupt credential
   issuance or canary signing; the ceremony and daily signing rounds continue on the remaining
   three share-holders. The restored node rejoins as a query node once its index is healthy, but
   **does not hold a signing share again until the next monthly DKG**. Canary signing for all four
   nodes continues on the three live shares.
4a. **Restore — binary integrity.** Before bringing the node back, verify the build
   binary against the signed release manifest (`legend-node-plan.md` §7). Only a manifest-matched
   binary is permitted to rejoin the query pool or the FROST signing set. A node running an
   unverified binary is treated as untrusted regardless of its hardware status.
4b. **Restore — index integrity.** Restore chain and Esplora index from a recent verified
   snapshot, or resync from the Bitcoin p2p network. A restored snapshot must have its integrity
   verified before the node rejoins: either confirm the index snapshot digest against the
   operator's signed snapshot manifest, or perform a full resync — the index is not trusted on the
   strength of the snapshot alone. The "hours to ~1 day" restoration window assumes a snapshot of
   confirmed provenance; an unverifiable snapshot is not a shortcut — it is an untrusted node.
   See `## Opus-A notes` for the open question on snapshot custody and verification method.
5. **Resurrect the canary.** Three of the four share-holding nodes sign a fresh canary statement
   for the restored node, embedding a recent block hash, and publish to all three channels. Silence
   ends only with a fresh FROST-signed statement — there is no automatic un-expiry.
6. **Notify.** Free tier: status page only. Enterprise: notify per SLA if the outage crossed an
   SLA threshold (below).
7. **SLA credit assessment.** The SLA defines availability as **≥2 nodes operational with role-split
   intact.** With five nodes (four full participants + one cold standby), a single-node outage leaves
   three full participants operational: it does **not** breach the 99.0% monthly availability target,
   does not interrupt FROST 3-of-4 signing, and does **not** consume the full-splitting budget.
   Two simultaneous node outages reduce the query pool to two nodes and the FROST signing set to
   two, which is below the 3-of-4 threshold for new credential issuance — this does breach the
   ≥95%-of-month full-splitting budget. Track outage duration per node. A 10% service credit is
   due for any calendar month below 97% availability — which a single-node outage alone cannot
   cause, but two compounding simultaneous outages could.

### 3B — Node seizure confirmed or strongly suspected

1. **Treat the node as lost and hostile.** Do not attempt to recover its share or its data. The
   seized node yields one FROST share (useless alone — three shares are required) and no historical
   query data (ephemeral sessions; nothing is stored to seize).
2. **Confirm the other three full-participant nodes are operational.** The service continues at
   Reduced splitting on three nodes. FROST 3-of-4 signing continues: three of four shares are
   sufficient. Collusion resistance drops from "any three of four" to "the only three." State this
   honestly; do not describe the reduced mode as secure — it is reduced.
3. **Publish the node-seizure statement (§7 #3)** via the three surviving nodes' channels, the
   GitHub mirror, and Nostr. This statement is pre-signed for exactly this moment because the
   operator may be under a non-disclosure order and unable to author a statement live. It reports
   loss of operator control of the node — a true statement — without characterising the legal
   cause, which the operator may be barred from disclosing.
4. **Re-key to exclude the seized share (§4, emergency re-key).** Until re-keyed, the current mint
   window's public key still corresponds to a key set that includes the seized share. Re-key so
   that no future signing set involves it. With three remaining nodes, the mint continues in a
   transient 3-of-3 configuration — all three surviving shares required — until a replacement
   node is provisioned and a fresh 3-of-4 DKG runs. This transient state is documented and
   stated on the status page; it is not described as full-threshold.
5. **Provider communication.** Request confirmation of the legal basis where lawful. Expect that a
   non-disclosure order may prevent both the provider and the operator from confirming anything.
   The pre-signed statement does not depend on the provider's or operator's ability to speak.
6. **Engage legal counsel** immediately on confirmation or strong suspicion.
7. **Client notification.** Enterprise: canary-expiry alert within 12 hours (the seized node's
   canary will expire); incident notification within 24 hours; written post-incident summary within
   5 business days. Free tier: status page.

### 3C — Provider forces migration (FlokiNET or Hetzner relationship ends)

**Replacement provider criteria.** A named vendor list is not maintained because vendors change;
criteria are maintained instead. A replacement must satisfy all of:

- **Jurisdiction preservation.** A replacement for Node C must preserve the property Node C provides:
  a jurisdiction outside the EU legal-assistance fast lane (Iceland is EEA, non-EU). A replacement for
  Node A or B must not collapse the two-jurisdiction distribution or introduce a shared parent company
  with an existing provider (the one-provider problem — a single order on one company must not reach
  two nodes).
- **Dedicated hardware, not VPS.** Shared CPU and storage I/O cannot sustain electrs indexing and
  RocksDB compaction bursts.
- **Spec parity.** 8+ cores x86_64, 64 GB RAM, 2 TB NVMe, 1 Gbps unmetered or high-bandwidth.
- **No shared parent company** with the other two providers.

**Migration sequence** (preserves FROST integrity and canary continuity):

1. Provision the replacement node. Install and verify the build against the signed release manifest.
2. Sync chain and Esplora index (from network, or restore a verified snapshot with confirmed
   provenance per 4b above).
3. Include the replacement in the next FROST DKG — either the scheduled monthly ceremony or an
   emergency re-key (§4) — and exclude the departing node. A live share cannot be migrated; it is
   ephemeral and node-bound. The clean path is a fresh DKG that includes the new node and excludes
   the old. During migration, the mint runs on the three stable nodes (3-of-3 transient configuration)
   while the replacement completes its sync and joins the ceremony.
4. Re-sign the node manifest with the offline Legend release key, removing the departing node's URL
   and canary and adding the replacement's. Publish to `refueler.io/legend/manifest.json`. No code
   change is required — this is a manifest update and re-sign.
5. Decommission the departing node only after the new key is live and the manifest is published.
6. **Client communication.** Enterprise: incident notification within 24 hours if service mode is
   affected during migration; otherwise a scheduled-change notice. Free tier: status page.

**Degraded-migration branch — concurrent node loss during migration.** If a second node becomes
unavailable while a migration is in progress (provider migration of Node C coinciding with a
hardware failure on Node A, for example), the procedure branches:

- **Two stable nodes remain (FROST signing continues at 3-of-3 transient, now 2-of-remaining):**
  below-3-of-4 threshold. New credential issuance halts. Canary signing continues as long as
  the two surviving, share-holding nodes and the DKG window are intact. Prioritise the DKG
  ceremony if a window boundary is approaching. Activate Node D immediately if not yet active;
  activate Node E's Esplora index build.
- **Status page:** show Reduced splitting — migration in progress alongside the node-offline
  state. Do not suppress either condition.
- **Enterprise notification:** treat as a compound incident — notify within 12 hours of the
  second node event regardless of SLA trigger on the first.
- **Do not attempt the migration DKG** until at least three nodes are stable and verified. A DKG
  run with an unverified replacement node included is worse than a delayed migration.

**Time estimate for full restoration.** In the five-node topology, restoration time depends
on which node is being replaced:

- **Replacing with Node D (warm standby):** Node D is fully synced and manifest-registered from
  day one. If the departing node is A, B, or C, Node D promotes immediately with no sync delay.
  The DKG and manifest re-sign complete in minutes. Full restoration to four-node FROST: **minutes
  to hours** (DKG + manifest propagation only).
- **Replacing with a fresh node (Node D consumed, new fourth node required):** dominated by the
  initial index build: Bitcoin Core chain (~650 GB) plus Esplora index (~1 TB) plus the Silent
  Payments tweak index. From scratch: **2–7 days**, bandwidth- and hardware-dependent. From a
  verified snapshot: **hours to ~1 day**, plus tip catch-up — contingent on snapshot integrity
  verification (see `## Opus-A notes`). Node E's chain is continuously synced, eliminating the
  2–7 day chain-sync component; only the Esplora index build remains.
- **DKG and manifest re-sign** themselves take minutes once the replacement node is synced and
  binary-verified.

---

## Section 4 — FROST re-keying

### Full re-key versus mint-window rotation

A **mint-window rotation** is the routine monthly DKG that produces fresh shares and destroys the
previous window's shares at close. A **full re-key** is an *unscheduled* rotation forced before the
window would naturally close — required when a share must be destroyed and regenerated early
because it is compromised, seized, or suspected intruded. Routine rotation is a calendar event;
full re-key is an incident response.

### Scheduled monthly DKG ceremony (all nodes online)

Per `legend-node-plan.md` §5:

1. All four full-participant nodes (A, B, C, D) online. Node E does not participate.
2. DKG produces four shares (one per node) and a single public key. The full private key is never
   assembled.
3. Credential issuance thereafter requires any 3-of-4 nodes to cooperate in a threshold signing
   round.
4. At window close, all four shares are deleted across all four jurisdictions simultaneously.

Status page during the ceremony: **Credential rotation in progress** — the routine-ceremony state,
rendered in `--text-tertiary` (no user action required). Existing valid credentials continue to
work against the current window until it closes; new credential issuance may be briefly unavailable
during the signing round. Address, transaction, and block queries that do not require fresh
credential issuance are unaffected.

### One node offline at ceremony time

With a 3-of-4 threshold, one offline node does **not** delay the ceremony — the DKG proceeds on
the three remaining nodes. The offline node rejoins the FROST signing set at the next ceremony.
A **24-hour extension window** is scheduled only if **two or more** nodes are offline simultaneously.
If nodes remain offline beyond the extension window, the rotation is treated as failed and requires
manual intervention. During the extension, the mint operates on the current window; the status page
shows the rotation state honestly.

### Node share compromised (seizure or suspected intrusion) — emergency re-key

Re-key immediately, regardless of the ceremony schedule, when a share is seized (§3B) or a node is
suspected intruded. Procedure:

1. Destroy the current window's shares on the surviving nodes.
2. Run a fresh DKG across the trustworthy nodes, excluding the compromised node. With three
   remaining nodes, re-key to a documented, transient 3-of-3 configuration until a replacement
   node is live, then re-key to 3-of-4.
3. Re-sign and publish the node manifest to remove the compromised node.

### §3D — Sub-quorum procedure

Sub-quorum is the state in which fewer than three full-participant FROST share-holders are
operational. Under the 3-of-4 scheme, sub-quorum requires three of four full-participant nodes
(A, B, C, D) to fail simultaneously — a materially less likely compound event than under the
original three-node 2-of-3 scheme, where two simultaneous failures sufficed.

**What is impossible at sub-quorum:**
- New credential issuance (requires 3-of-4 signing round — cannot be completed)
- New canary signing for the next publication cycle (same signing requirement)
- Running the §7 #3 or §7 #4 pre-signed statements (those were signed in advance; the current
  state means the operator cannot sign new FROST statements until the quorum is restored)

**What remains possible:**
- Existing valid credentials continue to work for the current window (already signed)
- Already-published canaries remain valid until their 72-hour expiry
- Node E Esplora index build (no FROST participation required)
- Querying on any surviving node (no credential issuance needed for free-tier queries)

**Operator priorities in sub-quorum state, in order:**
1. Note the time of sub-quorum entry and the expiry time of the earliest live canary.
   The operator has at most 72 hours — minus time already elapsed since last publication —
   before canaries begin expiring. The monitor must surface **per-canary time-to-expiry**
   continuously so this deadline is visible in real time.
2. Restore the fastest-to-bring-back node first. If Node D is available (warm standby), promote
   it immediately — it requires no sync time, only binary verification.
3. Activate Node E Esplora index build immediately regardless of which other node is being
   restored. E's chain is already synced; only the index build is outstanding.
4. If a DKG ceremony boundary arrives while still in sub-quorum, do not attempt the DKG —
   a sub-quorum DKG cannot complete. Record the missed ceremony; notify Enterprise clients.
5. Once three share-holders are online and verified, run an emergency re-key (above) immediately.
   Do not wait for the scheduled ceremony window.
6. Enterprise notification: sub-quorum is an SLA breach. Notify within 12 hours of entry
   regardless of whether canaries have expired. Do not wait for expiry to notify.

**DKG-under-attack — the compound failure of sub-quorum arriving during an active exploit
campaign.** This is the residual failure mode identified in Simulation 4 (Opus-B review,
Legend-7): sustained AI-assisted attack produces operational exhaustion precisely as a DKG
ceremony boundary arrives, and the DKG is not run, causing the mint window to close without
rotation, destroying the shares and halting canary signing.

**Procedural safeguard (stated as a priority decision, not a recommendation):** if a DKG
ceremony boundary arrives during an active attack, the query API is suspended or rate-limited
*before* the ceremony begins. The DKG takes minutes; suspension clears the attack surface for
that window. The ceremony outranks query-API continuity. This decision is locked in §5 and is
reproduced here because it is also the sub-quorum prevention mechanism: a completed DKG means
the mint window rotates, shares survive, and the automation that produces daily canaries
continues without operator involvement. A missed DKG is the one path from scenario D to
scenario C that the architecture does not otherwise prevent.

**What to tell clients during the window between suspected compromise and confirmed re-key:** the
status page shows Credential rotation in progress and the reduced node count. The honest position
during this window is that the affected node is treated as untrusted until re-key completes. Do not
overstate: one compromised share is insufficient to issue credentials or forge a canary. Do not
understate: until re-key completes, the affected node is out of the trust set.

### What clients observe during re-keying, and the maximum window

- **Status page state:** Credential rotation in progress.
- **Available:** address, transaction, block, and Silent Payments queries using existing valid
  credentials; canary verification.
- **Briefly unavailable:** new credential issuance during the active signing round.
- **Maximum window before full restoration:** the routine ceremony itself completes in minutes and
  the rotation state clears once the new public key propagates to the signed manifest and node
  endpoints. The node-offline extension (24 hours) is triggered only when two or more nodes are
  simultaneously offline at ceremony time — a single offline node does not delay the 3-of-4
  ceremony. For an emergency re-key, the operator target is completion within 24 hours of confirmed
  compromise, with the status page updated in real time and Enterprise clients notified within the
  standard incident window. Ceremony and propagation timing precision is flagged in `## Opus-A notes`.

---

## Section 5 — AI-assisted attack response

This is distinct from standard incident response because of what the Boltz case demonstrated:
sustained, iterative, automated exploit probing at a pace that overwhelms a solo operator, producing
exactly the operational exhaustion in which a canary lapses by neglect rather than coercion — and
the two are indistinguishable from outside.

### Detection signals on a privacy-first API

Anomaly detection here operates under a hard constraint: **it must not require query logging.** The
resolution is that request *metadata* — rate, size, error class, endpoint, malformed-request ratio,
credential-issuance rate, connection churn, and node CPU/IO saturation — reveals nothing about query
*content*. A counter that records "N requests hit this endpoint and M errored" is not a query log;
it never touches the address, transaction ID, or query payload. Detection runs on these
content-blind counters.

An AI-assisted attack signature: rapid iteration over endpoint and parameter permutations (exploit
fuzzing), error-rate spikes concentrated on specific endpoints, request patterns that mutate faster
than a human operator can patch, and sustained high-rate probing across many variants. The tell is
the *rate of mutation*, not the content.

### Immediate isolation — what can be reduced without killing the canary

**Canary publication is independent of the query API.** This is the central safeguard of this
section. Canary signing (FROST 2-of-3, node-to-node) and publication (`canary.json`, GitHub, Nostr)
continue as long as the nodes run and the current mint window's shares are live. The operator can
suspend the entire query API and the canaries keep signing.

Consequently, everything on the query path can be reduced or removed without touching the canary:
rate-limit the query API, disable the batch endpoint, throttle credential issuance, or suspend the
query API entirely. The only two things that kill a canary are the node ceasing to run or the mint
shares becoming unavailable (a window closing with no DKG). Neither is required to defend against an
attack.

### The specific risk, and the procedural safeguard

The risk: an AI attack creates operational exhaustion, which produces a canary lapse
indistinguishable from coercion. This is precisely the Boltz failure.

The safeguard is structural, not disciplinary. Because canary signing is automated and decoupled
from the query API, the operator's firefighting attention is on the query path while the canary
keeps itself alive with no operator involvement. The lapse-by-neglect failure mode is removed by
architecture. The one residual path is a monthly DKG ceremony falling inside a multi-day attack and
not being run — after which the window closes, shares are destroyed, and the automated canary halts.
**Priority order during an active attack, stated as a decision:** canary and mint continuity
outrank query-API restoration. If a DKG ceremony boundary arrives during an attack, run the
ceremony — it is a short operation — before returning to query-API recovery. Where feasible,
schedule ceremonies away from known-risk windows.

### Rate-limiting and IP privacy

At v1 the free tier is unlimited, with no account and no credential gate, so identity-based
rate-limiting is not available on the free path. During an active attack, temporary IP-based
rate-limiting or an IP-level challenge is an acceptable, time-bounded, documented degradation.
**This is an IP-level control, not query logging** — an IP rate counter records that an address made
requests, not what those requests asked. The copy-register distinction holds: IP privacy (which
free-tier users obtain via Tor) is a separate axis from query privacy (which the role-split
architecture provides). An attack-time IP control touches the former and leaves the latter, and the
no-query-logs guarantee, intact. State this explicitly in any notice, so the control is not
misread as logging.

### Suspend versus degrade

- **Degrade** (rate-limit, disable batch, IP challenge) when the attack is absorbable and the query
  path is not actively exploitable.
- **Suspend** the query API when a live vulnerability is being actively exploited and cannot be
  mitigated in place. Suspension protects node integrity and, critically, does not touch canary
  continuity.

### Public statement language

Use the attack-suspension statement (§7 #4). The honest statement when the cause cannot be confirmed
either way states what is known — service suspended, canary integrity maintained, no query data
exists to expose — and explicitly declines to characterise what cannot be verified. The structural
point carries the message: **a suspension accompanied by live canaries is the evidence that the
suspension is operational, not legal.** During scenario D the canaries stay live; the statement
directs clients to verify them independently (node-direct, GitHub, or Nostr). This is the direct
answer to the Boltz problem — where a canary lapse *during* an attack made the event unreadable. A
living canary during a suspension is self-evidencing in a way the operator's words are not.

---

## Section 6 — Client notification protocols

**Free tier:** status page update only. No accounts, no emails, no direct contact — by design.

**Enterprise:** direct notification within the SLA windows in `legend-enterprise-pricing.md`:
canary-expiry alert within 12 hours of an expired state; incident notification (status page + direct
email) within 24 hours; written post-incident summary within 5 business days; support response
within 4 UK business hours (Mon–Fri, 09:00–18:00 London); 10% service credit for any calendar month
below 97% availability.

### Notification matrix

| Incident | Free tier | Enterprise window | Minimum content | Pre-signed? |
|---|---|---|---|---|
| **A — Operational lapse** | Status page | Canary-expiry alert ≤12h; incident notice ≤24h | Which canary expired; current privacy mode; that resurrection is in progress; that the expiry should be read at face value until resurrected | Notice text pre-written (§7 #1 or #2 per cause) |
| **B — Single-node seizure** | Status page | Canary-expiry alert ≤12h; incident notice ≤24h; post-incident summary ≤5 business days | Which node is out of operator control; Reduced splitting mode; structural reason no query data existed on the node | **Yes — §7 #3** (operator may be under NDO) |
| **C — Legal compulsion** | Status page shows expiring canaries | No trustworthy operator notice is possible | The quadruple-expiry note (§7 #5) is the standing document; no live statement should be expected or trusted | **Yes — §7 #5**, held in contract/onboarding |
| **D — AI attack / suspension** | Status page | Incident notice ≤24h | Query API suspended or degraded; canary integrity maintained; suspension is operational, not legal; how to verify canaries independently | **Yes — §7 #4** |
| **3A — Node offline (no seizure)** | Status page | Incident notice ≤24h if an SLA threshold is crossed | Node offline; cause where known; privacy mode; SLA-credit status; note that single-node outage does not interrupt FROST 3-of-4 at five-node topology | Live-authored |
| **3C — Provider migration** | Status page | Incident notice ≤24h if mode affected; else scheduled-change notice | Migration in progress; mode during migration; estimated restoration window; whether Node D warm standby is active | Live-authored |
| **3D — Sub-quorum** | Status page | Notify ≤12h of sub-quorum entry regardless of canary state | Sub-quorum reached; number of operational share-holders; time-to-expiry of earliest live canary; credential issuance halted; restoration priority | Live-authored |
| **§4 — Re-keying** | Status page: Credential rotation in progress | Incident notice ≤24h for an emergency re-key; none required for routine rotation | Rotation in progress; what is and is not available; maximum window | Live-authored |

### Minimum content of every Enterprise notification

Incident identifier and date (with the current block height, which serves as the trustless
timestamp); affected node(s); current privacy mode (Full splitting / Reduced splitting / Single node
/ Credential rotation in progress / Sub-quorum); what changed; what the client should do;
SLA-credit status if triggered; and the time of the next update. Register: legal-document
precision, honest scope, no alarm language, no overstatement of protection.

**Channel asymmetry publication definition.** A canary is considered **published** when it is
FROST-signed and confirmed on at least two channels including at least one off-node channel (GitHub
mirror or ≥1 Nostr relay confirmation). A canary present only on the node's own `canary.json`
is **not** counted as published — the node cannot vouch for its own status. This definition applies
to incident reporting: if a node's Nostr relay is unreachable but GitHub mirror and `canary.json`
are live, the canary counts as published (two channels, one off-node). If only `canary.json` is
reachable, the canary is degraded and the status page reports a **channel-delivery degradation**,
not a canary expiry. Enterprise notifications must state which channels were confirmed at the time
of the report.

### The special case — canary expired, operator believes no compulsion has occurred

This is the Boltz case directly. Boltz's after-the-fact Stacker News post was reactive, unstructured,
and architecturally irrelevant once the canary had lapsed. Legend's notification for this case is
pre-written, FROST-signed, and structurally independent of the operator's ability to post to social
media (§7 #1 for planned maintenance, §7 #2 for incapacitation).

What it says: which node(s) expired; that the operator's records show no legal order as of the
statement's **preparation date**; that the statement was prepared in advance; that clients should
nonetheless read the expiry at face value under the one-strike principle until a fresh FROST-signed
canary resurrects it; and how to verify that fresh canary when it appears.

What it does not say: it does not assert present-tense absence of compulsion — only as of the
preparation date; it does not ask clients to disregard the expiry; and it does not characterise the
cause with certainty. The honest counterpart to Boltz is a notice that adds structural information
without asking to be trusted.

---

## Section 7 — Pre-signed statement bank

Template text for each statement, held ready to sign or already signed and held in escrow. Bracketed
fields are completed at issuance. Where a statement can be signed live (the operator is available),
it is re-signed with a current block hash at publication; where it must publish while the operator
is unavailable, it carries the preparation-date block hash and states that it was prepared in
advance. A canary embeds a recent block hash to prove it was not pre-signed — these are **notices**,
not canaries, and are honestly date-bounded.

**1. Scheduled maintenance canary pause notice** — FROST-signed by the two operational nodes.

```
LEGEND — SCHEDULED MAINTENANCE NOTICE

Node [X] ([location]) is offline for planned maintenance beginning [datetime UTC].
Its warrant canary is paused and will show as expired during the maintenance window.
This is a planned operational event. No legal order has compelled it.

The canary will resume within [N] hours of [datetime UTC], via a fresh threshold-signed
statement embedding a current block hash.

This notice was prepared before the maintenance window began and is signed by the two
nodes remaining in operation. Verify it against the node manifest at
refueler.io/legend/manifest.json.

Signed: [Node A] + [Node B] + [Node C] (FROST 3-of-4)
Prepared at block: [height] · [hash]
```

**2. Operator incapacitation notice** — held in escrow; released by the publication delegate on
confirmation of incapacitation, not by a timer.

```
LEGEND — OPERATOR AVAILABILITY NOTICE

The operator is temporarily unavailable. Canaries are being maintained by automated
threshold signing under pre-authorised procedures.

No legal compulsion has occurred as of the date this statement was prepared: [date].
This statement makes no claim about any date after its preparation. It is not evidence
about the present. Read the current canary state as the authority.

This notice was prepared in advance and released by a designated publication delegate
who holds no key share and can sign nothing. Verify the live canaries independently
(node-direct, GitHub mirror, or Nostr) before relying on the service.

Signed: [three operational nodes] (FROST 3-of-4)
Prepared at block: [height] · [hash]
```

**3. Node seizure public statement** — pre-signed by the two surviving nodes; published on confirmed
or strongly suspected seizure.

```
LEGEND — NODE STATUS STATEMENT

Node [X] ([location]) is no longer under operator control as of [date].

FROST 3-of-4 threshold signing continues across the remaining three nodes in three separate
legal jurisdictions. One cryptographic key share has been lost with the node. A single
share is computationally useless alone: it cannot issue credentials, forge a canary, or
retroactively sign anything. Three shares are required.

Query splitting is operating in reduced mode across the three remaining nodes. Collusion
resistance is now any-three-of-three rather than any-three-of-four. This is a reduced mode; it
is stated as reduced.

No query data exists on the node for the following structural reasons: Legend holds no
server-side query logs — this is an architectural constraint, not a policy promise.
Sessions are ephemeral, with no session token and no cross-query linkage, so there is no
stored history to reconstruct. Whatever the node held at the moment it left operator
control contained no record of what any user queried.

This statement reports loss of operator control. It does not characterise any legal cause,
which the operator may be prohibited from disclosing.

Signed: [three remaining nodes] (FROST 3-of-4)
Published at block: [height] · [hash]
```

**4. Service suspension — attack (non-legal).**

Timeline decision: this template publishes **without a stated restoration timeline**, with a
committed update cadence instead. The trade-off is deliberate. A stated timeline missed under a
mutating, automated attack destroys credibility precisely when credibility is the asset; an honest
"no fixed timeline, next update at [time]" with a kept cadence is the stronger position.

```
LEGEND — SERVICE SUSPENSION NOTICE

Legend's query API is temporarily suspended due to active exploitation of a vulnerability
under a sustained automated attack.

This suspension is operational, not legal. Canary integrity is maintained: all node
canaries continue to publish. A suspension accompanied by live canaries is the structural
evidence that this is an operational event and not a response to legal compulsion. Verify
the canaries independently — node-direct, the GitHub mirror, or Nostr — and read their
continued validity as the signal it is.

Any rate-limiting or IP-level challenge applied during this incident is an IP-level control.
It is not query logging. No record of query content is created; the no-query-logs guarantee
holds throughout.

We will not state a restoration timeline while the attack is active, because a missed
timeline under these conditions is worse than none. The next update will be published by
[time UTC] regardless of whether service has resumed.

Signed: [operational nodes] (FROST 3-of-4)
Published at block: [height] · [hash]
```

**5. Quadruple canary expiry — no statement possible.** This is not a statement from the operator.
It is the standing note held in the Enterprise contract and client onboarding materials, explaining
what simultaneous quadruple expiry means and what to do. It does not depend on the operator being
alive or free to publish anything.

```
LEGEND — WHAT SIMULTANEOUS QUADRUPLE CANARY EXPIRY MEANS
(Standing note. Held in your contract. Not issued in response to an event.)

If all four full-participant node canaries (Nodes A, B, C, D) expire at or near the same time,
treat the service as compromised or ended and act accordingly: cease reliance and migrate.

Simultaneous quadruple expiry is consistent with three causes that cannot be distinguished from
outside:

  1. UK IPA 2016 compulsion served on the operator personally, reaching all four full-participant
     nodes;
  2. Permanent operator incapacitation, after which the shares cannot be regenerated; or
  3. Catastrophic multi-node failure affecting all four full-participant nodes simultaneously
     (Node E publishes no canary — its absence from the canary set is structural, not a signal).

The correct action is identical in all three cases, which is why the ambiguity does not
matter to you. Do not wait for an operator statement. Under compulsion the operator may be
compelled to publish a false all-clear; incapacitated, the operator cannot speak at all.
The canary state — not any words from the operator — is the authority.

This is the designed endpoint of Legend's architecture. The canary is built so that its
silence is meaningful without anyone's testimony.
```

---

## Section 8 — Revision schedule

This document is reviewed:

- **Quarterly** from v1 launch.
- **After any incident**, regardless of severity.
- **After any change** to node topology, FROST parameters, or canary architecture.
- **After any relevant change in UK law** — IPA amendments, or a court ruling on compelled canary
  continuation (the untested question in §1 and `## Opus-A notes`).

Review responsibility is the operator's. For a sole operator, "reviewed" is not a rubber stamp. It is
an active read of the whole document against the then-current architecture and legal position — each
procedure confirmed to still match what the nodes, the FROST configuration, and the canary
publication actually do — recorded in the log below with a date and the block height at review.

### Revision log

| Version | Date | Block height | Reviewer | Change |
|---|---|---|---|---|
| 1.0 | 7 Aug 2026 | [height at commit] | Operator | Initial document (Legend-6). |
| 1.1 | 7 Aug 2026 | [height at commit] | Operator | Five-node topology and FROST 3-of-4 throughout (Legend-7B). Added: §3D sub-quorum procedure; §4 DKG-under-attack subsection; §3C degraded-migration branch; §3A step 4 split into 4a binary integrity / 4b index integrity; channel asymmetry publication definition in §6; quadruple-expiry standing note in §7 #5. All 2-of-3 and three-node references updated. Opus-A notes unchanged. |

---

## Opus-A notes

**Legal claims about the IPA 2016 to verify with a UK solicitor before this document is finalised:**

- Whether the statement "Node [X] is no longer under operator control" (§7 #3) can be published
  without breaching a non-disclosure / tipping-off provision attached to an IPA order or technical
  capability notice. The document assumes it can, because it reports loss of control rather than the
  existence of a warrant. This assumption needs confirmation.
- Whether IPA 2016 can lawfully compel *continued* canary publication while under a non-disclosure
  order, and if so, whether a compelled operator could also be compelled to run the monthly DKG
  ceremony that keeps the automated canary alive. The §1/§2 argument — that the DKG dependency caps
  the time to observable expiry at roughly one mint window — holds only if compelled continuation
  either is unlawful or cannot reach the DKG. This is the single most load-bearing untested legal
  point in the document.
- Whether a designated **publication delegate** (§2, §7 #2) who releases a pre-signed notice could
  themselves be served and compelled or gagged, and whether their act of publication carries legal
  exposure.
- Confirmation of the MLAT-pathway characterisation (Iceland/EEA-non-EU outside the EU
  legal-assistance fast lane) as it bears on §3C replacement criteria.

**Architectural claims that depend on implementation details not specified in the source documents:**

- **Automated, decoupled daily canary signing.** §5's central safeguard — and §2's short-absence
  argument — assume that daily canary signing runs automatically node-to-node during the mint window,
  as a service independent of the query-API process, and survives a full query-API suspension. If
  canary signing shares a process or a failure domain with the query API, the C/D distinction
  collapses. This must be confirmed as an implementation requirement, not an assumption.
- **Canary signing for a node whose own share is lost mid-window.** §3A assumes the three remaining
  shares can sign and publish the restored node's canary on its behalf until the next DKG. This
  follows from "any three of four" but should be confirmed against the actual publication path (whose
  npub signs the Nostr note, and which host serves the restored node's `canary.json` before it holds
  a share again).
- **Sub-quorum canary continuation (§3D, N-2 state).** With only two share-holders operational,
  the 3-of-4 threshold cannot be met for new signing rounds. The §3D procedure states that
  already-published canaries remain valid until their 72-hour expiry, but does not specify whether
  two remaining participants can produce a valid Schnorr signature under a 3-of-4 key for any
  purpose. This must be confirmed: can two participants produce a partial signature that becomes
  valid when a third rejoins, or does the 72-hour window represent a hard deadline with no
  extension possible? The answer directly affects the operator's priority timeline in §3D step 1.
- **DKG ceremony duration and the observable duration of the "credential rotation in progress"
  state** (§4) are stated as "minutes" without a measured figure. Confirm before quoting a window to
  Enterprise clients.
- **Nostr channel liveness.** The three-channel guarantee assumes the per-node npub's relays are
  reachable at publication and verification time. Behaviour when a node's relays are unreachable is
  unspecified — does the canary count as published if two of three channels carry it?
- **Offline manifest re-sign timing.** §3C assumes the operator can perform the offline release-key
  manifest re-sign within the migration timeline. Confirm the offline key's custody and access
  procedure support this.

**Open questions needed to complete Section 2 (dead-man's switch) to production standard:**

- Is there a designated publication delegate? Are they briefed and willing, and exactly which
  pre-signed notices are they authorised to release, under what confirmation of incapacitation?
- The concrete extended-incapacitation policy: at what elapsed operator silence does the pre-signed
  incapacitation notice (§7 #2) release, and by what mechanism? The document recommends a human
  release, not a timer; this needs to be an explicit, recorded operator decision.
- An explicit operator decision, recorded, that FROST share escrow is **not** implemented at v1 (the
  recommendation), so that the trade-off is a documented choice rather than an omission.
- Index snapshot integrity verification method for §3A/§3C — the recovery-time claims depend on being
  able to trust a restored snapshot (build hash, signed index digest, or full resync). This is
  currently unspecified and directly affects the "hours to ~1 day" restoration figure.
- Whether the permanent-incapacitation client statement (§2) already lives in the Enterprise contract,
  and whether counsel has reviewed the "single point of human failure" disclosure as worded.

*"Nothing stops this train."*
