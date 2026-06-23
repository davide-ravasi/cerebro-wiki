---
id: concept-snapshot-isolation-mvcc
title: "Snapshot Isolation (MVCC)"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, isolation, mvcc, concurrency]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**Snapshot isolation** gives each transaction a **consistent snapshot** of the database as of transaction start. The engine keeps **multiple versions** of data (**MVCC** — Multi-Version Concurrency Control) so readers see an old version while writers create new ones.

# Why It Matters

Fixes **read skew** (seeing half-old, half-new state within one transaction) without readers blocking writers. Default or common choice in PostgreSQL, Oracle, MongoDB multi-doc transactions, and many systems — but **not equivalent to full serializability**.

# How It Works

- Transaction begins → assigned snapshot timestamp or version.
- Reads return data visible at that snapshot.
- Writes create new versions; commit makes them visible to later snapshots.
- Conflicting writes may still occur; rules vary by engine.

# Tradeoffs

- **Pros:** high read concurrency, predictable per-transaction view.
- **Cons:** does not alone prevent **[[concept-write-skew]]** or all **[[concept-phantom-read]]** cases; storage overhead for versions.

# When To Use

Default for many OLTP workloads when anomalies are understood and stronger guarantees are not required. Step up to **[[concept-serializable-snapshot-isolation]]** or **[[concept-two-phase-locking]]** when invariants demand serializability.

# Example

Long report transaction sees account balances frozen at start time while payments commit concurrently — consistent snapshot, no dirty reads.

# Related Concepts

- [[concept-write-skew]]
- [[concept-phantom-read]]
- [[concept-serializable-snapshot-isolation]]
- [[map-databases]]
