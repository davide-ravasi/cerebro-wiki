---
id: concept-database-index
title: "Database Index"
type: concept
domain: databases
sources:
  - note: source-ddia-ch-03
tags: [concept, databases, storage, indexing, performance, query-optimization]
status: evergreen
confidence: medium
updated: 2026-04-29
---

# Definition

A **database index** is an auxiliary data structure derived from table data that lets the engine find rows matching a lookup or range predicate **without scanning the whole table**. It trades **extra storage** and **write cost** for **faster reads** on specific access paths.

# Why It Matters

Most application queries filter or join on a small set of columns. Without indexes, latency grows with table size (full scans). With the right indexes, common paths stay fast as data grows. Indexes also underpin **physical design** and, in some systems, **locking granularity** (e.g. range locks on index keys for phantom prevention).

# How It Works

Conceptually, an index maps **key values** (one or more columns) to **row locations** (row IDs, heap tuples, or primary-key clustering depending on the engine).

Common families:

- **B-tree (and variants)** — default in many relational stores; good for point lookups and ordered ranges on one or more columns.
- **Hash indexes** — fast equality lookups; weak or no range support.
- **LSM-tree secondary indexes** — common in log-structured stores; different compaction and read amplification tradeoffs.

The query planner chooses **index seek / scan** vs **heap / table scan** based on statistics, selectivity, and cost models.

# Tradeoffs

- **Faster reads** on indexed predicates vs **slower writes** (every insert/update/delete may need to update each relevant index).
- **More disk and memory** for index pages.
- **Wrong or redundant indexes** waste space and hurt write throughput without helping reads.

# When To Use

- Add indexes for **high-selectivity** filters, join keys, and foreign keys used in lookups.
- Revisit after measuring **slow queries** and **write hotspots**; avoid indexing every column by default.

# Example

A table `bookings(room_id, start_time, end_time)` with heavy queries like “conflicts for room 101 between 14:00–15:00” benefits from a composite or covering index aligned with that access pattern; without it, the engine may scan all bookings for every conflict check.

# Related Concepts

- [[concept-phantom-read]]
- [[concept-two-phase-locking]]
- [[concept-partitioning]]
- [[map-databases]]

# Related Patterns

- Add a `pattern-` note when you document a concrete indexing strategy for a workload (OLTP vs reporting).
