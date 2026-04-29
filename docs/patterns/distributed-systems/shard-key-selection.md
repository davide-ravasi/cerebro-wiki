---
id: pattern-shard-key-selection
title: "Shard Key Selection"
type: pattern
domain: distributed-systems
tags: [pattern, partitioning, sharding, distributed-systems]
status: draft
confidence: medium
updated: 2026-04-28
---

# Context

A distributed database needs a partition key to distribute data across shards.

# Problem

A poor shard key can create hotspots, unbalanced shards, and expensive rebalancing.

# Decision

Choose a shard key based on access patterns, cardinality, and expected write distribution.

# Why

A good key improves load balance, predictable latency, and cluster stability.

# Tradeoffs

- Hash keys improve distribution but hurt range queries.
- Range keys support scans but risk time-based hotspots.

# Alternatives Considered

- Pure range partitioning
- Pure hash partitioning
- Composite shard keys

# Signals To Revisit

- Persistent shard imbalance
- Growing tail latency on specific partitions

# Related

- [[concept-partitioning]]
- [[concept-replication]]
