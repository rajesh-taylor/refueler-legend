# legend-node-plan.md — refueler-legend
> **Version:** 1.1 | **Created:** Legend-0 · 5 Aug 2026 | **Updated:** Legend-7 · 7 Aug 2026
> Node infrastructure topology for Legend — privacy-first Bitcoin block explorer.
> Infrastructure section only. Economics section appended in Legend-1.
> Load in every build and infrastructure session.
> Companion documents: `legend-design-spec.md`, `legend-scope.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Section 1 — Node count and locations

### Five nodes. Locked from Legend-7.

Two is the mathematical minimum for role-split query sharding but leaves zero redundancy margin. Three gives role rotation per query, N-1 graceful degradation, and three independent warrant canaries across two legal jurisdictions. The incident protocol adversarial review (Legend-7, Opus-B) revealed that three nodes is insufficient: a double hardware fault or a seizure-then-migration compound event drives the system below the FROST signing floor, producing a state externally indistinguishable from legal compulsion. Five nodes resolves this.

**Node D** is a full warm standby: FROST participant, fully synced chain and Esplora index, manifest-registered, fourth independent jurisdiction, live from day one. When any of Nodes A–C is lost, D promotes immediately with no sync delay. **Node E** holds a full, continuously synced Bitcoin Knots chain only — no Esplora index, no FROST share. Its sole purpose is to be ready to build its Esplora index and join the manifest as the new D when D is consumed. The chain is the slow part of provisioning (2–7 days from scratch); E eliminates that delay, capping the window of four-node exposure after D is used.

The signed node manifest architecture (Section 3) means adding or replacing a node requires a manifest update and re-sign — no code changes.

### Locations

| Node | Provider | Datacentre | Jurisdiction | Role |
|---|---|---|---|---|
| Node A | Hetzner | Falkenstein, DE | Germany (EU) | Full participant |
| Node B | [non-Hetzner provider — TBD] | [TBD] | [different jurisdiction — TBD] | Full participant |
| Node C | FlokiNET | Reykjavik, IS | Iceland (EEA, non-EU) | Full participant |
| Node D | [fourth provider — TBD] | [TBD] | [fourth jurisdiction — TBD] | Full participant + warm standby |
| Node E | [fifth provider — TBD] | [TBD] | [fifth jurisdiction — TBD] | Chain-only cold standby |

**Provider selection criteria for Nodes B, D, E** (same as §3C of the incident protocol): dedicated hardware (no VPS); 8+ cores x86_64, 64 GB RAM, 2 TB NVMe, 1 Gbps unmetered; no shared parent company with any other node's provider; jurisdiction preserves the no-single-order-reaches-two-nodes property. Node B replaces the original Hetzner Helsinki node — the original topology had two Hetzner nodes, violating the no-shared-parent criterion. Candidates: Frantech/BuyVM (Luxembourg), Njalla (Sweden), Servers.com (Baltics), OVHcloud dedicated (FR — separate from Hetzner GmbH). Provider session required before provisioning.

**Why this combination:**

Hetzner provides operational reliability and cost efficiency. Two Hetzner nodes are
acceptable because the FlokiNET Iceland node sits outside the EU legal-assistance
fast lane. Iceland is EEA but not EU — the MLAT chain from a UK or US order to
Icelandic infrastructure runs through a different legal pathway than a request
to EU-domiciled providers.

**The honest caveat that must appear in all user-facing materials:**

All three nodes are operated by one person in London. The UK Investigatory Powers Act 2016 can compel disclosure and arguably compel continued canary publication against the operator regardless of where hardware is located. Per-node geographic distribution protects against orders served on the *hosting providers* more robustly than orders served on the operator personally. The canary architecture is strongest as a signal about hosting-provider orders; it is limited as a signal about operator-level compulsion. This distinction is stated plainly in the privacy explainer modal and in Enterprise ontract materials. A product that overstates its legal protections is weaker than on that understates them.

**The one-provider problem:**

Three Hetzner nodes would be one company. A single order served on Hetzner GmbH reaches Falkenstein, Helsinki, and any other Hetzner location simultaneously. Geographic distribution within one provider buys latency and hardware redundancy — it buys nothing for MLAT purposes. The original topology (Nodes A and B both Hetzner) violated this criterion and is corrected in v1.1: Node B moves to a provider with no shared corporate parent with Hetzner. Each of the five nodes must satisfy the no-shared-parent requirement independently. A provider session will confirm selections for Nodes B, D, and E before provisioning.

---

## Section 2 — Node specifications

### Per-node hardware (all three nodes identical)

| Component | Specification | Reason |
|---|---|---|
| CPU | 8+ cores, x86_64 | RocksDB compaction is multi-threaded; electrs indexing benefits from parallelism |
| RAM | 64 GB | electrs in Esplora mode requires substantial RAM during initial sync and compaction |
| Storage | 2 TB NVMe | Bitcoin chain ~650 GB now; Esplora index adds ~1 TB+; headroom to ~2029 before upgrade cycle |
| Network | 1 Gbps unmetered or high-bandwidth | PIR role-split sends more data to client than a standard Esplora query by design |
| OS | Ubuntu 24.04 LTS | Long-term support, unattended-upgrades for security patches |

**Dedicated, not VPS.** VPS instances share CPU and storage I/O. Initial electrs indexing and RocksDB compaction will saturate I/O in bursts that a shared environment cannot sustain. Hetzner AX-class dedicated (AX41 or AX51) is the appropriate tier for nodes A and B. FlokiNET dedicated for node C.

**Storage growth rate:** Bitcoin chain grows approximately 60 GB/year. The Esplora index grows proportionally. 2 TB NVMe provides approximately four to five years of headroom before a storage upgrade cycle, assuming current chain growth rate.

### What each node runs

- Bitcoin Knots full node (~650 GB chain data) — BIP110 policy enforcement enabled, RBF disabled. Knots chosen over Core for principled mempool policy: disabling RBF closes a known attack vector against multisig users with compromised seed phrases, consistent with Legend's position as infrastructure that works for the owner, not the observer. Release cadence monitored; node updates verified against the Knots release manifest before deployment.
- BLAKE3-accelerated electrs fork in Esplora mode (query API)
- Legend query API layer (role-split routing, Cashu credential verification)
- Per-node Cashu mint participant (FROST key share — see Section 5)
- Warrant canary publication service (see Section 6)
- Node status health endpoint (public, unauthenticated, no query data)

### Full index per node — locked

Every node holds a complete Bitcoin chain index. Roles (index node, data node) are
logical assignments made per query by the browser, not physical data partitions.

If nodes physically sharded the data, role rotation would break, failover would
require routing to specific nodes by data slice, and stage-2 of a role-split query
would leak which partition was accessed. Full copy per node is the only topology
consistent with the privacy architecture.

### BLAKE3 — role on these nodes

These are x86_64 dedicated boxes, not ARM. BLAKE3's advantage over SHA-256 is
narrower on x86 than on ARM because x86 has SHA-NI hardware acceleration for SHA-256. BLAKE3 still outperforms SHA-256 on x86 for bulk parallel operations — initial sync, RocksDB compaction, Silent Payments block scanning — because BLAKE3 parallelises across cores where SHA-256 is sequential per call.

BLAKE3 replaces SHA-256 for: RocksDB index lookup key generation, cache tag generation, and internal state hashing in the Silent Payments scanner. Bitcoin consensus hashing (SHA-256d on block headers and transaction IDs) is untouched. From a Sparrow Wallet or any Electrum-protocol client perspective, the indexer is indistinguishable from upstream electrs. BLAKE3 is entirely internal.

The primary BLAKE3 benefit on ARM/Raspberry Pi (the self-hosting use case) is larger — 4–8× faster than SHA-256 on ARM SIMD/NEON. The Hetzner/FlokiNET nodes benefit from BLAKE3 at scale and during initial indexing; the codebase consistency with the self-hosting target is a secondary benefit at no cost.

---

## Section 3 — PIR sharding topology

### The gateway problem — resolved

The original claim — "no single node sees the complete query, the client reassembles" — contains a structural contradiction when a gateway node does the splitting: the gateway sees everything and is a surveillance point with extra steps.

**No gateway. Ever.** The browser talks to nodes directly. This is the only topology consistent with the architecture's privacy claims.

A Bitcoin address query is not naturally splittable — you cannot send half an address to node A and half to node B and get anything useful back. The solution is role-split querying, not data-split querying.

### v1 — Role-split querying with nested NUT-00 blinding

The query proceeds in two stages, to two different nodes, chosen per query by the browser:

**Stage 1 — Index node (selected by browser for this query):**
The browser sends a *script-hash prefix* — not the full address. The node returns a
candidate set of opaque row identifiers. The index node knows only that a query arrived for one of approximately N addresses sharing that prefix — a k-anonymity set, tunable by prefix length. Longer prefix = smaller anonymity set = faster response. Default prefix length chosen to provide a meaningful anonymity set without degrading latency.

The browser blinds its query credential (NUT-00) before sending to the index node.
The index node verifies the credential without learning which session produced it.

**Stage 2 — Data node (a different node, selected by browser for this query):**
The browser sends the opaque row IDs received from stage 1 to a *different* node.
The data node returns the full UTXO and transaction data for those row IDs. The data node sees row IDs, not the original address — it cannot reconstruct what was queried without the stage-1 mapping, which it never receives.

The browser re-blinds a fresh routing token before presenting to the data node.
The data node cannot link this request to stage 1 even if it colluded with the index node after the fact — the blinding operations happened client-side and the blinding factors are ephemeral and never leave the browser.

**Browser reassembles:** two `fetch()` calls and a client-side filter. No installed
software required. Fully feasible in a vanilla JS SPA.

**Role rotation:** the node that serves as index node for query 1 serves as data node for query 2. No node accumulates a consistent view across queries from any session. The browser randomises node selection per query from the signed node manifest.

### What v1 sharding honestly achieves

The honest claim: **reconstructing a user's query requires collusion between the index node and data node for that specific query, plus possession of both blinding factors — which exist only in the browser and are discarded after the query completes.**

This is not cryptographic PIR. It is collusion-resistant query splitting with blind
credential unlinkability. These are different claims and the product documentation
uses the precise language, not the stronger claim.

v1 achieves:
- No single node sees the complete query
- Post-hoc collusion between nodes cannot reconstruct historical queries
- Credential unlinkability: no node can link two queries to the same session
- No server-side query logs: nothing to reconstruct from

v1 does not achieve:
- Mathematical proof that the server cannot learn the query (that is v2 Spiral PIR)
- Protection against a node that is actively logging in real time despite the architecture

### v2 — Spiral PIR upgrade

Spiral PIR (MIT, 2022) is single-server PIR. The server answers the query without
learning what was asked, provably, even in real time. It replaces the v1 role-split
approximation for UTXO lookups. The NUT-00 credential layer does not change between
v1 and v2 — the privacy guarantees stack independently.

### Signed node manifest

The browser fetches a signed manifest at session start:

```
{
  "nodes": [
    {"id": "node-a", "url": "https://...", "roles": ["index", "data"], "canary": "https://..."},
    {"id": "node-b", "url": "https://...", "roles": ["index", "data"], "canary": "https://..."},
    {"id": "node-c", "url": "https://...", "roles": ["index", "data"], "canary": "https://..."}
  ],
  "manifest_version": 1,
  "signed_at_block": 906412,
  "signature": "..."
}
```

Signed by the Legend release key (offline, not on any node). Adding node four
is a manifest update and a re-sign — no code changes required. The manifest is
served from `refueler.io/legend/manifest.json` and cached by the browser for
the session duration only. No persistent local storage.

---

## Section 4 — Cashu non-monetary credential architecture

### Separate Legend mint

Legend operates its own Cashu mint, entirely independent of Share's Cashu
infrastructure. The Legend mint is non-monetary. It issues query authorisation
tokens only — no ecash, no Lightning, no monetary value of any kind. This
distinction is stated clearly in all documentation and is legally meaningful.

### Primitives in use

**NUT-00 — Blind signatures (BDHKE):**
Core primitive. Client blinds a credential request; the mint signs the blind message without seeing the plaintext; client unblinds and holds a valid credential the mint cannot link to the issuance event. Applied at two points:

1. Credential issuance: the mint issues query authorisation tokens to the browser.
   The mint knows a batch was requested; it cannot link any individual token to a
   specific query or session.
2. Nested blinding across role-split stages: the browser re-blinds the routing token before stage 2, preventing the data node from linking stage 2 to stage 1 even in collusion with the mint.

**NUT-07 — Token state check:**
Allows credential validity verification without account state. A browser with
credentials from a previous session can verify they are still valid against the
current mint window without creating persistent identity.

**NUT-11 — P2PK spending conditions:**
Enterprise credentials are cryptographically bound to the Enterprise client's
public key. A leaked Enterprise credential is useless without the corresponding
private key. Prevents credential theft and replay attacks against Enterprise
endpoints.

**NUT-29 — Batched minting:**
The client requests multiple tokens in a single mint operation. Essential for the
breach scenario: a user pasting 47 addresses into the batch query modal receives
47 credentials in one round-trip, not 47 sequential requests. The mint learns only
that a batch of N was requested — not what any individual credential will authorise. For Enterprise, portfolio monitoring sessions (200+ addresses) are initialised with a single batch request.

### Rotating mint instances — monthly cadence

Legend does not use a single long-lived mint with rotating key sets. It uses
independent rotating mint *instances*:

- Each mint instance is fully independent with its own FROST-distributed keypair
  (see Section 5) and its own lifecycle.
- Each instance serves a fixed window: one calendar month.
- At window close, the FROST key shares on all three nodes are destroyed.
  No key material from the previous window survives on any hardware.
- Outstanding credentials issued by the expiring mint must be exchanged before
  window close or they expire. Exchange uses a fresh NUT-00 blind signature round
  against the new mint — the new mint never learns what credential was surrendered.

**Why rotating instances rather than rotating key sets:**

Forward secrecy at the mint level. A compromise of the current mint's key shares
reveals nothing about credentials issued by previous mints — those key shares no
longer exist. A legal order served on the current mint reaches only the current
window's issuance data. The non-monetary nature of the credentials means credential
exchange friction is low: a user who misses the exchange window loses rate-limit
allowance, not money.

**Credential exchange — unlinkability preserved:**

1. Browser presents expiring credential to exchange endpoint, blinded.
2. Incoming mint signs the blinded exchange request without seeing the credential.
3. Browser presents signed blind to new mint, which issues a new credential.
4. New mint sees only a blinded request — it cannot link it to the expiring credential or to any previous session.

**NUT-29 interaction with rotating mints:**

Batch requests issued near window close expire at window close. Enterprise onboarding documentation states this explicitly: credential batches should be requested with sufficient time remaining in the mint window. Enterprise monitoring sessions that span month boundaries require a credential refresh — this is documented in the contract and handled by the Enterprise client software, not the browser SPA.

### Self-hosting and the mint

Self-hosters run their own Legend instance. They run their own Cashu mint for their
own queries — no dependency on the Refueler-operated mint. The mint setup is included in the AI-assisted self-hosting guide. A self-hosted mint does not rotate unless the operator configures rotation — rotation is a multi-node FROST ceremony and a single-machine self-hosted instance will use a simpler single-key rotating-key-set approach instead.

---

## Section 5 — FROST threshold signatures

### What FROST provides

FROST (Flexible Round-Optimised Schnorr Threshold signatures) splits a signing key
across N parties such that any T of them must cooperate to produce a valid signature. The full key never exists on any single machine.

Legend uses FROST **3-of-4** across the four full-participant nodes (A, B, C, D) for:
1. Mint key management (monthly rotating instances)
2. Warrant canary signing

Node E holds no FROST share. It is a chain-only cold standby.

A node seizure yields one FROST key share — cryptographically insufficient to issue credentials, forge canaries, or retroactively sign anything. Three shares are required. Those three shares exist on hardware in three different legal jurisdictions. The sub-quorum floor is now three simultaneous node failures, compared to two under the original 2-of-3 scheme — a materially different risk profile.

**Why 3-of-4 over 2-of-4:** 2-of-4 allows any pair of nodes to sign, enlarging the collusion surface. 3-of-4 requires three of four nodes to cooperate, maintaining stronger collusion resistance while tolerating one offline node at DKG time without needing an extension window.

### Mint key management via FROST

At the start of each monthly mint window, the three nodes perform a FROST distributed key generation (DKG) ceremony:

1. All four full-participant nodes (A, B, C, D) must be online. Node E does not participate.
2. DKG produces four key shares — one per node — and a single public key.
3. No node holds the full private key at any point. The full key is never assembled.
4. Credential issuance requires any 3-of-4 nodes to cooperate in a threshold signing operation. A client request for credential issuance triggers a 3-of-4 signing round. The result is a single valid Schnorr signature.
5. At window close, all four key shares are deleted from node storage. The key is destroyed across all jurisdictions simultaneously.

**Node down at rotation time:** with four participants and a 3-of-4 threshold, one node may be offline at DKG time and the ceremony proceeds on the remaining three. A 24-hour extension window is scheduled only if two or more nodes are offline simultaneously. If nodes remain offline beyond the extension window, the rotation is treated as failed and requires manual intervention per the incident protocol (§4). The status page shows the rotation state honestly throughout.

**DKG ceremony prerequisites — clean conditions required:** all participating nodes must be below 60% CPU and 70% I/O utilisation at ceremony start. If the ceremony boundary arrives during an active exploit campaign (incident protocol §5), the query API is suspended or rate-limited before the ceremony begins. The DKG's node-to-node communication path runs on the private interface only (see Section 7) — it is not reachable from the public internet and cannot be starved by query-API attack traffic. If any node's intrusion cannot be affirmatively excluded at ceremony time, the emergency re-key procedure (incident protocol §4) is used in preference to the scheduled ceremony — the un-cleared node is excluded and a fresh DKG runs on the remaining nodes. Never fold an un-cleared node into fresh key material via the scheduled ceremony.

**Implementation:** `frost-secp256k1` Rust crate from the ZF FROST project
(Zcash Foundation). Production-quality, actively maintained.

### Canary signing via FROST

Each node's warrant canary is signed via the same FROST 3-of-4 scheme. A canary publication requires cooperation between any three of the four full-participant nodes. A seized node cannot forge a canary statement for itself. The Enterprise client paragraph:

*"Publishing a false canary requires cooperation between three nodes across three separate legal jurisdictions. A legal order served on one node produces one FROST share — insufficient to produce a valid canary signature. Two shares are insufficient. Three are required."*

---

## Section 6 — Warrant canary architecture

### One canary per node, independently meaningful

Three canaries, not one shared canary. Per-node canaries mean a legal order
targeting the Helsinki node kills only the Helsinki canary. Users and Enterprise
clients see exactly which node is affected and can reason about whether their
specific query routing was affected.

### Signed statement contents

Each canary statement contains:

```
Node:          node-b (Helsinki, FI)
Block height:  906412
Block hash:    000000000000000000029a4f...
Timestamp:     2026-09-01T00:00:00Z
Expires:       2026-09-02T00:00:00Z  (72 hours)
Declaration:   This node's operator and hosting provider have received no legal
               order compelling disclosure, logging, or modification of node
               behaviour as of block 906412.
