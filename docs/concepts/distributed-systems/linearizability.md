---
id: concept-linearizability
title: "Linearizability"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-09
tags: [concept, distributed-systems, consistency, replication, concurrency]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**Linearizability** is a recency guarantee on single-object operations: the system behaves as if there is **one copy** of the data and all reads/writes happen in **some serial order** that respects **real time**. If operation A **completes** before operation B **starts**, then A appears before B in that order.

# Why It Matters

Users expect it after successful writes: create user → immediate read finds the user; acquire lock → others see the lock; compare-and-set must reflect recent state. **Stale reads** after a completed write feel like data loss or broken coordination. Locks, leader election, and uniqueness constraints typically require linearizable registers.

# How It Works

Each operation has a start and end time. A history is linearizable if you can assign each operation a **linearization point** (between its start and end) such that:

1. Serial order matches register semantics (reads return the latest preceding write in that order).
2. If A ended before B started in real time, A precedes B in the serial order.

**Overlapping operations:** if neither finished before the other started, order may be flexible (e.g. concurrent read during write may return old or new value — both can be valid). **Not overlapping:** if write W completes before read R starts, R **must** see W.

**Who enforces it:** not the client — a **shared mechanism** (single leader, quorum reads/writes, consensus log, coordination service).

# Tradeoffs

- **Pros:** intuitive correctness for coordination and “read your latest successful write” semantics.
- **Cons:** expensive in distributed systems (coordination, latency, availability under partition); often limited to specific operations or services (ZooKeeper, etcd), not whole eventually-consistent stores.

# When To Use

- **Use** for locks, leader election, membership, uniqueness checks, CAS registers.
- **Avoid** assuming it on every read from async replicas or eventually consistent systems without explicit API guarantees.

# Example

`POST /users` returns 201; `GET /users/alice` from a **lagging replica** returns 404 — not linearizable. Fix: read from primary / `readConcern: "majority"` (MongoDB) or a linearizable coordination service.

**Whiteboard analogy:** Alice finishes writing “booked”; Bob looks five minutes later and still sees “free” — the shared board failed the guarantee.

# Related Concepts

- [[concept-quorum-majority-truth]]
- [[concept-replication]]
- [[concept-serializable-snapshot-isolation]]
- [[concept-last-write-wins-timestamp-risk]]
- [[map-distributed-systems]]

# Related Patterns

- [[pattern-leader-based-replication]]
