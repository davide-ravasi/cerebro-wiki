---
id: concept-write-skew
title: "Write Skew"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, isolation, concurrency, snapshot-isolation]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

**Write skew** is a concurrency anomaly where **two (or more) transactions read overlapping data**, each **writes based on what it read**, and the **combined writes** violate an **invariant** that each transaction assumed was true—even though there may be **no direct read–write conflict on the same row** between them.

# Why It Matters

Under **snapshot isolation**, each transaction often sees a **consistent snapshot** and passes **local** checks (“I can safely update **my** row”), yet commits can still produce a **globally invalid** state. DDIA discusses this with examples such as **on-call doctors** and **meeting-room booking**; phantoms can **enable** skew when decisions depend on a **search** whose result another transaction can change.

# How It Works

Each transaction:

1. Reads some facts from the database (possibly the same rows, or rows related by a business rule).
2. Decides it is allowed to write (e.g. “at least two doctors remain on call”).
3. Writes **different** rows (each updates its own row).

Individually the writes look valid against each snapshot; together they break a **multi-row** or **predicate-level** rule. Preventing skew at serializable strength may require **serializable** isolation (e.g. **SSI**), **two-phase locking** with appropriate **predicate/range** locking, or careful **application-level** constraints—not snapshot isolation alone.

# Tradeoffs

- Fixing write skew with stronger isolation or locking improves **correctness** but can reduce **throughput** and increase **retries** (aborts).
- Application-only checks without DB support are easy to get wrong under concurrency.

# When To Use

- Model explicitly when invariants span **multiple rows** or **existence** (“no row may match this query”) rather than single-row updates.
- Prefer serializable mechanisms (or documented patterns) when skew would cause **real business harm** (double booking, safety staffing rules, etc.).

# Example

**On-call doctors:** two doctors each see “at least two of us are on call” and both set themselves off duty—each update touches a **different** row, but after both commits **zero** doctors remain on call.

**Booking + phantoms:** one transaction **searches** for conflicting bookings in a time window; another **inserts** a conflicting row that **was not in the first search result**. That insert is a **phantom** relative to the search; together the commits can realize **write skew** on the “no overlap” invariant unless the database prevents concurrent inserts/updates against that **predicate**.

# Related Concepts

- [[concept-phantom-read]]
- [[concept-two-phase-locking]]
- [[map-databases]]

# Related Patterns

- Add a `pattern-` note when you capture a concrete invariant (booking, staffing) and the chosen isolation level.