Signature:     [FROST 2-of-3 Schnorr signature]
```

**Block hash as freshness oracle:** embedding the block hash of a recent block proves the statement was not pre-signed before that block existed. The Bitcoin chain itself provides the timestamp — no trusted time source required.

**72-hour expiry:** cadence is daily publication. A canary that expires every 72 hours and is published every 24 hours provides a 48-hour silence window before expiry is reached — long enough to distinguish a publication failure from a deliberate canary death, short enough to be meaningful.

**Silence reads as death — by design.** An expired canary is a dead canary. No grace periods. No ambiguity. A node down for maintenance kills its canary; the status page says the node is under maintenance; resurrection requires a fresh FROST-signed statement. Ambiguity is what canaries exist to eliminate.

### Publication — three independent channels

Each canary statement is published to three locations simultaneously:

1. **Static file on the node itself:** `https://[node-url]/canary.json` — served directly from the node, no Legend infrastructure intermediary. Fetching it reveals nothing about the fetcher beyond the fact that they fetched a public static file.

2. **GitHub repository mirror:** `rajesh-taylor/refueler-legend/canaries/[node-id]/latest.json` — public, immutable commit history, independent of Legend infrastructure.

3. **Nostr note from per-node npub:** each node has its own Nostr keypair. The canary is published as a Nostr event from the node's npub to a minimum of three relays simultaneously: `wss://relay.damus.io`, `wss://relay.nostr.band`, `wss://nostr.wine`. Publication to Nostr counts as delivered when ≥2 of the three relays confirm receipt. Verification via Nostr requires no contact with Legend infrastructure — a user can verify all five canaries through any Nostr relay without Legend knowing a verification occurred.

