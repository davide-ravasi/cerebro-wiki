---
id: concept-schema-evolution
title: "Schema Evolution"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-04
tags: [concept, distributed-systems, schema, compatibility]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

Schema evolution is the ability to change data structures over time without breaking producers or consumers.

# Why It Matters

Data outlives code, so safe evolution is required for rolling upgrades and long-lived storage.

# How It Works

Compatibility rules (backward and forward) plus defaults and unknown-field handling allow old and new versions to coexist.

# Tradeoffs

- Strong compatibility rules increase safety but constrain rapid schema changes.
- Weak rules allow speed but increase migration risk.

# When To Use

- Always required for distributed systems with independent deployments.
- Critical for event-driven systems and long-retained data.

# Example

Adding an optional field with a default value keeps old readers and new writers compatible.

# Related Concepts

- [[concept-replication]]
- [[concept-partitioning]]

# Related Patterns

- [[pattern-version-tolerant-events]]
