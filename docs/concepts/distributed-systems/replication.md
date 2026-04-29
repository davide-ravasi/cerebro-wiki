---
id: concept-replication
title: "Replication"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-05
tags: [concept, distributed-systems, replication]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

Replication is the practice of maintaining copies of data on multiple nodes.

# Why It Matters

It improves availability, supports read scalability, and can reduce latency for geographically distributed users.

# How It Works

Writes are propagated across replicas with different topologies (single-leader, multi-leader, or leaderless).

# Tradeoffs

- Better availability but harder consistency guarantees.
- Lower read latency but more conflict and synchronization complexity.

# When To Use

- Use when uptime and read scale are core requirements.
- Avoid naive setups without clear consistency expectations.

# Example

Single-leader replication sends all writes to the leader and asynchronously updates followers.

# Related Concepts

- [[concept-reliability]]
- [[concept-partitioning]]

# Related Patterns

- [[pattern-leader-based-replication]]