**Nostr relay redundancy:** three relays per node npub. A single relay outage does not fail the Nostr channel. The operator monitors relay health alongside node health. Relay rotation (replacing an unreliable relay) is a configuration change — it does not require a DKG ceremony or manifest update.

### Publication success definition

> A canary is **published** when it is FROST-signed and confirmed on **at least two channels including at least one off-node channel** (GitHub mirror or ≥1 Nostr relay confirmation). The node's own `canary.json` is a distribution channel, not an authority — it cannot vouch for the node's own status. A canary present only on `canary.json` is **not** counted as published.

This definition resolves the channel-disagreement ambiguity identified in the incident protocol adversarial review (Legend-7, Opus-B, Simulation 5). A single off-node channel outage (e.g. one Nostr relay unreachable) with `canary.json` + GitHub live is a **channel-delivery degradation**, not a canary expiry. The status page reflects this as a distinct state, separate from the amber EXPIRED state.

### Verification without query logs

A user or Enterprise client verifying canary status:
- Fetches `canary.json` directly from each node — static file, no log entry created
  beyond a standard HTTP access log (which nodes do not keep)
- Or reads the GitHub mirror
- Or queries any Nostr relay for events from the node npubs

No Legend-operated endpoint is involved in verification. The architecture makes
verification-without-logging structurally true, not a policy promise.

