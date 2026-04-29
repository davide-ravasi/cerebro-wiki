---
id: pattern-leader-based-replication
title: "Leader-Based Replication"
type: pattern
domain: distributed-systems
tags: [pattern, replication, consistency, distributed-systems]
status: draft
confidence: medium
updated: 2026-04-28
---

# Context

A system needs strong write ordering and simple operational behavior for replication.

# Problem

How to replicate data safely while keeping write semantics understandable for developers.

# Decision

Use a single leader for writes and one or more followers for read scaling and failover.

# Why

Centralized write coordination simplifies conflict handling and reasoning about correctness.

# Tradeoffs

- Simpler write path and conflict model.
- Leader can become a bottleneck and failover adds complexity.

# Alternatives Considered

- Multi-leader replication
- Leaderless quorum-based replication

# Signals To Revisit

- Write throughput approaches leader limits.
- Multi-region write latency becomes unacceptable.

# Related

- [[concept-replication]]
- [[concept-reliability]]
