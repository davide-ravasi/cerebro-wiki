---
id: concept-process-pause
title: "Process Pause"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, faults, leader-election]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

A **process pause** is an interruption where a thread or process **stops executing** for a significant period while **real time** continues. The process often **does not notice** until it resumes and checks clocks later.

# Why It Matters

Breaks assumptions behind **leases**, **leader election**, and “I checked the lease then processed the request.” Even with **[[concept-monotonic-clock]]** and **[[concept-ntp]]**, a paused leader can process writes after another node became leader → **split brain** and data corruption.

# How It Works

Common causes:

- Stop-the-world **garbage collection**
- **VM** suspend/resume (live migration, hypervisor)
- **OS** context switches and CPU **steal time** under load
- Synchronous **disk or network I/O**
- **Swap** / memory pressure
- Signals such as **SIGSTOP**

During pause: no heartbeat, no lease renewal, no request handling — peers may **timeout** and fail over.

# Tradeoffs

- **Hard real-time** systems (RTOS, WCET) can bound pauses — expensive, rare in typical servers.
- **Soft mitigations** (coordinated GC, rolling restart) reduce impact but do not remove the failure mode.

# When To Use

Design every distributed safety property assuming: **“This node may freeze at any instruction for a long time.”** Use fencing tokens, quorum, version checks — not only local lease checks.

# Example

Leader checks lease (10 s buffer OK), **GC pauses 15 s**, processes request while lease expired elsewhere — unsafe write as “leader.”

# Related Concepts

- [[concept-timeout-failure-detection]]
- [[concept-monotonic-clock]]
- [[concept-time-of-day-clock]]
- [[concept-reliability]]
- [[map-distributed-systems]]