### The UK operator caveat — stated in full

All three nodes are operated by one person domiciled in London, UK. The
Investigatory Powers Act 2016 (IPA 2016) permits:

- Compelled disclosure of decryption keys and system access
- Non-disclosure orders (the operator cannot reveal a warrant has been served)
- Arguably, compelled continuation of canary publication while under a non-disclosure order — the legal position on this specific point is untested in UK courts

The per-node geographic distribution across Germany, Finland, and Iceland protects
against orders served on hosting providers in those jurisdictions. It does not protect against a UK IPA order served on the operator personally, which can affect all three nodes regardless of hardware location.

This caveat appears in:
- The privacy explainer modal (plain language, non-alarming register)
- The Enterprise contract materials (precise legal language)
- The `refueler.io/legend` below-the-fold "how this works" section

A product that overstates its legal protections is weaker than one that understates
them. The honest version is the stronger version with the clients Legend wants.

---

## Section 7 — Node hardening and security

### Baseline hardening (pre-launch, all three nodes)

- SSH key authentication only — password authentication disabled
- Non-standard SSH port
- `ufw` firewall: allowlist only — Bitcoin p2p (8333), Esplora API port, SSH
- `fail2ban` on SSH
- `unattended-upgrades` for security patches (OS packages only — Legend software
  updates are manual and verified)
