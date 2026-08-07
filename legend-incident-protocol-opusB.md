# legend-incident-protocol-opusB.md — refueler-legend
> **Opus-B — attack-vector simulations & node-recovery drills** · follows Legend-6 · 7 Aug 2026
> Adversarial review of `legend-incident-protocol.md` v1.0, Sections 3, 4, 5.
> Companion to `## Opus-A notes` (legal + implementation flags). Same register: flag gaps
> clearly, without hedging, for the operator to resolve before v1. Legal questions (IPA 2016,
> NDO, MLAT) are out of scope — that is Opus-C, blocked on solicitor input.
>
> Grounded in the committed v1.0 (`31c5caa`), not the prompt paste (which was left as a placeholder).

---

## Preface — the composability problem

Sections 3 and 4 are written for **single incidents against a three-node floor**. Every
procedure that keeps the canary alive assumes at least two operational, share-holding nodes,
because FROST 2-of-3 needs two participants to sign anything — a mint round, a canary, or a
§7 notice. Four of the six simulations below drive the system *below that floor* by composing
two benign or two independent events. In each case the single-incident procedure is locally
correct and globally silent: it does not fail loudly, it simply has nothing to say about the
compound state, and the architecture's core principle — "silence reads as death, by design"
(Orientation) — then works against a blameless operator. That is the through-line. The
five-part structure follows for each simulation.

---

## SIMULATION 1 — Cascading node failure during FROST re-key

### 1. Scenario
Node A goes offline at hour 0 (Hetzner hardware fault, no seizure). Operator opens §3A. At
hour 6, before Node A is restored, Node B goes offline (unrelated Hetzner issue). State:
Node C alone operational. No signing quorum. No mint. No new canary can be produced. Canaries
expiring within 72 h of their last successful signing round.

### 2. Walk the procedure
- **§3A step 1 (Detect).** Monitor flags A, then B. Status page → *Single node — query splitting
  unavailable.* Correct as far as it goes.
- **§3A step 2 (Confirm cause is not seizure).** Operator must now clear **two** nodes of seizure
  under time pressure. Fine in principle; doubled cognitive load, no guidance on doing it twice
  concurrently.
- **§3A step 3 (Assess FROST share state).** The step reads: *"The mint continues on the two
  surviving shares (2-of-2 signing)… Canary signing for all three nodes continues on the two live
  shares — any two of three suffice."* **There are no two surviving shares.** Only Node C is
  reachable. The step's entire premise is false in this state and it offers no alternative branch.
- **§4 emergency re-key.** Step 2: *"If only two nodes remain, re-key to a documented, transient
  2-of-2 configuration."* There is no "only one node remains" clause. §4 has no sub-two-node path.
