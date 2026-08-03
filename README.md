# refueler-legend

> A BLAKE3-accelerated fork of esplora-electrs — the indexing engine underpinning **Legend**, a privacy-first Bitcoin block explorer.

**Legend at:** [refueler.io/legend](https://refueler.io/legend) *(post-B9)*
**Upstream:** [blockstream/esplora-electrs](https://github.com/Blockstream/electrs)
**Part of the [Refueler](https://refueler.io) ecosystem**

---

## Consensus & Security Notice

> **This fork maintains 100% Bitcoin network consensus.**
> BLAKE3 is used exclusively for **internal local disk index lookups** within RocksDB.
> It does not touch, modify, or replace any Bitcoin consensus-critical hashing.
> SHA-256d remains unchanged throughout all block validation, transaction verification,
> and network protocol layers.

---

## What This Is

This repo has two complementary scopes:

**1. BLAKE3-accelerated Esplora fork**
Standard `esplora-electrs` indexes blockchain data on a single CPU core. On ARM hardware
this produces slow initial sync and RAM exhaustion under mempool spikes. This fork replaces
internal RocksDB index lookup key hashing with BLAKE3, enabling multi-core parallel indexing
via ARM SIMD/NEON. Bitcoin consensus hashing is untouched.

**2. Legend — privacy-first Bitcoin block explorer**
A privacy layer built on top of the BLAKE3 Esplora foundation. Adds Cashu blind-signature
query credentials, ephemeral sessions, PIR-inspired query sharding, Silent Payments (BIP-352)
native support, Tor-native API, and proof-of-query receipts. Lives at `refueler.io/legend`.

The two scopes are not in conflict. The ARM performance work is the engine. Legend is the
privacy interface built on top of it.

---

## What Changes from Upstream

| Layer | Standard Esplora | This Fork |
|-------|-----------------|-----------|
| Bitcoin consensus hashing | SHA-256d | SHA-256d (unchanged) |
| RocksDB internal index keys | SHA-256 | **BLAKE3** |
| Cache tag generation | SHA-256 | **BLAKE3** |
| CPU core utilisation | Single-threaded | **Multi-core parallel** |
| RAM under mempool spike | Vulnerable | **Lightweight caching layer** |

---

## Legend — Privacy-First Bitcoin Block Explorer

> Built on this engine. The indexer is the foundation. Legend is the interface.

*"Chainalysis works for the observer. Legend works for the owner."*

### The problem

Every query to a public block explorer tells that server exactly which addresses and
transactions you're watching. Mempool.space and Blockstream.info log every request by
design. For a family office monitoring significant holdings, a lawyer verifying a client's
payment, or a Bitcoiner checking whether a breach has swept their addresses, querying a
public explorer compounds the very privacy risk they're trying to assess.

No existing explorer is architected to prevent this. Legend is.

### What Legend adds

**Cashu query credentials** — query budgets issued as Cashu blind-signature tokens.
The server cannot reconstruct a client's query history across sessions. Structurally
impossible, not a policy promise.

**Ephemeral sessions** — no cookies, no session tokens, no client correlation across
queries. Every request is unlinkable from the last.

**PIR-inspired sharding** — queries split across 3–5 nodes. No single node sees the
complete query. The client reassembles locally.

**Tor-native API (Enterprise)** — client IP never reaches the server.

**Silent Payments native (BIP-352)** — first explorer to display Silent Payments static
addresses and their derived outputs correctly. Requires scanning every block; public
explorers do not support this.

**Proof-of-query receipts** — a blind cryptographic receipt per query. Clients can
demonstrate their query behaviour is unloggable without revealing what they asked.

### Who Legend serves

- **Plebs** — free tier, no account, unlimited queries. Privacy as default.
- **Lightning and Cashu wallet users** — private channel activity tracking. API-compatible
  with Sparrow Wallet's custom Esplora endpoint out of the box.
- **Family offices** — private address monitoring, UTXO provenance scoring, compliance
  reporting. All queries private to the client.
- **Enterprise** — unlimited, PIR-sharded, Tor-native, Silent Payments scanning,
  self-hosting option with support contract.

### Business model

Free at point of use. No account. No rate limit. No friction at the distress moment — ever.

Enterprise contracts are the revenue source: dedicated nodes, Tor API, Silent Payments
scanning, compliance reporting, SLA, quarterly security review. The software is free and
open source. One Enterprise client covers years of infrastructure (~€96/month).

---

## Build Status

**Phase 1 — Working explorer by December (target)**

| Item | Status |
|------|--------|
| Architecture locked | ✓ Multi-3 |
| UI/UX design spec | ✓ Multi-4 |
| Product scope locked | ✓ Multi-5 |
| Brand pass | ✓ Multi-6 |
| Eleventy shell + SPA mount | ✓ Multi-7 |
| First query flow | Multi-8 |
| Silent Payments scanner | Phase 1 |
| Batch address flow | Phase 1 |
| Article 14 | Phase 1 |

**Prerequisite:** Share Lightning node live at B9. No Legend code ships before B9.

---

## Deployment

- **[Start9](https://start9.com)** — via `s9pk` package
- **[Umbrel](https://umbrel.com)** — via app manifest
- **Direct** — standard Rust build on any ARM64 or x86_64 Linux host
- **Hetzner** (Legend production) — dedicated VPS instances for PIR sharding nodes

Installation and build instructions: *(active development post-B9)*

---

## Ecosystem Position

```
[ refueler.io ]
       (Commerce platform — merchant POS, editorial, Legend explorer)
                     |
      +--------------+------------------+
      |              |                  |
(This repo)    [share.refueler.io]  [mint.refueler.io]
BLAKE3 indexer  Encrypted P2P        Cashu mint
+ Legend        file transfer
privacy layer
```

---

## Licence

MIT — matches upstream Esplora/electrs for clean fork compatibility.