- No public RPC exposure — Bitcoin Core RPC is localhost only
- No remote debug endpoints — no query logs exist to expose

### Dependency integrity

- Reproducible builds for the BLAKE3 electrs fork
- Pinned dependency hashes in `Cargo.lock` — no floating versions
- Build verification step before any node update: build hash compared against
  a signed release manifest before deployment

### Key material handling

**Mint FROST key shares:** generated during DKG ceremony, held in node memory
during the mint window, never written to disk unencrypted. At window close,
memory is zeroed and key shares are gone. If a node restarts mid-window, it
has lost its share and must signal the other nodes — the mint operates on
2-of-2 (degraded) until the next scheduled rotation.

**Cashu signing key never on the node in full:** FROST ensures the full key
never exists on any node. This is the architectural answer to the question
of mint key compromise. A node seizure does not yield a usable key.

**Argon2id for all key material at rest:**
Any key material written to disk (node identity keys, Nostr npub keys,
canary signing key shares between sessions) is encrypted at rest using
Argon2id key derivation. Parameters: memory cost ≥ 64MB, time cost ≥ 3,
parallelism set to available cores. Brute force of a seized disk is
computationally infeasible at these parameters.

**Canary signing keys:** FROST shares, Argon2id-encrypted at rest, published
daily. One FROST share per node; canary publication requires 2-of-3.