- **Aggravating detail.** §3A step 3 also states a restarted node *"has lost its in-memory share
  for the current mint window."* If A and B both **crashed** (not merely network-partitioned),
  both shares are gone; only C's share survives. Recovery then requires a fresh DKG to regenerate
  shares — but §4 emergency re-key step 1 (*"Destroy the current window's shares on the surviving
  nodes"*) presumes the surviving nodes still hold shares, and a DKG needs ≥2 participants online.
  The document never describes share **regeneration after multi-node share loss**.

### 3. Where the procedure holds
Detection and status-page reflection are correct. The 72 h-vs-24 h publication margin (§2) still
buys real time: the operator does not lose canaries the instant the second node drops — they lose
them 72 h after the **last successful signing round**. If restoration of a second node beats that
deadline, no canary expires and the compound event is invisible.

### 4. Where it fails, stalls, or is underspecified
- **No sub-quorum procedure.** Below two operational share-holders, mint, canary signing, and
  §7 notice-signing **all halt**. The document has no section for this state.
- **No signable notice.** Every §7 template is signed *"(FROST 2-of-3)"* by two nodes. With one
  node, the operator **cannot sign any reassurance**. §7 #1 (maintenance) is for *planned* events
  and, if pre-signed and escrowed, could publish — but there is no pre-signed notice for an
  *unplanned, benign, multi-node* outage.
- **Indistinguishable from scenario C.** If the deadline is missed, all three canaries expire ≈
  together → the exact observable §1 assigns to compulsion / permanent incapacitation, and §7 #5
  instructs clients to *"cease reliance and migrate… Do not wait for an operator statement."* A
  fixable double-hardware-fault thereby triggers **irreversible** client abandonment.
- **No real-time expiry readout.** The operator's one governing deadline is "earliest live
  canary's 72 h expiry," and nothing in the document surfaces it. They must reconstruct it by hand.
- **No restore-priority criterion.** Restore A or B first? The document is silent. (Correct answer:
  whichever re-establishes a signing pair soonest — but it must be stated.)
- **SLA exposure (§6).** At 0–1 nodes availability is below the *"≥2 nodes operational"* definition,
  so below 99.0%; the ≤24 h incident-notice obligation lands during the exact window in which the
  operator has no quorum to FROST-sign that notice. The notification the SLA demands cannot be
  produced in the architecture's own trust register.

### 5. Suggested resolution
Add **§3D — Sub-quorum state (fewer than two operational share-holders).** State plainly: below
two live shares, FROST is entirely down — mint, canary, and notice-signing included. Define the
single operating priority: restore any second node before the earliest live canary reaches 72 h,
and require the monitor to expose **per-canary time-to-expiry** so the operator can read that
deadline in real time. Specify share **regeneration** (fresh DKG once ≥2 nodes are back, whether
or not prior shares survived). Pre-sign, during normal three-node operation, a **multi-node
operational-outage notice** held in escrow and released by the publication delegate — while
stating honestly, in the register of the compelled-continuation limit, that clients should still
read triple expiry at face value because a benign sub-quorum outage is externally indistinguishable
from scenario C once signing is down. That indistinguishability is a documented limit, not a
solvable problem.

---

## SIMULATION 2 — Monthly DKG ceremony inside an active AI-assisted attack

### 1. Scenario
The monthly DKG boundary arrives mid-attack. A sustained AI-assisted exploit campaign (§5) is
consuming the operator, who is triaging the query API. §5 orders: *"If a DKG ceremony boundary
arrives during an attack, run the ceremony… before returning to query-API recovery."* All three
nodes are under active probing load.

### 2. Walk the procedure
- **§5 priority order.** *"Canary and mint continuity outrank query-API restoration."* Correct, and
  the decoupling claim (canary signing independent of the query API) is the right principle.
- **§4 scheduled DKG step 1: *"All three nodes online."*** The ceremony *"requires all
  participants."* Under §5's own detection signals, one tell is *"node CPU/IO saturation"* — a node
  can be **reachable but pinned** by attack traffic. §4 has a branch for *"one node offline at
  ceremony time"* (24 h extension) but **none for "online but saturated."** A saturated node is
  neither cleanly online nor offline, and the ceremony may not complete on it.
- **The load-bearing conflict §5 does not resolve.** *"Run the ceremony"* points at the **scheduled**
  DKG, which **includes all three nodes**. But §4's emergency-re-key path exists precisely for
  *"suspected intrusion"* and **excludes** the suspect node. During an active exploit campaign,
  intrusion of a node frequently **cannot be affirmatively excluded**. If the operator obeys §5
  literally and runs the all-nodes scheduled DKG, they mint fresh shares **with a possibly-compromised
  node inside the new key set** — the worst possible outcome, produced by following the instruction.

### 3. Where the procedure holds
The decoupling principle and the priority ordering are sound. Existing valid credentials keep
working during rotation (§4), so query-side users are not stranded by the ceremony itself. The
recognition that a missed ceremony is *"the one residual path by which D degrades into C"* (§1) is
exactly right — the document sees the danger clearly; it just under-equips the operator to avoid it.

### 4. Where it fails, stalls, or is underspecified
- **No clean-conditions sequencing.** The safe move is to **suspend or IP-rate-limit the query API
  first**, drop node CPU/IO below saturation, then run the ceremony on unloaded nodes. §5 permits
  suspension without touching the canary — but nowhere instructs the operator to suspend *in order
  to* run the DKG. The document treats the ceremony as instantaneous and load-free.
- **"Online" is undefined under load.** No criterion distinguishes a participating node from a
  saturated one for DKG eligibility.
- **Scheduled-vs-emergency unresolved under attack.** No rule tells the operator to switch to the
  §4 emergency re-key (exclude the un-cleared node) when intrusion cannot be excluded.
- **DKG failure-domain unconfirmed.** Opus-A flags decoupling for routine **canary signing**. The
  monthly **DKG** is a heavier, all-three-nodes key-generation event; whether *it* shares a failure
  domain or network path with the query API is not asserted anywhere. If it does, the attack can
  starve the ceremony.
- **Duration unmeasured.** Opus-A already flags DKG duration as *"minutes"* without a figure. Under
  attack the window is decisive: if ceremony + 24 h extension both fall inside a multi-day attack
  and no clean run is achieved, the window closes, shares destroy, the automated canary halts —
  D → C.

### 5. Suggested resolution
Add **§4 subsection "DKG under active attack (cross-ref §5)."** Sequence it: (1) suspend or
rate-limit the query API to bring node CPU/IO below a stated threshold before the ceremony; the
ceremony's node-to-node path must be **confirmed isolated** from the query-API failure domain —
promote the Opus-A decoupling requirement from *canary signing* to *canary signing **and** the
monthly DKG*. (2) Decision rule: if the intrusion of any node **cannot be affirmatively excluded**,
run the §4 **emergency re-key excluding** that node (accept transient 2-of-2) — **never** fold an
un-cleared node into fresh key material via the scheduled ceremony. (3) Give *"online"* an operational
test: a node that completes a probe signing round within [N] s under current load; otherwise treat
as offline and use the 24 h extension. (4) Record the measured ceremony duration so the operator
knows whether it fits a lull in the attack.

---

## SIMULATION 3 — Single-node seizure followed by provider-forced migration

### 1. Scenario
Hour 0: Node C (FlokiNET, Reykjavik) seized. Operator runs §3B — treat as lost/hostile, publish
§7 #3, emergency re-key (§4) to 2-of-2 on Nodes A + B. Hour 36, still in interim 2-of-2: Hetzner
terminates Node A's contract with 30 days' notice (§3C). The operator must complete the §3B
posture, replace Node A **and** the seized Node C, run DKGs to restore 2-of-3, and hold canary
continuity — from a starting point of two nodes, one of which is departing.

### 2. Walk the procedure
- **§3B steps 1–4.** Correct at hour 0: two nodes can sign §7 #3; emergency re-key to 2-of-2 on
  A + B per §4 step 2. Transient degraded state entered as documented.
- **§3C migration sequence step 3: *"During migration the mint runs 2-of-2 on the two stable
  nodes."*** In the base three-node case the two "stable" nodes are the two not migrating. **Here
  Node C is already gone**, so the two nodes are A and B — and A is the one departing. The moment
  A is decommissioned there is **one** stable node (B), not two. §3C's premise is false in the
  compound state.
- **§3C step 5: *"Decommission the departing node only after the new key is live."*** This is the
  saving clause — it lets the operator keep A alive through provisioning. But the document never
  states that in a **post-seizure** migration this is *mandatory*, because there is no second stable
  node to fall back to. It reads as good practice, not as the single thing preventing collapse.
- **Two replacements, not one.** §3B already committed the operator to provision a replacement for
  seized Node C (C′). §3C now adds a replacement for departing Node A (A′). Restoring 2-of-3 needs
  three of {B, A(departing), C′, A′}. The document sequences **neither** the two provisioning tracks
  nor the two DKGs.

### 3. Where the procedure holds
The pre-signed §7 #3 works at hour 0. The criteria-not-vendor-list model (§3C) is sound and, applied
to C′ and A′, correctly demands jurisdiction preservation and no shared parent. Step 5's ordering,
*if* read as mandatory here, keeps ≥2 nodes alive.

### 4. Where it fails, stalls, or is underspecified
- **Drops below the two-stable-node floor.** As above: mid-replacement the only stable node is B.
  For the provisioning duration (§3C: *"hours to ~1 day"* from snapshot, **"2–7 days"** from scratch)
  the system risks running on one node → Simulation 1's sub-quorum state → triple expiry.
- **2-of-2 is zero-fault-tolerant and the document never says so.** A 2-of-2 mint requires **both**
  nodes to sign every round. A single blip on A **or** B during a multi-day provisioning window halts
  canary signing. The document calls 2-of-2 *"reduced"* but never flags that it has **no redundancy**
  for canary continuity — a materially different claim from "reduced splitting."
- **Latent Hetzner concentration contradicts §3C's own criterion.** Node A (Hetzner, Falkenstein)
  and Node B (Hetzner, Helsinki) **share a parent company**. §3C requires *"No shared parent company
  with the other two providers"* and warns of *"the one-provider problem — a single order on one
  company must not reach two nodes."* The **base topology already violates this**: a Hetzner-wide
  action reaches A and B together. In this simulation, with C already seized, a Hetzner action
  broader than A's contract termination is an instant **two-node** event → single node → collapse.
- **No double-replacement sequencing.** Ordering of C′ and A′ provisioning and their DKGs is
  unspecified.
- **Overlapping SLA clocks.** §3B (canary-expiry ≤12 h, incident ≤24 h, summary ≤5 business days)
  and §3C (incident ≤24 h if mode affected) run concurrently with different origins; the 3C notices
  are *"live-authored,"* i.e. not pre-signed, during a period when the operator is stretched.
- **No status-page state for the compound mode.** The four locked modes (Full / Reduced / Single /
  Rotation) cannot express "Reduced, mid-migration, second replacement pending." The operator must
  pick a lossy label.

### 5. Suggested resolution
Add to **§3C a branch: "Migration while already degraded (post-seizure or post-failure)."** State:
if the service is already at two nodes when a migration is forced, keeping the departing node
operational until the first replacement is synced and a 2-of-3 DKG completes is **mandatory, not
optional** — never drop below two live share-holders. If the departing provider will not allow the
node to remain alive for the provisioning window, classify the event as **triple-expiry risk** and
invoke the pre-signed multi-node-outage notice (Sim-1 resolution). Sequence double replacement
explicitly: provision the **seized/lost** node's replacement first, DKG to 2-of-3, *then* run the
departing node's migration as an ordinary §3C. Flag 2-of-2 as **zero-fault-tolerant for canary
continuity** and name the provisioning window as an elevated triple-expiry period. Finally, add a
criteria note: **the current A + B Hetzner concentration violates §3C's own "no shared parent"
criterion** and should be corrected (move A or B to a distinct parent); until corrected, document
it as an accepted exposure in which a single Hetzner action is a two-node event.

---

## SIMULATION 4 — Anomaly false negative: slow-rate credential/data exhaustion

### 1. Scenario
The attacker never trips §5's tell. Credentials issued at normal rate across many IPs; a patient,
low-volume extraction of address/UTXO data under every rate, error, malformed-ratio, and mutation
threshold the section names. Rate of mutation: zero. They use the system exactly as designed.

### 2. Walk the procedure
- **§5 detection model.** *"The tell is the rate of mutation, not the content."* Content-blind
  counters: rate, size, error class, endpoint, malformed ratio, credential-issuance rate, connection
  churn, CPU/IO. Applied here: mutation ≈ 0, error rate ≈ 0, malformed ratio ≈ 0, per-IP rate normal.
  **Nothing fires.** The attack is invisible to the detection model as written.
- **§5 rate-limiting.** *"At v1 the free tier is unlimited, with no account and no credential gate,
  so identity-based rate-limiting is not available on the free path."* Confirmed against CLAUDE.md
  and `legend-scope.md` (free tier: unlimited, no credential gate at v1). So on the free path there
  is **no per-request credential to count** and **no identity to correlate** — and correlating would
  require the query logging §5 forbids.

### 3. Where the procedure holds
The content-blind philosophy is correct and non-negotiable, and §5 already lists **node CPU/IO
saturation** among its counters — which is the *right* surface for the one sub-case that genuinely
matters here (see below). Suspend/degrade remains available if nodes saturate.

### 4. Where it fails, stalls, or is underspecified
- **Rate-of-mutation is the wrong sole tell for a within-spec harvest.** §5 defines the attack class
  by *fuzzing* behaviour; a within-spec harvest has no fuzzing signature. The section does not
  acknowledge a within-spec threat class at all.
- **Counters are not specified global-vs-per-source.** A distributed low-per-source harvest defeats
  per-source counters. An **aggregate (global)** credential-issuance or request-rate ceiling would
  still register the total climbing — but §5 lists *"credential-issuance rate"* without saying whether
  it is measured globally. On the free path this is moot anyway (no credential gate), which sharpens
  the gap: the only content-blind signal left for free-tier slow harvest is **aggregate node cost**.
- **No cost-based counter distinct from rate.** The expensive endpoints are the real exposure:
  full-range Silent Payments scans (`legend-scope.md`: ~8 min parallelised, 5–10 concurrent per node)
  and 50-address batch queries (NUT-29). A slow, sustained harvest of *costly* queries saturates nodes
  at a **low request rate** — the tell there is aggregate compute, not mutation. §5 has no
  cost-per-interval counter.
- **No stated position on two uncomfortable truths.** (a) Extracting *public* chain data is not a
  breach of anyone's private data — the chain is public; the harm is resource/cost, not
  confidentiality. (b) Content-blindness means Legend **cannot distinguish an owner querying their
  own addresses from an analyst querying someone else's**, so Legend can be used as privacy
  infrastructure *for* third-party surveillance. `legend-scope.md` draws the line at not **building**
  an analytics API; it does not — and content-blindly *cannot* — police plain-explorer use. §5 never
  states this boundary.

### 5. Suggested resolution
No architectural change breaks the privacy model; the fixes are additive and honest. In §5:
(1) add an **aggregate expensive-endpoint compute counter** (content-blind: "N core-seconds of
full-range-scan / batch work per interval, service-wide"), decoupled from rate-of-mutation, as the
detection surface for slow harvests of costly queries; expensive endpoints get a **global concurrency
cap** independent of any identity. (2) Specify all counters as **global as well as per-source**, so
distributed low-per-source campaigns register in aggregate. (3) State plainly that a within-spec,
low-and-slow read of **public** chain data is **not** a threat §5 defends against — the data is
public and detection would require forbidden logging — and that this, together with the inability to
distinguish owner from observer at query time, is the **accepted cost of the no-logging guarantee**,
stated in the same honest register as the compelled-continuation limit.

---

## SIMULATION 5 — Canary publication path failure: Nostr relay unreachable

### 1. Scenario
Node B's Nostr relay becomes unreachable during normal operation. Node B's canary is FROST-signed
and published to `canary.json` (node-hosted) and the GitHub mirror; the per-node npub Nostr
publication fails. Opus-A flags exactly this: *"does the canary count as published if two of three
channels carry it?"*

### 2. Walk the model
- **Orientation** treats the three channels as parallel distribution of one signed canary:
  *"one independently meaningful canary daily to three channels (node-hosted `canary.json`, GitHub
  mirror, per-node Nostr npub)."* The canary here is **validly signed** (FROST succeeded) and live
  on **two** channels; only one *distribution* channel failed. Signed ≠ published, and the document
  does not separate the two.
- **Status page** (`legend-design-spec.md`): per-node canary block, live = no colour, expired = amber
  *"NODE B · CANARY · EXPIRED"*, current state only, refresh-driven. What **drives** the page's
  live/expired decision across disagreeing channels is unspecified.

### 3. Where the model holds
Three channels give real redundancy; a single relay outage does not stop signing, and clients on
`canary.json` or GitHub see the fresh, valid canary. The event is *recoverable and non-fatal* — the
question is purely how it should be **read and labelled**.

### 4. Where it fails, stalls, or is underspecified
- **(a) What a monitor observes:** different clients see different things. A **Nostr-only** monitor
  sees Node B's canary missing/stale and, under §1's rule *"an expired canary is a dead canary…
  read it at face value,"* concludes death. A GitHub/node-direct monitor sees it live. The
  three-channel design manufactures a **split observation**.
- **(b) A/B/C/D classification:** strictly, none — the canary is not expired. But a single-channel
  monitor experiences a **false scenario-A/B** (operational lapse / single-node) reading. *"Silence
  reads as death, by design"* here fires on benign relay unreachability.
- **(c) Operator action:** undefined. Republish on relay recovery? Rotate relays? Treat 2-of-3 as
  sufficient and do nothing? No rule, no threshold, no status-page treatment.
- **(d) The sharp gap — the three channels are NOT interchangeable.** The naive answer ("two of three
  is fine") is unsafe because **which two** is decisive. The node's own `canary.json` **cannot vouch
  for itself**: a seized/compromised node can keep serving a stale-but-not-yet-expired `canary.json`
  while its off-node channels go dark. So *losing Nostr while `canary.json` + GitHub remain* is
  tolerable; *losing both off-node channels, leaving only `canary.json`,* is exactly the state a
  compromised node produces and must **not** count as published. The document counts channels
  symmetrically; it must count them **asymmetrically**.

### 5. Suggested resolution
Define the publication success criterion in the Orientation and §5: a canary is **published** when it
is FROST-signed **and** available on **at least two channels including at least one off-node channel
(GitHub mirror or Nostr)**; the node-hosted `canary.json` alone is **never** sufficient to establish
liveness. State that a **single off-node channel outage (e.g. Nostr) with `canary.json` + GitHub live
is a channel-delivery degradation, not a canary expiry**, and add a status-page state to say so —
distinct from the amber EXPIRED state — so a single-channel absence is read against the other channels,
not at face value. Specify per-npub **relay redundancy** (≥ N relays) and an operator action to
republish/rotate when a relay is unreachable beyond [X] hours. This directly resolves the Opus-A
question: *two of three counts only if one of the two is off-node.*

---

## SIMULATION 6 — Node recovery snapshot integrity failure

### 1. Scenario
Node A offline (§3A). Operator restores from an index snapshot (§3A step 4). Step 4 requires
*"verify the build hash against the signed release manifest"* and *"A restored index snapshot must
have its integrity verified — see ## Opus-A notes."* Opus-A: *"currently unspecified."* Integrity
verification is the operator's blocking step.

### 2. Walk the procedure
- **§3A step 4 conflates two different verifications.** (i) **Build hash vs signed release manifest**
  verifies the **binary** is the genuine Legend release — specified (`legend-node-plan.md` §7), sound.
  (ii) **Index snapshot integrity** verifies the ~650 GB chain + ~1 TB Esplora index restored from
  snapshot is untampered — **unspecified**. The build hash covers the binary, **not** the data:
  a genuine binary will happily serve a poisoned index. The step reads as if one verification implies
  the other; it does not.

### 3. Where the procedure holds
Binary verification against the signed manifest is specified and correct. Resync-from-network (§3A
step 4 alternative) is a genuinely trustless fallback — at the cost of the 2–7 day timeline.

### 4. Where it fails, stalls, or is underspecified
- **No integrity method for the index snapshot** — the operator's blocking step has no defined
  procedure to complete.
- **The quoted fast timeline is unsupported.** §3C: *"From a verified index snapshot… hours to ~1
  day… contingent on snapshot integrity verification (see Opus-A notes)."* Whether "hours to ~1 day"
  is achievable depends entirely on **which** integrity method is chosen — and none is chosen.
- **Every degraded-window duration depends on this.** The transient 2-of-2 windows in §3B and §3C,
  and the sub-quorum deadline in Simulation 1/3, are only as short as the restore. If the fast
  restore is not trustworthy, the honest figure is the **2–7 day** resync, which lengthens every
  degraded window, worsens the triple-expiry risk, and changes the §3A step 7 / §6 availability and
  10%-credit math.

### 5. Suggested resolution
Specify an index-integrity method that meets the document's own *"structural, not policy"* standard.
Preferred, **trustless**: restore the **chain** snapshot, let Bitcoin Knots fully validate it
(self-verifying against proof-of-work), then **rebuild the Esplora index locally** from the validated
chain — the index is a deterministic function of the chain, so its integrity reduces to the chain's
and needs no external signature. This is precisely where the fork's BLAKE3-accelerated ARM indexing
earns its place; quote the **measured rebuild time** rather than "hours to ~1 day." Acceptable
alternative: a **signed index digest** (operator produces and offline-key-signs a Merkle digest of a
known-good index at block height H; restored snapshot verified against it) — but this inherits the
offline-key custody/access question Opus-A already flags for the §3C manifest re-sign, and needs a
stated production cadence and height granularity. Either way: **split §3A step 4** into "binary
verification" and "index integrity" as separate steps, and do not quote a restoration figure to
Enterprise until the chosen method's real duration is measured. Until then the quotable figure is the
2–7 day resync.

---

## Ranked gap list — five most serious, by operational consequence

**1. Sub-quorum (≤ 1 operational node) is unhandled across §3A and §4.** *(Sim 1; feeds Sim 3.)*
Both sections floor at two nodes; below two, mint, canary signing, and §7 notice-signing all halt,
and there is no procedure, no signable notice, and no operator criteria. A benign double outage
becomes externally indistinguishable from scenario C and triggers irreversible client migration per
§7 #5 — during the exact window in which the operator has no quorum to sign the §6 notice the SLA
demands. *Resolution:* new **§3D — sub-quorum state**: declare FROST fully down below two shares;
set the single priority (restore a second node before the earliest canary's 72 h) backed by a
per-canary time-to-expiry monitor readout; specify share regeneration via fresh DKG; add an
escrow-held pre-signed multi-node-outage notice; state the indistinguishable-from-C limit honestly.

**2. "Run the ceremony" under attack conflates scheduled DKG (include all) with emergency re-key
(exclude suspect), and gives no clean-conditions sequencing.** *(Sim 2.)* Followed literally when
intrusion cannot be excluded, §5's instruction mints fresh shares with the attacker inside the key
set — and §4 has no "online-but-saturated" branch. This is the D → C degradation path §1 itself
names, under-specified in the most dangerous direction. *Resolution:* §4 subsection — suspend/
rate-limit the query API to unload nodes before the ceremony; confirm the DKG shares no failure
domain with the query API; if any node's intrusion cannot be excluded, run the emergency re-key
excluding it, never the all-nodes DKG; define "online" under load; measure ceremony duration.

**3. Compound degradation (seizure → forced migration) drops below §3C's two-stable-node premise,
with no double-replacement sequencing and a latent Hetzner concentration.** *(Sim 3.)* §3C step 3's
"2-of-2 on the two stable nodes during migration" is false once a prior seizure has already spent a
node; the interim 2-of-2 is zero-fault-tolerant for canary continuity (undisclosed); and A + B both
on Hetzner already violate §3C's own "no shared parent" criterion, making a Hetzner-wide action a
two-node event. *Resolution:* §3C "migration-while-degraded" branch (keep the departing node alive
until the first replacement is synced — mandatory; sequence the two replacements; never fall below
two share-holders); flag 2-of-2's zero fault tolerance; correct or explicitly document the Hetzner
concentration.

**4. Index snapshot integrity is unspecified, so the "hours to ~1 day" restore figure is unsupported
— and it sets the length of every degraded window.** *(Sim 6; amplifies 1 and 3.)* The fast restore
is what keeps the transient 2-of-2 and sub-quorum windows short; without a defined method the honest
figure is the 2–7 day resync, which lengthens every degraded window and rewrites the SLA/credit and
triple-expiry math. *Resolution:* adopt trustless index rebuild from the PoW-validated chain
(preferred) or a signed index digest with defined custody/cadence; split §3A step 4 into binary vs
index-integrity verification; quote only the measured figure.

**5. Canary "publication" is undefined on channel disagreement, and the three channels are wrongly
treated as interchangeable.** *(Sim 5.)* A transient Nostr outage reads as canary death to
single-channel monitors (false scenario A/B), and the channel-counting cannot distinguish the safe
loss (Nostr down; `canary.json` + GitHub live) from the dangerous state a compromised node produces
(only its own `canary.json` live). *Resolution:* define published = FROST-signed **and** on ≥ 2
channels **including ≥ 1 off-node channel**; state the node's own `canary.json` cannot vouch for
itself; add relay redundancy and a status-page "channel-delivery degradation" state distinct from
EXPIRED; instruct clients to read a single-channel absence against the other channels.

---

*Cross-cutting note for the operator:* four of the five ranked gaps are the **same structural fact**
seen from different angles — the FROST 2-of-3 floor is two live share-holders, and every procedure
that keeps the canary alive silently assumes it. The highest-leverage single fix is to make that
floor **explicit and instrumented**: one monitor field for per-canary time-to-expiry, one declared
sub-quorum procedure, and one escrow-held notice for the benign multi-node case. That converts the
architecture's "silence reads as death" from a trap for a blameless operator into a state the operator
can see coming and act on.

*"Nothing stops this train."*
