---
id: concept-serializable-snapshot-isolation
title: "Serializable Snapshot Isolation (SSI)"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, isolation, serialization, optimistic]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

**Serializable Snapshot Isolation (SSI)** is an **optimistic** serializability algorithm: transactions run on **snapshots** without blocking locks during execution; the database **detects serialization conflicts at commit** and **aborts** transactions that would violate serial order.

# Why It Matters

Bridges the gap between fast **[[concept-snapshot-isolation-mvcc]]** and full **serializability**. Popularized in PostgreSQL (~2012). Catches **[[concept-write-skew]]** and related conflicts that snapshot isolation alone allows.

# How It Works

While transactions run, the engine tracks:

- Reads that became **stale** because another transaction wrote the data.
- Writes that **invalidate** another transaction’s reads.

On **commit**, if a serial order cannot include this transaction, **abort** and let the application **retry**. No growing-phase lock waits like **[[concept-two-phase-locking]]** during execution.

# Tradeoffs

- **Pros:** reads stay fast; no predicate-lock blocking during work; good for many read-heavy workloads.
- **Cons:** under high write contention, **abort rate** rises; apps must handle retries; not “free” serializability.

# When To Use

When you need **serializable** guarantees but want to avoid 2PL’s pessimistic blocking — and you can implement **idempotent retry** in the application.

# Example

Two on-call doctors both read “≥2 on duty,” both go off duty — SSI detects the skew at commit and aborts one transaction.

# Related Concepts

- [[concept-snapshot-isolation-mvcc]]
- [[concept-write-skew]]
- [[concept-two-phase-locking]]
- [[concept-phantom-read]]
- [[map-databases]]
