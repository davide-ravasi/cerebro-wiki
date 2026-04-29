---
id: concept-deadlock
title: "Deadlock"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, concurrency, locking]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

A **deadlock** is a situation where two or more transactions (or processes) are **blocked indefinitely** because each waits for a **resource** (e.g. a lock) held by another participant in the same cycle. No one can proceed without external intervention.

# Why It Matters

Lock-based concurrency control (including **two-phase locking**) naturally creates **wait-for** relationships. Without detection or prevention, the system can stall forever on a small set of transactions.

# How It Works

Typical example with two rows and two transactions:

1. **T1** locks **row A**, then tries to lock **row B** (waits if T2 holds B).
2. **T2** locks **row B**, then tries to lock **row A** (waits if T1 holds A).

Cycle: T1 → waits for T2 → waits for T1 → **deadlock**.

Database engines usually:

- **Detect** deadlocks (e.g. wait-for graph) or use **timeouts**, then
- **Abort** one transaction (rollback), releasing its locks so others can continue.

# Tradeoffs

- Aborting restores liveness but **fails** one client operation (must retry).
- Timeouts are simple but can misclassify **slow** waits as deadlocks.
- **Prevention** (e.g. global lock ordering) reduces deadlocks but constrains how applications access data.

# When To Use

“Deadlock” is not something you opt into; it is a **risk** whenever transactions acquire multiple locks in competing orders. You **design** access patterns and indexes to reduce contention; the engine **handles** resolution.

# Example

Same as above: classic **A then B** vs **B then A** lock order on two hot rows under concurrent updates.

# Related Concepts

- [[concept-two-phase-locking]]
- [[map-databases]]

# Related Patterns

- Consistent lock ordering in application code to reduce deadlock probability (pattern note optional).
