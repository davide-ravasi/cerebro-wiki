---
id: concept-phantom-read
title: "Phantom Read"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-07
tags: [concept, databases, transactions, isolation, concurrency, serialization]
status: evergreen
confidence: medium
updated: 2026-04-28
---

# Definition

A **phantom read** happens when one transaction **re-runs a search** (a query with a **predicate**, e.g. a room and a time window) and gets a **different set of rows** because another transaction **inserted, deleted, or updated** rows that **now match** (or no longer match) that predicate between the two reads.

# Why It Matters

If correctness depends on “what exists in the world **matching this query**,” phantoms let another transaction **change the answer** to that query **without touching the same row IDs** you already locked. Serializable isolation must **prevent phantoms**, not only row-level conflicts.

# How It Works

Row locks on existing tuples are not enough when the risk is **new tuples appearing** in the query’s range. Engines use stronger mechanisms, for example:

- **Predicate locks** (conceptual model in DDIA), or
- **Index-range / gap / next-key** style locks in real systems,

so that a concurrent **insert or update** cannot land inside another transaction’s **searched** predicate (same room + overlapping time window in the meeting-room example) until serialization rules allow it.

# Tradeoffs

- Stronger predicate or range locking **prevents phantoms** and many skew-style bugs tied to searches.
- It increases **blocking**, **deadlock risk**, and implementation complexity compared to naive row locks.

# When To Use

- Whenever application invariants are expressed as **“no other row may exist matching X”** and you rely on a **search** before an insert/update.
- When you choose **serializable** isolation (or equivalent) and need behavior aligned with that guarantee.

# Example

**Transaction A** searches for existing bookings for **room 101** between **14:00–15:00** and sees none, then prepares to insert its booking. **Transaction B** concurrently inserts a booking for **room 101** overlapping that window. B’s row is a **phantom** relative to A’s earlier search result; allowing both commits can violate the “no double booking” invariant unless the engine blocks overlapping inserts against A’s predicate.

# Related Concepts

- [[concept-write-skew]]
- [[concept-two-phase-locking]]
- [[map-databases]]

# Related Patterns

- Add a `pattern-` note when you document a concrete booking or inventory invariant in your wiki.
