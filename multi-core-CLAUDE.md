# CLAUDE.md — refueler-multi-core
> **Version:** 1.2 | **Initialised:** CC-64 · 8 July 2026 | **Updated:** Multi-4 · 3 Aug 2026
> Load alongside `multi-core-SESSIONS.md` at the start of every session on this repo.
> For platform-wide context, load `REFUELER-BRIDGE.md` which lives in this repo root.

---

## What this repo is

`refueler-multi-core` has two complementary scopes:

**1. BLAKE3-accelerated Esplora fork (original scope)**
A fork of esplora-electrs optimised for ARM architecture and low-power hardware
(Raspberry Pi, Umbrel, StartOS). BLAKE3 replaces SHA-256 for internal RocksDB
index lookups only — Bitcoin consensus hashing is untouched. Enables multi-core
parallel indexing on ARM SIMD/NEON hardware, cutting sync times and RAM pressure.

**2. Legend — privacy-first Bitcoin block explorer (AP-7 scope)**
A privacy layer built on top of the BLAKE3 Esplora foundation. Legend adds
Cashu blind-signature query credentials, ephemeral sessions, PIR-inspired
query sharding, Silent Payments (BIP-352) native support, Tor-native API,
and proof-of-query receipts. Lives at `refueler.io/legend` post-B9.

The two scopes are not in conflict. The ARM performance work is the engine.
Legend is the privacy interface built on top of it.

**Local path:** `/Users/rajeshtaylor/Documents/refueler-multi-core/`
**GitHub:** `rajesh-taylor/refueler-multi-core` (public)
**Licence:** MIT (matches upstream esplora/electrs — fork compatibility)

---

## Upstream lineage

| Project | Licence | Role |
|---------|---------|------|
| electrs | MIT | Electrum-compatible Bitcoin indexer |
| esplora | MIT | Block explorer and API layer |
| refueler-multi-core | MIT | BLAKE3-accelerated ARM fork + Legend privacy layer |

---

## Locked decisions

**BLAKE3 scope (original):**
- BLAKE3 replaces SHA256 for internal RocksDB index lookup keys and cache tag generation only
- Bitcoin consensus hashing (SHA-256d) remains 100% unmodified throughout
- ARM/Raspberry Pi is the primary deployment target for the indexer
- Must not break compatibility with existing Electrum protocol clients

**Legend scope (AP-7):**
- Privacy-first query architecture — no session persistence, no client correlation
- Cashu query credentials — same blind-signature infrastructure as Refueler Share
- PIR-inspired sharding across 3–5 nodes — no single node sees the complete query
- Silent Payments (BIP-352) native support — first explorer to do this correctly
- Tor-native API for Enterprise tier
- Open source — "don't trust us, read the code" is the only honest privacy claim
- Licence stays MIT to match upstream — Apache 2.0 is for Share, not this repo
- Do not start Legend build before Share Lightning node is live at B9

**Query credit model (locked Multi-4):**
- Free tier: unlimited queries at v1 launch. No account. No rate limit. No friction at distress moment.
- Paid Share tiers: Share discovery drives Legend awareness — no query gate on Legend
- Enterprise: unlimited, PIR-sharded, Tor-native

---

## Build sequence

1. B9 — Share Lightning node live (prerequisite, non-negotiable)
2. B9-plan — Legend planning session before any code
3. Post-B9 — Esplora fork, Paper/Carbon design pass, Cashu credential gating
4. Post-B9 — PIR sharding layer, Tor API, Silent Payments scanner
5. Post-B9 — Family office reporting, Lightning channel correlation
6. Post-B9 — `refueler.io/legend` live, article 14 published

---

## Session queue

See `multi-core-SESSIONS.md`.

---

## Key article

**Article 14** (unlocks post-B9): "The metadata leak nobody talks about — what
querying a public block explorer tells the world."
Opening line locked: "Every time you look up a Bitcoin address on a public block
explorer, you're telling that server exactly what you own and what you're watching.
Here's what we built instead, and why it matters for our clients."

---

*"Nothing stops this train."*
