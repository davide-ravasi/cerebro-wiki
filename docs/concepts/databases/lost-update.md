---
id: concept-lost-update
title: "Lost Update"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, concurrency, isolation]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

A **lost update** occurs when two transactions **read the same value**, each **computes a new value**, and both **write** — one update is overwritten and **lost** (classic read–modify–write race).

# Why It Matters

Common in counters, balances, inventory, and “load JSON → edit → save” patterns. Even **read committed** does not fully solve this without explicit techniques.

# How It Works

1. A reads `count = 10`
2. B reads `count = 10`
3. A writes `count = 11`
4. B writes `count = 11` (A’s increment lost)

Mitigations:

- **Atomic operations** (DB-side increment).
- **Explicit row locks** during read–modify–write.
- **Compare-and-set** (update if value still equals what you read).

# Tradeoffs

- Atomic ops: simple, fast, but limited to supported operations.
- Locking: correct, can block.
- CAS: good for low contention; retries on failure.

# When To Use

Prefer **atomic updates** when possible; use transactions with appropriate isolation/locking when logic spans rows or documents.

# Example

Two users save a profile: both read version 1, both write version 2 — last writer wins, first edit lost.

# Related Concepts

- [[concept-snapshot-isolation-mvcc]]
- [[concept-two-phase-locking]]
- [[map-databases]]
