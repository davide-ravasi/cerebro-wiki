---
id: concept-monotonic-clock
title: "Monotonic Clock"
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

A **monotonic clock** measures **elapsed time** on a single machine and is guaranteed **never to go backward**. Examples: `System.nanoTime()`, Linux `CLOCK_MONOTONIC`. The absolute value is arbitrary and **not comparable across machines**.

# Why It Matters

Correct tool for **timeouts**, **latency measurement**, and “how long did this take?” on **one node**. Does not fix distributed ordering and does **not** prevent **[[concept-process-pause]]** from breaking lease/leader logic.

# How It Works

Read at start and end of an operation; subtract for duration. NTP may **slew** the rate slightly but should not step the monotonic clock backward. On multi-socket servers, OS tries to present a consistent monotonic view across CPUs.

# Tradeoffs

- **Pros:** no backward jumps; good for local intervals.
- **Cons:** meaningless across nodes; thread may still be **paused** while monotonic time advances.

# When To Use

- Service response times, local timeout checks, profiling.
- **Do not use** to compare “now” between two servers or as global transaction IDs without sync + uncertainty handling.

# Example

`lease.expiry - monotonicNow()` on one node is safer than mixing wall clocks; still fails if the thread does not run for 15 s during GC before handling a request.

# Related Concepts

- [[concept-time-of-day-clock]]
- [[concept-process-pause]]
- [[map-distributed-systems]]
