---
id: source-ddia-ch-08
title: "DDIA Chapter 8: The Trouble with Distributed Systems (partial)"
type: source
domain: distributed-systems
sources:
  - kind: book
    title: "Designing Data-Intensive Applications"
    chapter: 8
    author: "Martin Kleppmann"
tags: [source, ddia, distributed-systems, networks, clocks, faults]
status: draft
confidence: medium
updated: 2026-05-13
---

# Summary

Partial chapter coverage (through **unreliable clocks** and **process pauses**; remainder TBD). Core message: distributed systems must assume **partial failure**, **unbounded network delay**, and **unreliable time**. Timeouts detect faults heuristically; clocks drift and jump; **wall-clock timestamps are unsafe for ordering writes**; **process pauses** can invalidate leases and leader assumptions even when clocks are synchronized.

Raw working notes: `raw/chapter-8.md`.

# Key Concepts

## Networks (covered in raw)

- Timeouts are a **heuristic**, not proof of failure.
- Delays are **unbounded** (congestion, queuing, CPU load, VM pauses, flow control).
- Packet-switched (TCP/IP) vs circuit-switched (telephone): fixed bandwidth circuits do not fit bursty datacenter traffic.

## Clocks (covered in depth)

- [[concept-time-of-day-clock]]
- [[concept-monotonic-clock]]
- [[concept-ntp]]
- [[concept-leap-second]]
- [[concept-last-write-wins-timestamp-risk]]
- [[concept-logical-clock]]
- [[concept-process-pause]]

# Section notes (wiki body)

## 1. Unreliable networks

### Detecting faults and timeouts

- A node cannot distinguish **slow** from **dead**; **timeouts** are the practical approach.
- **Long timeout** → fewer false positives, slower failover.
- **Short timeout** → faster failover, more false positives.
- Network delay has **no hard upper bound** in practice.

### Why delay varies

- **Congestion** and queueing on links and switches.
- Busy **CPU** on the receiver (request queued).
- **Virtualization** (VM paused by hypervisor).
- **Flow control** (sender throttles).

Delays must be tuned empirically; public clouds share resources with noisy neighbors.

### Synchronous vs asynchronous networks

- **Circuit switching** (classic telephone): dedicated bandwidth for the call duration — predictable but wasteful for bursty workloads.
- **Packet switching** (Ethernet/IP/TCP): statistical multiplexing, queues, variable delay — good for datacenter traffic, bad for assuming fixed latency.

## 2. Unreliable clocks

Applications use time for **durations** (timeouts, latency, QPS windows) and **points in time** (publish date, cache expiry, log timestamps). In distributed systems:

- Messages are not instantaneous; **order across machines** is unclear.
- Each machine has its own **quartz clock** (drift).
- **NTP** synchronizes time-of-day clocks but with limited accuracy and failure modes.

### Time-of-day (wall clock)

- Calendar time since epoch (e.g. `System.currentTimeMillis()`, `CLOCK_REALTIME`).
- Often NTP-synced between machines, but can **jump backward** or forward.
- **Leap seconds** and coarse historical resolution on some OSes make it poor for **elapsed time**.
- Dangerous for **ordering events** across nodes (see LWW).

### Monotonic clock

- Always moves forward on **one machine** (`nanoTime()`, `CLOCK_MONOTONIC`).
- Good for **measuring intervals** and local timeouts.
- **Meaningless to compare** across different computers.
- NTP may **slew** rate slightly; cannot jump backward.
- Still does not run your **application code** during pauses.

### Clock synchronization pitfalls

- Quartz **drift** (temperature-dependent); limits best-case accuracy.
- Forced **clock reset** if too far from NTP → time goes backward or jumps.
- Firewall blocks NTP → drift unnoticed.
- Network jitter limits NTP accuracy on the internet (tens of ms to ~seconds).
- Bad NTP servers, **leap seconds**, **VM time virtualization** (clock jumps when VM resumes).

**Monitor clock offset** between nodes; eject nodes with broken clocks before silent data loss.

### Timestamps for ordering (LWW)

In multi-leader replication, tagging writes with **local wall-clock time** and keeping the “latest” write (**last write wins**) can **drop newer writes** when clocks skew (Fig. 8-3: increment lost because timestamp looks older).

Problems:

- Silent loss with lagging clocks.
- Cannot separate **causal** sequence from true concurrency without **version vectors**.
- Tie-breakers on equal timestamps can violate causality.

**Logical clocks** order events by **happened-before**, not by wall time — safer for ordering.

### Clock readings as intervals

A timestamp is not a point with infinite precision; it has a **confidence interval** (earliest…latest). Microsecond digits may be meaningless if uncertainty is ±100 ms.

**TrueTime** (Spanner): returns `[earliest, latest]`; waits for interval to pass before commit so snapshots do not violate causality — expensive (GPS/atomic clocks per datacenter).

### Process pauses

Even correct clocks do not help if the **process stops running**:

- Leader holds a **lease**; must renew before expiry.
- Code checks lease, then **GC / VM suspend / I/O / context switch** pauses the thread for seconds.
- Lease expires elsewhere; **another leader** is elected.
- Thread resumes, still thinks it is leader, **processes writes unsafely**.

Causes: stop-the-world **GC**, **VM migration**, laptop sleep, **OS scheduling**, synchronous **disk/network I/O**, **swap**, `SIGSTOP`, etc.

Analogous to multi-threading: you cannot assume timing between two lines of code. Distributed systems have **no shared memory** — only messages and timeouts.

**Hard real-time** (RTOS, WCET analysis) can bound pauses in embedded systems; most servers accept non-real-time behavior and must design accordingly.

Mitigations: coordinated GC, rolling restarts, traffic shift before GC — reduce impact, do not eliminate the model.

# Important Tradeoffs

| Area | Tradeoff |
|------|----------|
| Timeouts | Fast failover vs false failure detection |
| LWW + timestamps | Simple conflict resolution vs silent data loss |
| NTP | Cheap sync vs drift, jumps, and bounded accuracy |
| Leases + wall clock | Simple leadership vs clock skew and pauses |
| TrueTime / Spanner | Strong global ordering vs cost and complexity |

# What Changed My Mental Model

- **Timeout ≠ proof of death**; it is a bet with a tradeoff.
- **Monotonic** for duration on one node; **time-of-day** for human time, not for distributed ordering.
- **Process pause** is a separate failure mode from **clock skew**: the node can be “alive” but not making progress.
- **LWW** with timestamps is tempting and dangerous.

# Open Questions

- How do production systems choose timeout values (adaptive, percentiles)?
- How do etcd / ZooKeeper / Consul combine leases, fencing, and clocks?
- Remaining sections of Ch. 8 (beyond process pauses): to be added when read.

# Related Notes

- [[map-distributed-systems]]
- [[map-databases]]
- Raw: `raw/chapter-8.md`
