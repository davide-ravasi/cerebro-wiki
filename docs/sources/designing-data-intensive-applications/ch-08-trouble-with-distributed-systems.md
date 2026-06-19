---
id: source-ddia-ch-08
title: "DDIA Chapter 8: The Trouble with Distributed Systems"
type: source
domain: distributed-systems
sources:
  - kind: book
    title: "Designing Data-Intensive Applications"
    chapter: 8
    author: "Martin Kleppmann"
tags: [source, ddia, distributed-systems, networks, clocks, faults, quorum, fencing]
status: draft
confidence: medium
updated: 2026-05-19
---

# Summary

Distributed systems must assume **partial failure**, **unbounded network delay**, and **unreliable time**. A single node cannot distinguish slow from dead; **timeouts** are heuristics. **Wall-clock timestamps** are unsafe for ordering; **process pauses** break leases. **Truth is quorum-defined**, not local — use **fencing tokens** when stale leaders might write. **Byzantine faults** are a stronger (rarer in practice) model than crash faults.

Raw working notes (Italian, handwritten scan → markdown): `raw/chapter-8.md`.

# Key Concepts

## Networks and faults

- [[concept-timeout-failure-detection]]

## Clocks and time

- [[concept-time-of-day-clock]]
- [[concept-monotonic-clock]]
- [[concept-ntp]]
- [[concept-leap-second]]
- [[concept-last-write-wins-timestamp-risk]]
- [[concept-logical-clock]]
- [[concept-clock-confidence-interval]]
- [[concept-process-pause]]

## Knowledge, truth, and safety

- [[concept-quorum-majority-truth]]
- [[concept-fencing-token]]
- [[concept-byzantine-fault]]

# Section notes (wiki body)

## 1. Faults and partial failures

- Single machine: works or broken. Distributed: **partial failures** — nondeterministic, something always broken in large clusters.
- **Cloud** model: commodity hardware, IP/Ethernet, geographic distribution, fault tolerance as requirement.
- **Supercomputing** model: specialized hardware/network — different assumptions.

## 2. Unreliable networks

### Shared-nothing and async packets

- Nodes communicate only via network. Request/response can fail in many indistinguishable ways (lost, queued, node dead, slow, response lost/delayed).
- **Cannot tell why** — assume **no response**; use **timeout** (not proof of failure).

### Detecting faults and timeouts

- Occasional feedback (OS, crash scripts, management NIC, ICMP) — **do not rely** on it.
- **Long timeout** → fewer false positives, slower failover.
- **Short timeout** → faster failover, more false positives.
- Delays are **unbounded** in practice.

### Why delay varies

- **Congestion** and queueing on links and switches.
- Busy **CPU** on the receiver (request queued).
- **Virtualization** (VM paused by hypervisor).
- **Flow control** (sender throttles).

App sees **resulting delay**, not always packet loss. Tune timeouts **experimentally**; public clouds share resources with noisy neighbors.

### Synchronous vs asynchronous networks

- **Circuit switching** (classic telephone): dedicated bandwidth for the call duration — predictable but wasteful for bursty workloads.
- **Packet switching** (Ethernet/IP/TCP): statistical multiplexing, queues, variable delay — good for datacenter traffic, bad for assuming fixed latency.

## 3. Unreliable clocks

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

### Clock readings as intervals and global snapshots

A timestamp is not a point with infinite precision; it has a **confidence interval** (earliest…latest). Microsecond digits may be meaningless if uncertainty is ±100 ms. Most systems **do not expose** this uncertainty.

For a **global snapshot** at time T across partitions, naive synchronized clocks can disagree on what “at T” means. **TrueTime** (Spanner): returns `[earliest, latest]`; **commit wait** until uncertainty passes so snapshots and commits do not violate causality — expensive (GPS/atomic clocks per datacenter). See [[concept-clock-confidence-interval]].

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

## 4. Knowledge, truth, and lies

A distributed system has:

- No **shared memory**
- **Unreliable network** with variable delay
- No reliable view of a **non-responding** node’s state
- **Process pauses** and **unreliable clocks**

You cannot trust a single node’s local judgment alone.

## 5. The truth is defined by the majority

A single node may:

- **Receive** but not **send** (asymmetric partition)
- Experience a long **stop-the-world GC**

Therefore decisions (leader, lock, registration) require **quorum** agreement — see [[concept-quorum-majority-truth]]. A node that **believes** it is leader without quorum backing is dangerous.

## 6. The leader, the lock, and fencing tokens

Systems often require exactly **one** leader, **one** lock holder, or **one** successful registration.

**Local belief ≠ cluster truth.** After lease expiry and failover, a delayed former leader must not corrupt shared state. **Fencing tokens** (monotonic numbers included in every write; storage rejects stale tokens) — see [[concept-fencing-token]].

## 7. Byzantine faults

**Byzantine fault tolerance:** correct operation even when nodes malfunction, disobey the protocol, or act maliciously. **Complicated**, often needs hardware support; uncommon in typical datacenter crash-fault designs. Web apps must still validate **untrusted user input** — related but not the same as inter-node BFT. See [[concept-byzantine-fault]].

# Important Tradeoffs

| Area | Tradeoff |
|------|----------|
| Timeouts | Fast failover vs false failure detection |
| LWW + timestamps | Simple conflict resolution vs silent data loss |
| NTP | Cheap sync vs drift, jumps, and bounded accuracy |
| Leases + wall clock | Simple leadership vs clock skew and pauses |
| TrueTime / Spanner | Strong global ordering vs cost and complexity |
| Quorum decisions | Safety vs latency and availability during partitions |
| Fencing tokens | Stale leader protection vs storage integration |
| Byzantine tolerance | Strongest fault model vs complexity and cost |

# What Changed My Mental Model

- **Timeout ≠ proof of death**; it is a bet with a tradeoff.
- **Monotonic** for duration on one node; **time-of-day** for human time, not for distributed ordering.
- **Process pause** is a separate failure mode from **clock skew**: the node can be “alive” but not making progress.
- **LWW** with timestamps is tempting and dangerous.
- **Global snapshot at T** needs **clock uncertainty**, not naive NTP equality.
- **Truth is quorum-defined**; fencing protects storage from false local belief.
- **Byzantine** is the extreme case; most systems assume crash faults plus untrusted clients.

# Open Questions

- How do production systems choose timeout values (adaptive, percentiles, Phi accrual)?
- How do etcd / ZooKeeper / Consul combine leases, fencing, and clocks in practice?
- When is vector clock worth the metadata vs Lamport only?

# Related Notes

- [[map-distributed-systems]]
- [[map-databases]]
- Raw: `raw/chapter-8.md`
