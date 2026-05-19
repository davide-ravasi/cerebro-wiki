---
id: concept-ntp
title: "NTP (Network Time Protocol)"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, time, synchronization]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

**NTP (Network Time Protocol)** is how computers **align their system clocks** to a shared time reference over the network. An **NTP server** provides the authoritative time; **clients** periodically query it, estimate network delay, and **adjust** the local clock.

# Why It Matters

In distributed systems, each machine has its own hardware clock that **drifts**. Without synchronization, logs, timeouts, leases, and “expires at” logic become unreliable. NTP keeps clocks **close enough** for many operational uses—but it is not a perfect global timeline for correctness proofs.

# How It Works

1. Client sends a time request to an NTP server.
2. Server responds with its current time (and timing metadata).
3. Client estimates **round-trip delay** and applies a small correction to local time.
4. Adjustments are usually **gradual** (slew) rather than one large jump, when possible.

Typical accuracy: often within **milliseconds** on a good LAN; worse over the public Internet due to variable latency.

# Tradeoffs

- **Pros:** cheap, ubiquitous, good enough for monitoring, TTLs, coarse ordering, human-readable timestamps.
- **Cons:** remaining **skew** and **jitter**; possible **clock jumps**; not sufficient alone when you need strict event ordering across nodes (see logical clocks in DDIA).

# When To Use

- Sync servers and services for operations, logging correlation, certificate expiry, session timeouts.
- Do **not** assume `Date.now()` on two nodes is comparable without margin; prefer **logical clocks** or **hybrid** designs when ordering events is safety-critical.

# Example

Two API servers without NTP might disagree by seconds; after NTP, they might still differ by tens of milliseconds—enough to mis-order events if you trust wall time naively.

# Related Concepts

- [[concept-time-of-day-clock]]
- [[concept-leap-second]]
- [[concept-process-pause]]
- [[concept-reliability]]
- [[map-distributed-systems]]

# Related Patterns

- Use **monotonic clocks** (`performance.now()` in Node) for measuring durations, not for cross-machine ordering.