### Canary signing service — process isolation (implementation requirement)

Canary signing and the monthly DKG ceremony run as **separate systemd units**, independent of the query API:

- `legend-canary.service` — automated daily canary signing, node-to-node
- `legend-dkg.service` — monthly DKG ceremony
- `legend-api.service` — query API (public-facing)

**No systemd dependency chain** exists between the canary/DKG units and the API unit. `systemctl stop legend-api.service` must not affect canary or DKG operation — verified by test at provisioning time and after every software update.

**Network binding:** `legend-canary.service` and `legend-dkg.service` bind to the node-to-node private network interface only. `legend-api.service` binds to the public interface. The DKG and canary signing paths are unreachable from the public internet and cannot be starved by query-API attack traffic.

This is an **implementation requirement, not a preference**. The C/D canary distinction in the incident protocol (§1) — a suspended query API with living canaries is operational, not legal — depends entirely on canary signing surviving a full `legend-api.service` suspension. If the services share a process or failure domain, the C/D distinction collapses. Confirm the isolation holds under `systemctl stop legend-api.service` before declaring v1 ready.

### What an attacker gets from a compromised node

- Live query stream through that node for the period of compromise (index or
  data role, not both simultaneously — role rotates per query)
- One FROST key share (insufficient for anything without a second share)
- No historical query data (none stored)
- No other nodes' key shares (nodes do not communicate key material)

