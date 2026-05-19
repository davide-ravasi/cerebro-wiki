---
id: concept-logical-clock
title: "Logical Clock"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, ordering, causality]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

A **logical clock** orders events by **relative causality** (“happened before”), not by wall-clock time. It uses counters (e.g. **Lamport timestamps**) or structures like **version vectors**, not quartz oscillators.

# Why It Matters

**[[concept-time-of-day-clock]]** and **[[concept-ntp]]** cannot give safe global ordering when skew, jumps, and network delay exist. Logical clocks support correct **causal ordering** without assuming synchronized physical time.

# How It Works

**Lamport clock (sketch):**

1. On local event: increment counter `L`.
2. On send: attach `L` to message.
3. On receive: `L = max(L, L_message) + 1`.

If `L(A) < L(B)`, then A might have happened before B (not the reverse: concurrent events can have arbitrary order).

**Vector clocks** extend this to detect **concurrency** and avoid some causal violations that Lamport alone cannot express.

Logical clocks measure **order**, not “what time is it on Earth?”

# Tradeoffs

- **Pros:** safe ordering abstraction for distributed causality.
- **Cons:** no human time; more metadata; vector clocks cost grows with replicas.

# When To Use

Conflict detection, causal consistency, replacing naive **[[concept-last-write-wins-timestamp-risk]]** when ordering matters.

# Example

Two writes with no causal link may get Lamport times where order is arbitrary; version vectors reveal they were concurrent.

# Related Concepts

- [[concept-last-write-wins-timestamp-risk]]
- [[concept-time-of-day-clock]]
- [[map-distributed-systems]]
