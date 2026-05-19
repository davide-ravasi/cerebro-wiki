---
id: concept-last-write-wins-timestamp-risk
title: "Last Write Wins (Timestamp Risk)"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, replication, clocks, consistency]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

**Last write wins (LWW)** resolves write conflicts by keeping the value with the **latest timestamp** and discarding others. When “latest” comes from **local time-of-day clocks** on different nodes, **causally later** writes can look **older** and be dropped silently.

# Why It Matters

Used in multi-leader and some leaderless stores (e.g. Cassandra, Riak patterns). Simple for developers, dangerous with clock skew: **arbitrary data loss** without errors to the application.

# How It Works

Each write tagged with origin node’s wall time. On merge, highest timestamp wins. If node B’s clock lags, B’s write can lose to an earlier-looking write from A even when B’s operation happened after A’s in real causality (DDIA Fig. 8-3).

Additional issues:

- Millisecond resolution → ties need random tie-breakers (can break causality).
- True concurrency vs causal sequence not distinguished without **version vectors**.

# Tradeoffs

- **Pros:** simple, low metadata, easy mental model.
- **Cons:** silent loss; depends on **[[concept-time-of-day-clock]]** accuracy; “impossible” orderings (packet “arrives before sent” in timestamp space).

# When To Use

Only with eyes open: understand clock sync limits, monitor skew, prefer **[[concept-logical-clock]]** / version vectors when causality matters.

# Example

Client A sets `x = 1`; replicated; client B increments to `x = 2`; B’s timestamp lower than A’s → `x = 2` discarded on some replica.

# Related Concepts

- [[concept-time-of-day-clock]]
- [[concept-logical-clock]]
- [[concept-replication]]
- [[map-distributed-systems]]
