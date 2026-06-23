---
id: concept-two-phase-locking
title: "Two-Phase Locking (2PL)"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, concurrency, isolation, serialization]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

Two-phase locking is a **rule for acquiring and releasing locks** so that concurrent transactions behave as if they ran in some **serial order**. In the **growing phase**, a transaction may acquire locks but not release them. In the **shrinking phase**, it may release locks but not acquire new ones.

**Strict 2PL** (common in real systems and textbooks) holds all locks until **commit or abort**, then releases them together. That simplifies reasoning about what other transactions can see.

# Why It Matters

Without a disciplined locking protocol, interleaved reads and writes produce anomalies (lost updates, read skew, write skew, phantoms). 2PL is the classic **pessimistic** way to get **serializable** behavior without literally running one transaction at a time.

# How It Works

A **lock manager** mediates access to shared objects (rows, pages, index ranges, etc.):

- **Shared (S) lock** for reads: compatible with other S locks, not with **exclusive (X)** locks.
- **Exclusive (X) lock** for writes: incompatible with any other lock on the same object.

Before an operation runs, the transaction **requests** the lock; if it cannot be granted, the transaction **waits**. The two-phase rule prevents schedules that would otherwise be equivalent to **non-serializable** interleavings.

**Phantom reads** require stronger locks than “this row only” (e.g. predicate or index-range locks), because otherwise another transaction can **insert** rows that match your query’s condition.

# Tradeoffs

- Strong correctness guarantees (serializability) vs **throughput** and **latency** under contention.
- Risk of **deadlocks** when transactions wait on each other in a cycle.
- Predicate / range locking adds complexity and blocking.

# When To Use

- When you need **serializable isolation** and accept **blocking** and deadlock handling.
- Prefer understanding **SSI** or snapshot-based approaches when the workload is read-heavy and contention patterns are unfriendly to locking.

# Example

Transaction A updates row 1 then row 2; transaction B updates row 2 then row 1. If each holds an X lock on its first row while waiting for the second, you can get a **deadlock** unless the DBMS aborts one transaction.

# Related Concepts

- [[concept-deadlock]]
- [[concept-phantom-read]]
- [[concept-write-skew]]
- [[concept-index-range-lock]]
- [[concept-serializable-snapshot-isolation]]
- [[concept-database-index]]
- [[map-databases]]

# Related Patterns

- (Add patterns when you document isolation levels in your wiki.)
