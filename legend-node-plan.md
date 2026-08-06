# legend-node-plan.md — refueler-legend
> **Version:** 1.0 | **Created:** Legend-0 · 5 Aug 2026
> Node infrastructure topology for Legend — privacy-first Bitcoin block explorer.
> Infrastructure section only. Economics section appended in Legend-1.
> Load in every build and infrastructure session.
> Companion documents: `legend-design-spec.md`, `legend-scope.md`, `CLAUDE.md`, `SESSIONS.md`.

---

## Section 1 — Node count and locations

### Three nodes. Locked.

Two is the mathematical minimum for role-split query sharding but leaves zero redundancy margin. Three gives role rotation per query, N-1 graceful degradation, and three independent warrant canaries across two legal jurisdictions.

Five nodes is v2/v3 territory, added when Enterprise isolation requires dedicated
hardware. The signed node manifest architecture (see Section 3) means adding a node
requires a manifest update, not a code change.

### Locations

| Node | Provider | Datacentre | Jurisdiction |
|---|---|---|---|
| Node A | Hetzner | Falkenstein, DE | Germany (EU) |
| Node B | Hetzner | Helsinki, FI | Finland (EU) |
| Node C | FlokiNET | Reykjavik, IS | Iceland (EEA, non-EU) |

**Why this combination:**

Hetzner provides operational reliability and cost efficiency. Two Hetzner nodes are
acceptable because the FlokiNET Iceland node sits outside the EU legal-assistance
fast lane. Iceland is EEA but not EU — the MLAT chain from a UK or US order to
Icelandic infrastructure runs through a different legal pathway than a request
to EU-domiciled providers.

**The honest caveat that must appear in all user-facing materials:**

All three nodes are operated by one person in London. The UK Investigatory Powers Act 2016 can compel disclosure and arguably compel continued canary publication against the operator regardless of where hardware is located. Per-node geographic distribution protects against orders served on the *hosting providers* more robustly than orders served on the operator personally. The canary architecture is strongest as a signal about hosting-provider orders; it is limited as a signal about operator-level compulsion. This distinction is stated plainly in the privacy explainer modal and in Enterprise ontract materials. A product that overstates its legal protections is weaker than on that understates them.

**The one-provider problem:**

Three Hetzner nodes would be one company. A single order served on Hetzner GmbH
reaches Falkenstein, Helsinki, and any other Hetzner location simultaneously.
Geographic distribution within one provider buys latency and hardware redundancy —
it buys nothing for MLAT purposes. FlokiNET Iceland resolves this. The honesty cost
of a third Hetzner node is too high for a product built on the honest version being
stronger than the marketing version.

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

Legend uses FROST 2-of-3 across the three nodes for:
1. Mint key management (monthly rotating instances)
2. Warrant canary signing

A node seizure yields one FROST key share — cryptographically insufficient to issue
credentials, forge canaries, or retroactively sign anything. Two shares are required. Those two shares exist on hardware in two different legal jurisdictions.

### Mint key management via FROST

At the start of each monthly mint window, the three nodes perform a FROST distributed key generation (DKG) ceremony:

1. All three nodes must be online.
2. DKG produces three key shares — one per node — and a single public key.
3. No node holds the full private key at any point. The full key is never assembled.
4. Credential issuance requires any 2-of-3 nodes to cooperate in a threshold signing operation. A client request for credential issuance triggers a 2-of-3 signing round between two available nodes. The result is a single valid Schnorr signature.
5. At window close, all three key shares are deleted from node storage. The key is
   destroyed across all jurisdictions simultaneously.

**Node down at rotation time:** if one node is offline at the monthly DKG ceremony,
the ceremony cannot proceed with the remaining two (DKG requires all participants).
The ops runbook covers this: a 24-hour ceremony window is scheduled; if a node
remains offline beyond the window, it is treated as a failed rotation requiring
manual intervention. The previous mint window is extended by 24 hours maximum;
the status page shows the rotation state honestly.

**Implementation:** `frost-secp256k1` Rust crate from the ZF FROST project
(Zcash Foundation). Production-quality, actively maintained.

### Canary signing via FROST

Each node's warrant canary is signed via the same FROST 2-of-3 scheme. A canary
publication requires cooperation between any two nodes. A seized node cannot forge
a canary statement for itself. The Enterprise client paragraph:

*"Publishing a false canary requires cooperation between two nodes in two separate
legal jurisdictions. A legal order served on one node produces one FROST share —
insufficient to produce a valid canary signature."*

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

1. **Static file on the node itself:** `https://[node-url]/canary.json` — served
   directly from the node, no Legend infrastructure intermediary. Fetching it reveals nothing about the fetcher beyond the fact that they fetched a public static file.

2. **GitHub repository mirror:** `rajesh-taylor/refueler-legend/canaries/[node-id]/latest.json`
   — public, immutable commit history, independent of Legend infrastructure.

3. **Nostr note from per-node npub:** each node has its own Nostr keypair. The canary is published as a Nostr event from the node's npub. Verification via Nostr requires no contact with Legend infrastructure whatsoever — a user can verify all three canaries through any Nostr relay without Legend knowing a verification occurred.

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

### N-1 (two nodes operational)

Fully functional. Role-split operates with two nodes. Collusion resistance drops
from "any two of three" to "the only two." The status page shows the reduced node
count. Privacy mode displayed as "Reduced splitting — one node offline."

Mint credential issuance operates in degraded 2-of-2 FROST mode if one node is down.

### N-2 (one node operational)

Functional, but role-split is gone. One node sees the full query.

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

## Carry-forward — infrastructure cost

All cost figures are confirmed and documented in `legend-economics.md` §1.
**Do not restate figures here — refer to that document.**
Current confirmed range: €340–420/month midpoint ~€360/month (verified August 2026).
The "one Enterprise client covers years of infrastructure" claim is confirmed and understated — see `legend-economics.md` §4.

**Legend-4** adds `legend-incident-protocol.md`: node recovery procedures, provider-switch protocol, FROST re-keying after topology changes, and attack vector simulations (compelled shutdown, jurisdiction-level enforcement, DDoS against the role-split API). Two Opus sessions — production then review. Quarterly revision cadence begins at v1 launch.

---

*"Nothing stops this train."*