**If two nodes are simultaneously compromised:** an attacker who controls both
nodes in a specific query's role split sees the full query for that query.
This is the ceiling of the v1 threat model. v2 Spiral PIR makes this irrelevant.
The status page and privacy explainer document this ceiling honestly.

**Compromised node detection:** canary failure is the primary signal. If a node
stops publishing canaries, something has happened to it. The status page shows
canary health in real time. Enterprise clients are notified directly of any
canary failure.

### Anomaly detection — aggregate compute counter

Standard anomaly detection runs on content-blind request metadata (rate, error class, malformed-request ratio, credential-issuance rate, connection churn, CPU/IO saturation) per the incident protocol §5.

**Additional counter — expensive-endpoint compute (content-blind):** full-range Silent Payments scans and 50-address batch queries are metered by aggregate core-seconds consumed per rolling 10-minute window, service-wide. This counter detects slow, distributed harvests of costly endpoints that produce zero mutation signal and therefore evade the standard §5 detection model. It records no query content, no identity, no IP — only that N core-seconds of expensive-endpoint work occurred in the window. Threshold breach triggers the same degrade/suspend decision tree as a mutation-rate spike.

---

## Section 8 — Transport security and post-quantum roadmap

### v1 transport

- Standard HTTPS (TLS 1.3) for free-tier queries
- Client IP visible to contacted nodes over HTTPS
- Tor hidden service for Enterprise endpoint only
- Free-tier users who want IP privacy use Tor Browser or route through their own
  Tor instance — this is documented honestly and not hidden

### Enterprise transport — Signal-style forward secrecy (v2)

The Double Ratchet algorithm (as used in Signal) over Curve25519 keys provides:
- Forward secrecy: compromise of today's key does not expose yesterday's sessions
- Break-in recovery: the ratchet heals after a key compromise

Applied to the Enterprise API: an initial X3DH handshake establishes a shared secret between the Enterprise client software and the Legend Enterprise endpoint. The ratchet rotates per query. A node seizure or key compromise after the fact cannot decrypt historical query content.

Implementation: `libsignal` (Apache 2.0, Signal Foundation), Rust crate.
Target: Enterprise tier, v2.

### Post-quantum transport — ML-KEM hybrid (v2, Enterprise)

The harvest-now-decrypt-later threat is real for Enterprise clients. A state-level
actor recording encrypted query traffic today can decrypt it when quantum capability arrives. For clients who need to make a credible long-term privacy argument to their legal team, this matters now even though quantum decryption is not imminent.

**ML-KEM-768** (NIST FIPS 203, August 2024, formerly KYBER) as a hybrid with X25519:
- X25519 for current security (well-understood, fast)
- ML-KEM-768 for post-quantum security (quantum-resistant key encapsulation)
- Hybrid: both must be broken to compromise the key exchange

Applied to: the Enterprise API endpoint transport layer.
Not applied to: the NUT-00 credential layer — post-quantum blind signatures are
a research-level problem, not a production-ready one. Documented as v3+ consideration.

Implementation: `ml-kem` Rust crate. Target: Enterprise tier, v2.

The sentence an Enterprise client's legal team needs:
*"Query transport uses a hybrid X25519 + ML-KEM-768 key exchange. A quantum computer breaking elliptic curve keys cannot decrypt recorded traffic because the ML-KEM component is quantum-resistant. Both components must be broken simultaneously."*

---

## Section 9 — Redundancy and failover

### N-1 (three nodes operational — one offline)

Fully functional. Role-split operates across three nodes. FROST 3-of-4 ceremony can proceed with three participants — no extension window needed. Privacy mode: **Reduced splitting — one node offline.**

### N-2 (two nodes operational — two offline)

Role-split operates with two nodes; collusion resistance drops to "the only two." FROST 3-of-4 signing continues: two surviving share-holders plus Node D (if available) or remaining participant. Privacy mode: **Reduced splitting — two nodes offline.** If the two offline nodes share a provider, the status page notes the reduced jurisdiction count.

