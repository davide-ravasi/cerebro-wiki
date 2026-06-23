---
id: concept-index-range-lock
title: "Index-Range Lock (Gap / Next-Key Lock)"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, isolation, locking, indexes]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

An **index-range lock** locks a **range of an index** (including **gaps** between existing keys), approximating a **predicate lock** without checking every write against every active query condition. InnoDB calls this **next-key locking** (record + gap).

# Why It Matters

**[[concept-two-phase-locking]]** on individual rows cannot stop **[[concept-phantom-read]]** when the conflicting insert is a **new row** matching a search. Range locks on the **index** block inserts into that slice of key space.

**[[concept-database-index]]** therefore affects not only query speed but **what can be locked precisely** during transactions.

# How It Works

1. Transaction searches `room_id = 101 AND time BETWEEN 14:00 AND 15:00`.
2. With an index on `(room_id, time)`, the engine locks the **index range** covering that interval (and gaps).
3. Concurrent insert at 14:30 must update the index in the locked range → **waits** or fails per isolation rules.

Pure **predicate locks** are precise but costly; index-range locks may lock **slightly more** than strictly necessary — pragmatic tradeoff.

# Tradeoffs

- **Pros:** practical phantom prevention in real engines.
- **Cons:** without suitable index, engine may escalate to **coarser** locks (e.g. table); extra blocking on hot ranges.

# When To Use

Understand when designing **serializable** or strong **2PL** workloads with existence checks (“no row in this range”) before insert.

# Example

Empty slot search for meeting room: no row to lock, but index gap lock on the time range prevents double booking insert.

# Related Concepts

- [[concept-phantom-read]]
- [[concept-two-phase-locking]]
- [[concept-database-index]]
- [[concept-write-skew]]
- [[map-databases]]
