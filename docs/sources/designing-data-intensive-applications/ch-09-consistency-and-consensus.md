---
id: source-ddia-ch-09
title: "DDIA Chapter 9: Consistency and Consensus (partial)"
type: source
domain: distributed-systems
sources:
  - kind: book
    title: "Designing Data-Intensive Applications"
    chapter: 9
    author: "Martin Kleppmann"
tags: [source, ddia, distributed-systems, consistency, linearizability, consensus]
status: draft
confidence: medium
updated: 2026-05-19
---

# Summary

Chapter 9 moves from **single-node transactions** (Ch. 7) and **unreliable time/networks** (Ch. 8) to **distributed consistency** and **agreement**. **Linearizability** is a strong per-operation guarantee: one logical copy of data, serial order respecting real time. Implementations use **shared ordering mechanisms** (leader, quorum, consensus, total order broadcast) — covered in later sections of the chapter.

Partial coverage: **linearizability** (intro + concurrent ops). Remainder TBD (causal consistency, total order broadcast, Raft, etc.).

Raw working notes (Italian): `raw/chapter-9.md`.

# Key Concepts

- [[concept-linearizability]]

# Section notes (wiki body)

## 1. What linearizability promises

Not eventual convergence alone — **immediate recency**: after a write **completes**, any read that **starts after** must see that write (or later state).

Analogies:

- **Single whiteboard** everyone reads — not a photocopy that updates later.
- **Register** with one serial history visible to all clients.

## 2. Formal intuition (without full proof)

- Operations have intervals `[start, end]`.
- Pick a **linearization point** inside each interval.
- Sort operations by those points → must match single-register behavior.
- **Real-time constraint:** if `end(A) < start(B)`, then A before B in the sort.

## 3. Concurrent operations

| Pattern | Linearizable? |
|---------|----------------|
| W finishes, then R starts | R must see W |
| R finishes, then W starts | R sees pre-W value |
| W and R **overlap** | R may see old **or** new (either order can be valid) |
| W1 and W2 overlap | One serial order W1→W2 or W2→W1; one final value |

Common mistake: thinking overlap means “still writing” when the scenario states the write **already completed**.

## 4. Who decides / who enforces

- **Not** the client choosing what to see.
- **Not** a single node’s local belief without quorum ([[concept-quorum-majority-truth]]).
- **Yes** the storage/coordination **protocol**: primary replication, sync quorum, consensus log, ZooKeeper/etcd-style services.

Links to Ch. 8: unreliable clocks and async replication make **naive** reads linearizable only if the design enforces ordering.

## 5. Linearizability vs serializability (Ch. 7)

| | Linearizability | Serializability |
|---|-----------------|-----------------|
| Unit | Usually single operation / register | Whole **transaction** |
| Real-time order | Required between non-overlapping ops | Not required |
| Typical home | Coordination, locks, registers | Database transactions |

Serializable transaction history can violate linearizability if serial order does not respect wall-clock order across objects.

## 6. Cost and when it appears

- Stronger than **eventual consistency** or **causal consistency** (later in chapter).
- Often implemented only for **metadata** paths (leader, lock, membership), not every row in a huge eventually consistent store.
- CAP / partition tradeoffs: linearizable coordination may sacrifice availability during network split.

## 7. Practical dev checklist

- After write success, reads must not hit **stale replicas** unless you accept weaker semantics.
- MongoDB: primary vs secondary, `readConcern`, `writeConcern`.
- Do not use wall-clock timestamps alone for global ordering ([[concept-last-write-wins-timestamp-risk]]).

# Open Threads (remainder of Ch. 9)

- Causal consistency and happens-before across replicas
- Total order broadcast
- Distributed transactions vs consensus
- Raft / Paxos
- Membership and coordination services (ZooKeeper, etcd)

# What Changed My Mental Model

- Linearizability is about **one shared serial story** + **real-time recency**, not “everyone converges eventually.”
- **Overlap** allows ambiguity; **finished-then-read** does not.
- The **system** imposes order; clients only choose **which API/replica** they call.

# Related Notes

- [[map-distributed-systems]]
- [[map-databases]]
- [[source-ddia-ch-07]] — serializability
- [[source-ddia-ch-08]] — quorum, time
- Raw: `raw/chapter-9.md`
