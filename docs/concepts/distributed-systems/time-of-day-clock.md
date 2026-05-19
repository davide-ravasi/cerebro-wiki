---
id: concept-time-of-day-clock
title: "Time-of-Day Clock (Wall Clock)"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, time]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

A **time-of-day clock** returns the current **calendar time** (wall-clock time): e.g. seconds since the Unix epoch. Examples: `System.currentTimeMillis()`, Linux `CLOCK_REALTIME`.

# Why It Matters

Used for log timestamps, cache expiry, “published at”, reminders, and human-facing semantics. Often synchronized via **[[concept-ntp]]** so different machines share a similar notion of “now” — but it is **not** a reliable measure of **elapsed time** or **causal order** across nodes.

# How It Works

- Hardware: quartz oscillator per machine (drifts).
- NTP adjusts the clock periodically; large skew may cause **forced reset** (time jumps backward or forward).
- **[[concept-leap-second]]** and historical coarse resolution on some systems add quirks.

# Tradeoffs

- **Pros:** intuitive, globally meaningful when sync is good enough for ops.
- **Cons:** jumps, drift, unsuitable alone for distributed **ordering** or strict duration measurement.

# When To Use

- User-visible timestamps, auditing, coarse expiry.
- **Avoid** for **last-write-wins** conflict resolution or “which write happened first?” without extra mechanisms ([[concept-logical-clock]], version vectors).

# Example

Two nodes with 3 ms skew can still order writes wrong under LWW (see [[concept-last-write-wins-timestamp-risk]]).

# Related Concepts

- [[concept-monotonic-clock]]
- [[concept-ntp]]
- [[concept-leap-second]]
- [[map-distributed-systems]]
