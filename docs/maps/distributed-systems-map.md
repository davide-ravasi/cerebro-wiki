---
id: map-distributed-systems
title: "Distributed Systems Map"
type: map
domain: distributed-systems
tags: [map, navigation, distributed-systems]
status: evergreen
updated: 2026-05-19
---

# Purpose

Central navigation for distributed systems notes and decisions.

# Core Concepts

## Foundations

- [[concept-reliability]]
- [[concept-replication]]
- [[concept-partitioning]]
- [[concept-schema-evolution]]

## Networks and faults (DDIA Ch. 8)

- [[concept-timeout-failure-detection]]

## Clocks and time (DDIA Ch. 8)

- [[concept-time-of-day-clock]]
- [[concept-monotonic-clock]]
- [[concept-ntp]]
- [[concept-leap-second]]
- [[concept-last-write-wins-timestamp-risk]]
- [[concept-logical-clock]]
- [[concept-clock-confidence-interval]]
- [[concept-process-pause]]

## Truth, quorum, and safety (DDIA Ch. 8)

- [[concept-quorum-majority-truth]]
- [[concept-fencing-token]]
- [[concept-byzantine-fault]]

# Practical Patterns

- [[pattern-leader-based-replication]]
- [[pattern-shard-key-selection]]

# Source Notes

- [[source-ddia-ch-01]]
- [[source-ddia-ch-02]]
- [[source-ddia-ch-03]]
- [[source-ddia-ch-04]]
- [[source-ddia-ch-05]]
- [[source-ddia-ch-06]]
- [[source-ddia-ch-08]] — Ch. 8 complete (networks, clocks, quorum, fencing); raw: `sources/designing-data-intensive-applications/raw/chapter-8.md`

# Open Threads

- Promote Ch. 7 transactions source
- Consensus protocols (Raft, Paxos) — Ch. 9
- Stream processing and event-time semantics
- Deeper Spanner / TrueTime case study (optional)

# Related Maps

- [[map-databases]] — transactions, locking, isolation (DDIA Ch. 7)
