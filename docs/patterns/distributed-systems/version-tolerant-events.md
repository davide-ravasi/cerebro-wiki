---
id: pattern-version-tolerant-events
title: "Version-Tolerant Events"
type: pattern
domain: distributed-systems
tags: [pattern, schema-evolution, events, compatibility]
status: draft
confidence: medium
updated: 2026-04-28
---

# Context

Multiple services publish and consume events while upgrading independently.

# Problem

Schema changes can break consumers that lag behind producer deployments.

# Decision

Use additive schema changes, defaults, and unknown-field tolerance for events.

# Why

This allows backward and forward compatibility across deployment boundaries.

# Tradeoffs

- Higher compatibility and safer rollouts.
- Extra discipline in schema governance and testing.

# Alternatives Considered

- Lockstep deployment of all services
- Version-per-topic with hard cutovers

# Signals To Revisit

- Frequent consumer breakages due to schema mismatch
- Growing complexity of compatibility rules

# Related

- [[concept-schema-evolution]]
- [[concept-replication]]
