---
id: concept-clock-confidence-interval
title: "Clock Confidence Interval (Global Snapshots)"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, time, spanner, snapshots]
status: evergreen
confidence: medium
updated: 2026-05-19
---

# Definition

A **clock confidence interval** treats a timestamp not as a precise point but as a **range** `[earliest, latest]` within which the true time probably lies. For **global snapshots** or distributed commits, systems may **wait until the uncertainty window passes** before fixing an event’s timestamp.

# Why It Matters

NTP-synced **[[concept-time-of-day-clock]]** clocks are close but not identical across nodes. Using a single wall-clock value **T** as a “photograph of the whole system at T” can produce **inconsistent snapshots** when skew and jitter exist. Treating time as an interval — and waiting — is how **TrueTime** (Spanner) makes global ordering safer than naive timestamps.

# How It Works

1. Clock hardware + sync (GPS, atomic clocks, NTP) still leaves **uncertainty** (often tens of milliseconds or more on commodity networks).
2. Instead of returning `now = 12:00:00.500`, return `now ∈ [12:00:00.480, 12:00:00.520]`.
3. Before **commit** or fixing a global snapshot timestamp **S**, the system **waits** until `latest < S` (commit wait) so no causally earlier event can receive a later timestamp by clock ambiguity.

Most systems expose only a point timestamp and **hide uncertainty** — dangerous when ordering or snapshots are safety-critical.

# Tradeoffs

- **Pros:** stronger external consistency / global snapshot semantics than raw wall-clock.
- **Cons:** added **latency** (commit wait), expensive infrastructure (Spanner-style), still does not fix **[[concept-process-pause]]** alone.

# When To Use

- Multi-partition **consistent snapshots**, globally ordered transactions, “as of” reads across shards.
- **Avoid** assuming microsecond-precision `Date.now()` across nodes without margin or without this model.

# Example

Two nodes skewed by 2 ms: a write on A may fall inside or outside snapshot **T** depending on which clock you trust. Confidence intervals + wait align “before/after” with real elapsed time at datacenter scale.

# Related Concepts

- [[concept-time-of-day-clock]]
- [[concept-ntp]]
- [[concept-last-write-wins-timestamp-risk]]
- [[concept-logical-clock]]
- [[map-distributed-systems]]
