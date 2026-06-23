---
id: source-ddia-ch-07
title: "DDIA Chapter 7: Transactions"
type: source
domain: databases
sources:
  - kind: book
    title: "Designing Data-Intensive Applications"
    chapter: 7
    author: "Martin Kleppmann"
tags: [source, ddia, databases, transactions, isolation, acid, concurrency]
status: draft
confidence: medium
updated: 2026-05-19
---

# Summary

Transactions group reads and writes into one logical unit so applications can handle errors and concurrency without ad-hoc coordination. **ACID** names the goals; **isolation levels** trade correctness for performance. Weak isolation prevents some anomalies but not **lost updates**, **write skew**, or **phantoms**. **Serializability** restores “as if sequential” execution via **serial execution**, pessimistic **2PL** (with **index-range locks** for phantoms), or optimistic **SSI** (abort at commit on conflict).

Raw working notes (Italian): `raw/chapter-7.md`.

# Key Concepts

## Foundations

- [[concept-snapshot-isolation-mvcc]]

## Anomalies

- [[concept-lost-update]]
- [[concept-write-skew]]
- [[concept-phantom-read]]

## Serializable isolation

- [[concept-two-phase-locking]]
- [[concept-index-range-lock]]
- [[concept-serializable-snapshot-isolation]]
- [[concept-deadlock]]

# Section notes (wiki body)

## 1. What is a transaction?

Not magic — an **abstraction** to bundle multiple operations. On error, **abort** and discard partial work (**atomicity**). **Consistency** is mostly application responsibility (valid states). **Isolation** keeps concurrent transactions from stepping on each other. **Durability** means committed data survives crashes (WAL, replication, etc.).

## 2. Weak isolation levels

Most production DBs default below serializable for performance.

### Read committed

- No **dirty reads** (only committed data visible).
- No **dirty writes** (cannot overwrite uncommitted data).

### Snapshot isolation (MVCC)

Each transaction reads from a **consistent snapshot** at start — fixes **read skew** (inconsistent reads within one transaction). **Readers don't block writers** and vice versa via **multi-version** storage. See [[concept-snapshot-isolation-mvcc]].

Does **not** prevent **write skew** or all phantom scenarios without extra mechanisms.

## 3. Preventing lost updates

Classic **read–modify–write** race: two transactions read the same value, both increment, one increment is lost.

Mitigations:

- **Atomic writes** (e.g. MongoDB `$inc`) — single-server operator, no read-modify-write in app.
- **Explicit locking** (`SELECT … FOR UPDATE`).
- **Compare-and-set** — update only if value unchanged since read.

See [[concept-lost-update]].

## 4. Write skew and phantoms

**Write skew:** transactions read overlapping data, write **different** rows, together violate an invariant (on-call doctors, booking rules). Possible under snapshot isolation because each transaction passes **local** checks.

**Phantom read:** a **search** returns different rows on re-run because another transaction **inserted/deleted/updated** matching rows. Row locks on existing tuples are insufficient.

See [[concept-write-skew]], [[concept-phantom-read]].

## 5. Serializability

Strongest isolation: result equivalent to **some serial order** of transactions.

| Approach | Style | Idea |
|----------|-------|------|
| **Actual serial execution** | Extreme pessimism | One thread, one transaction at a time — simple, limited throughput |
| **Two-phase locking (2PL)** | Pessimistic | Shared locks for read, exclusive for write; hold until commit; writers block readers and each other |
| **SSI** | Optimistic | Run on snapshot without locks; **detect conflicts at commit**, abort if serialization violated |

### Two-phase locking

- Growing phase: acquire locks; shrinking phase: release (strict 2PL: release only at end).
- **Performance cost:** less concurrency, **deadlocks** possible — see [[concept-deadlock]].
- **Phantoms:** need locks on **predicates**, not only existing rows.

### Predicate locks vs index-range locks

**Predicate lock** (conceptual): lock all rows matching a query, including **not-yet-inserted** rows — prevents phantoms but expensive to enforce.

**Index-range lock** (practical): lock a **range in the index** (including gaps / next-key) — approximate predicate lock, faster. **Indexes matter for transaction safety**, not only query speed. See [[concept-index-range-lock]].

### Serializable Snapshot Isolation (SSI)

PostgreSQL-style (≈2012): keep snapshot isolation performance; track **stale reads** and **writes that invalidate others' reads**; **abort at commit** if true serialization would be violated (e.g. write skew).

- **Pros:** reads stay fast, no predicate-lock blocking during execution.
- **Cons:** under heavy contention, more **aborts** — application must **retry**.

See [[concept-serializable-snapshot-isolation]].

## 6. Practical notes (MERN / MongoDB)

- MongoDB multi-document transactions (v4.0+) use **snapshot isolation** by default — not full serializable unless configured and supported paths.
- Prefer **atomic operators** when a single-document update suffices.
- With SSI or serializable APIs elsewhere, design backends for **conflict retries**.

# Important Tradeoffs

| Area | Tradeoff |
|------|----------|
| Isolation level | Correctness vs throughput |
| Snapshot isolation | Fast reads vs write skew / some anomalies |
| 2PL | Serializable pessimism vs blocking and deadlocks |
| SSI | Optimistic speed vs commit-time aborts and retries |
| Index-range locks | Phantom prevention vs lock granularity and index design |

# What Changed My Mental Model

- **Isolation level names are not interchangeable** across databases — read the fine print.
- **Snapshot isolation ≠ serializable** — write skew is the classic gap.
- **Phantoms** need **predicate/range** thinking, not only row locks.
- **Indexes** participate in **locking**, not just SELECT performance.
- **Optimistic (SSI)** pushes conflict handling to **commit + app retry**.

# Open Questions

- MongoDB serializable / snapshot behavior on sharded clusters in practice?
- When is atomic `$inc` enough vs multi-document transaction?

# Related Notes

- [[map-databases]]
- [[map-distributed-systems]]
- Raw: `raw/chapter-7.md`