Mint credential issuance: degraded but operational while ≥3 share-holders are available. If exactly two share-holders remain, credential issuance halts — canary signing continues on two shares because the DKG already produced signatures valid under the 3-of-4 scheme. Note: a 2-of-remaining-share canary requires implementation clarification — confirm against the FROST scheme that two participants can produce a valid canary under a 3-of-4 key before quoting this guarantee.

### N-3 (one node operational — sub-quorum)

**Sub-quorum state.** Fewer than three operational FROST share-holders. No mint round, no new canary signing, and no §7 notice signing is possible. This is an implementation requirement to be surfaced in the incident protocol §3D.

The operator's single priority at sub-quorum: restore a second and third share-holder before the earliest live canary reaches its 72-hour expiry. The monitor must surface **per-canary time-to-expiry** continuously so the operator can read that deadline in real time.

With five nodes (four full participants + one cold standby), sub-quorum requires three full-participant nodes to fail simultaneously — a materially less likely compound event than under the original three-node topology, where sub-quorum required only two simultaneous failures.

**Node E activation at sub-quorum:** if sub-quorum is reached, Node E's Esplora index build begins immediately. Node E already holds a fully synced Bitcoin Knots chain, reducing provisioning from 2–7 days (chain sync) to hours–1 day (index build only). Node E joins the manifest as the new Node D once its index is verified.

The browser SPA shows a plain notice, never hidden:
*"Operating on a single node. Query splitting is unavailable — this node can see
your full query. Consider using Tor for additional IP privacy."*

Never silently degrade the privacy claim. The notice is honest and non-alarming —
it states the limitation and offers the available mitigation.

### Node down mid-query

No server-side state exists. A failed stage-1 or stage-2 request is retried against
a remaining node — the retry is a fresh stateless request. Failover cannot create
session persistence because there are no sessions to persist. The browser handles
the retry transparently; the user sees a brief delay, not an error.

### Status page — what it shows

The node status page at `refueler.io/legend/status` shows per node:
- Reachability (live / unreachable)
- Bitcoin chain sync height
- Canary status: live (with last signed block) / expired (with timestamp of expiry)
- Current privacy mode: Full splitting / Reduced splitting / Single node

The status page does not show:
- Query volumes (no logs exist to draw them from)
- Traffic graphs
- Any information that would reveal query patterns

---

## Section 10 — Enterprise isolation

### Decision: v2

There are no Enterprise clients at v1 launch. Dedicated hardware for an empty tier
is operational overhead with no corresponding revenue. The signed node manifest
architecture means a dedicated Enterprise node slots in with a manifest update
— no structural code changes.

**Contract language for Enterprise isolation** is designed in Legend-2 even though
the hardware ships at v2. Enterprise clients contracted at v1 use the shared
infrastructure with Tor transport and FROST-protected credentials. Dedicated node
isolation is a v2 upgrade offered to existing Enterprise clients.

---

## Carry-forward

**Infrastructure cost figure is stale and must be updated in `legend-economics.md`.**
Original estimate: €170–220/month (three nodes). Five-node topology: approximately **€350–550/month** at current dedicated-server pricing, pending confirmed quotes for Nodes B, D, and E. One Enterprise contract continues to cover years of infrastructure — the economics claim holds — but the honest five-node figure replaces all earlier estimates in every document that carries a cost figure.

**Provider selection session required before provisioning.** Nodes B, D, and E are TBD. The session confirms provider, jurisdiction, and spec for each, verifying the no-shared-parent and jurisdiction-independence criteria. Output: a completed locations table with no TBD entries.

**FROST 3-of-4 implementation confirmation required.** The `frost-secp256k1` crate supports configurable thresholds. Confirm 3-of-4 is supported and test the ceremony before any node goes live with query traffic.

**DKG ceremony duration — measure and record.** The figure "minutes" appears in the incident protocol and Enterprise SLA materials. Replace with a measured figure from the first test ceremony before any Enterprise contract is signed.

**Index rebuild time — measure and record.** The "hours to ~1 day" snapshot restoration figure in the incident protocol is contingent on trustless index rebuild from a PoW-validated chain. Measure on matching hardware and replace the estimate with a real figure.

**`legend-economics.md` update required** — five-node cost model, revised break-even, revised monthly and annual infrastructure figures.

---

*"Nothing stops this train."*
