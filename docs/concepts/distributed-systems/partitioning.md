---
id: concept-partitioning
title: "Partitioning"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-06
tags: [concept, distributed-systems, partitioning, sharding]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

Partitioning (sharding) splits a dataset across multiple nodes so each node stores and serves only a subset of data.

# Why It Matters

It enables horizontal scale when one machine is no longer enough for throughput or storage.

# How It Works

A partition key selects where each record lives, and routing logic sends requests to the right shard.

# Tradeoffs

- Better scalability but more routing and rebalancing complexity.
- Poor shard-key choices can create hotspots and uneven load.

# When To Use

- Use when data volume or request rates exceed single-node limits.
- Avoid early sharding when a single node still provides enough headroom.

# Example

Hash-based partitioning distributes writes more evenly, while range partitioning favors range queries.

# Related Concepts

- [[concept-replication]]
- [[concept-reliability]]

# Related Patterns

- [[pattern-shard-key-selection]]
