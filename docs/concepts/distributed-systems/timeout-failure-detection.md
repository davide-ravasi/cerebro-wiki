---
id: concept-timeout-failure-detection
title: "Timeout-Based Failure Detection"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, networks, faults, timeouts]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

In distributed systems, a node often cannot tell whether a peer is **dead** or merely **slow**. **Timeouts** declare failure when no response arrives within a limit. This is a **heuristic**, not a proof of failure.

# Why It Matters

Without timeouts, a single slow or stuck peer can block resources indefinitely. With timeouts, you trade **availability** and **failover speed** against **false positives** (declaring alive nodes dead).

# How It Works

Delays are **unbounded** in practice: congestion, queueing, receiver CPU load, VM pauses, flow control. Operators tune timeout values from measurements; adaptive strategies may use load and latency percentiles.

- **Long timeout** → fewer false alarms, slower detection of real failures.
- **Short timeout** → faster failover, more risk of marking slow nodes as dead.

# Tradeoffs

- **Pros:** simple, universal, enables progress.
- **Cons:** wrong guesses; does not distinguish overload from crash; coupled to unpredictable network and **[[concept-process-pause]]** on the remote side.

# When To Use

Always in request/response over unreliable networks — but choose values deliberately and monitor tail latency.

# Example

A overloaded database responds after 60 s; with a 5 s timeout the client sees “failure” and may retry, amplifying load.

# Related Concepts

- [[concept-process-pause]]
- [[map-distributed-systems]]
