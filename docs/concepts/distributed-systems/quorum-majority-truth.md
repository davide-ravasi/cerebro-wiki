---
id: concept-quorum-majority-truth
title: "Quorum — Truth Defined by the Majority"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, consensus, quorum, faults]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

In a distributed system, **truth** about leadership, locks, or committed state is what a **quorum** (typically a **majority** of nodes) agrees on — not what any **single node** believes locally.

# Why It Matters

One node can be wrong while still “healthy”: **asymmetric network fault** (can receive but not send), long **[[concept-process-pause]]**, or stale local lease. Relying on “I checked locally, I am the leader” causes **split brain**. Quorum-based decisions survive individual node lies, silence, or isolation — within crash-fault assumptions.

# How It Works

- **Leader election / locks:** a candidate must get votes or lease acknowledgment from **> n/2** nodes (exact rules vary by system).
- **Replication (Ch. 5 link):** reads and writes overlap quorums (`w + r > n`) so a read sees latest write.
- A node that **believes** it is leader but **lost quorum contact** must stop acting — otherwise it needs **[[concept-fencing-token]]** or version checks at storage.

Single-node truth fails because you cannot know if a silent peer is slow, paused, or partitioned.

# Tradeoffs

- **Pros:** foundation for safe failover, consistent replication, distributed locks (ZooKeeper, etcd, Raft).
- **Cons:** needs **odd** node counts or careful tie-breaking; **latency** for round trips; majority unavailable → unavailability.

# When To Use

Any safety property that must hold cluster-wide: one writer, one lock holder, one registration, committed transaction visibility.

# Example

Leader holds lease locally; GC pauses 15 s; quorum already elected a new leader. Old leader’s local clock/lease check is **false belief** — quorum truth says someone else leads.

# Related Concepts

- [[concept-process-pause]]
- [[concept-fencing-token]]
- [[concept-timeout-failure-detection]]
- [[concept-replication]]
- [[pattern-leader-based-replication]]
- [[concept-linearizability]]
- [[map-distributed-systems]]
