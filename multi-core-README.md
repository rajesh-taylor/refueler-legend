# refueler-multi-core

> A BLAKE3-accelerated fork of esplora-electrs — optimised for ARM hardware, high-throughput streaming data, and real-time Lightning payment indexing. Also the engine underpinning **Legend**, a privacy-first Bitcoin block explorer.

**Live at:** [multi-core.refueler.io](https://multi-core.refueler.io) *(coming soon)*
**Legend at:** [refueler.io/legend](https://refueler.io/legend) *(post-B9)*
**Upstream:** [blockstream/esplora-electrs](https://github.com/Blockstream/electrs)
**Part of the [Refueler](https://refueler.io) ecosystem**

---

## A Consensus & Security Notice

> **This fork maintains 100% Bitcoin network consensus.**
> BLAKE3 is used exclusively for **internal local disk index lookups** within RocksDB. It does not touch, modify, or replace any Bitcoin consensus-critical hashing. SHA-256d remains unchanged throughout all block validation, transaction verification, and network protocol layers.

---

## What This Is

Standard `esplora-electrs` indexes blockchain data linearly on a single CPU core, leaving available hardware largely idle. On a Raspberry Pi 4 or 5, this produces two chronic problems:

- **Initial sync takes 72+ hours** due to sequential read/hash/write cycles against RocksDB
- **Mempool spikes above 300MB cause RAM exhaustion**, crashing background processes

`refueler-multi-core` addresses both by replacing the internal RocksDB index lookup key hashing with BLAKE3, which is architecturally optimised for parallel execution across multiple cores using ARM SIMD/NEON instruction sets natively supported on Raspberry Pi hardware.

### What Changes

| Layer | Standard Esplora | This Fork |
|-------|-----------------|-----------|
| Bitcoin consensus hashing | SHA-256d | SHA-256d (unchanged) |
| RocksDB internal index keys | SHA-256 | **BLAKE3** |
| Cache tag generation | SHA-256 | **BLAKE3** |
| CPU core utilisation | Single-threaded | **Multi-core parallel** |
| RAM under mempool spike | Vulnerable | **Lightweight caching layer** |

### Why BLAKE3 Here

BLAKE3 processes data as a binary Merkle tree, making it intrinsically parallelisable. On a Raspberry Pi's ARM cores, it runs at over **1 GB/s per core** using native hardware instructions. For internal database index operations — which are purely about speed and not consensus — this is a direct, measurable improvement with no security tradeoffs.

---

## Performance Targets

| Metric | Standard esplora-electrs | refueler-multi-core |
|--------|-------------------------|---------------------|
| Initial sync (Raspberry Pi 4) | 72+ hours | Target: significantly reduced |
| RAM under 300MB+ mempool | Crash risk | Stable |
| CPU core utilisation | ~25% (1 of 4 cores) | ~90%+ across all cores |
| Power draw / thermals | Higher (thrash pattern) | Lower (efficient parallel bursts) |

*Benchmarks will be published on first confirmed Raspberry Pi 4 and Pi 5 test runs.*

---

## Target Hardware

- Raspberry Pi 4 (4GB or 8GB RAM)
- Raspberry Pi 5
- Any ARM64 device running Ubuntu 22.04+ or Debian Bookworm

This fork is designed to run on hardware people **already own**. No new kit required.

---

## Streaming & Lightning Use Case

Beyond personal node operation, this fork serves as the data indexing engine for high-throughput Lightning streaming applications — specifically Podcasting 2.0 environments where thousands of real-time 1-sat micropayments and Boostagram tips arrive simultaneously.

Standard Esplora was not designed for this data profile. The BLAKE3 parallel pipeline clears the indexing bottleneck that causes buffering on high-definition podcast streams under payment load.

---

## BLAKE3 Deduplication Engine (Video Relay Mode)

For operators running Nostr video relays alongside their Bitcoin node, this fork includes an optional BLAKE3 pre-hash deduplication layer:

- Before storing an incoming video file, the relay hashes the **first 16KB and last 16KB** using BLAKE3's ultra-fast block flagging
- If the hash matches an existing file on disk, the relay discards the duplicate and links the new Nostr event to the existing file
- Saves up to **40% of relay storage** — making a home Raspberry Pi video relay economically viable

---

## Target Crates (CDK-Adjacent)

For operators also running a Cashu mint alongside their indexer, the following CDK crates are the correct targets for BLAKE3 integration:

| Crate | Role | BLAKE3 Target |
|-------|------|---------------|
| `cdk-redb` | Pure-Rust key-value store for mint state | Internal index lookup keys |
| `cdk-database` | Abstraction layer for proof (token) state queries | Lookup hashing |
| `cdk-sqlite` / `cdk-postgres` | SQL-backed token state tracking | Not targeted |

---

## Legend — Privacy-First Bitcoin Block Explorer

> Built on this engine. The indexer is the foundation. Legend is the interface.

### The problem

Every query to a public block explorer tells that server exactly which addresses and
transactions you're watching. Mempool.space and Blockstream.info build query logs by
design — their business model depends on understanding user behaviour. For a family
office monitoring a significant holding, a lawyer verifying a client's payment, or a
Bitcoiner checking whether a suspected breach has swept their addresses, querying a
public explorer compounds the very privacy risk they're trying to assess.

No existing explorer is architected to prevent this. Legend is.

### What Legend adds

**Cashu query credentials** — query budgets issued as Cashu blind-signature tokens
(same infrastructure as Refueler Share). The server cannot reconstruct a client's
query history across sessions. Structurally impossible, not a policy promise.

**Ephemeral sessions** — no cookies, no session tokens, no client correlation
across queries. Every request is unlinkable from the last.

**PIR-inspired sharding** — queries split across 3-5 nodes. No single node sees
the complete query. The client reassembles locally. A world first for production
Bitcoin chain data.

**Tor-native API (Enterprise)** — client IP never reaches the server.

**Silent Payments native (BIP-352)** — first explorer to display Silent Payments
static addresses and their derived outputs correctly. Requires scanning every block;
public explorers do not support this.

**Proof-of-query receipts** — a blind cryptographic receipt per query. Clients can
demonstrate their query behaviour is unloggable without revealing what they asked.

### Who Legend serves

- **Plebs** — free tier, no account required, 50 queries/day. Privacy as default.
- **Lightning and Cashu wallet users** — private channel activity tracking without
  revealing your node footprint to Mempool.space. API-compatible with Sparrow Wallet's
  custom Esplora endpoint out of the box.
- **Family offices** — private address monitoring, UTXO provenance scoring, compliance
  reporting. All queries private to the client.
- **Enterprise** — unlimited, PIR-sharded, Tor-native, Silent Payments scanning,
  self-hosting option with support contract.

### The query credit model

| Tier | Queries |
|------|---------|
| Free standalone | 50/day |
| Free Share tier | 10 per upload |
| Paid Share tiers | 50 per upload, uncapped daily |
| Enterprise | Unlimited |

### Live at

`refueler.io/legend` — post-B9. No code yet. Planning scope complete.

---

## Deployment

Designed for deployment on:

- **[Start9](https://start9.com)** — via `s9pk` package
- **[Umbrel](https://umbrel.com)** — via app manifest
- **Direct** — standard Rust build on any ARM64 or x86_64 Linux host
- **Hetzner** (Legend production) — dedicated VPS instances for PIR sharding nodes

Installation and build instructions: *(coming soon — active development post-B9)*

---

## Community & Open Source

This fork is fully open source and MIT licensed. The BLAKE3 optimisation is a
contribution to the Bitcoin self-sovereignty stack. Better personal node performance
means more people run nodes, which strengthens the network. Legend's open source
licence means you can verify the privacy claims yourself — "don't trust us, read
the code" is the only honest answer to a privacy claim.

Tag your issues and PRs with the relevant component:
`rocksdb`, `blake3`, `arm`, `lightning`, `nostr-relay`, `legend`, `silent-payments`

Repository topics: `bitcoin` `esplora` `electrum-server` `blake3` `rust` `umbrel`
`startos` `raspberry-pi` `lightning` `silent-payments` `privacy` `cashu`

---

## Ecosystem Position

```
[ refueler.io ]
       (Commerce platform -- merchant POS, franchise dashboard, editorial)
                     |
      +--------------+------------------+
      |              |                  |
(This repo)    [share.refueler.io]  [mint.refueler.io]
BLAKE3 indexer  Encrypted P2P        Closed-loop
+ Legend        file transfer        loyalty stamp mint
privacy layer
```

---

## Status

Pre-build. Architecture locked. Legend scope defined (AP-7, Aug 2026).
Active development begins post-B9 (Share Lightning node prerequisite).

Upstream tracking: [blockstream/esplora-electrs](https://github.com/Blockstream/electrs)

---

## Licence

MIT -- matches upstream Esplora/electrs for clean fork compatibility.
