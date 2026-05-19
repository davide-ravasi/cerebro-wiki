---
id: concept-leap-second
title: "Leap Second"
type: concept
domain: distributed-systems
sources:
  - note: source-ddia-ch-08
tags: [concept, distributed-systems, clocks, time, utc]
status: evergreen
confidence: medium
updated: 2026-05-13
---

# Definition

A **leap second** is an occasional **one-second adjustment** to civil time (**UTC**) so it stays aligned with Earth’s rotation, which is not perfectly constant. UTC can insert an extra second (e.g. `23:59:60`) or, rarely, remove one.

# Why It Matters

It illustrates why **wall-clock time** is a poor foundation for strict distributed logic: a minute may have **59 or 61 seconds**, timestamps can **repeat** or **jump**, and “time always moves forward” is **not** guaranteed at the OS level.

# How It Works

- Atomic time (very stable) defines the base scale.
- Earth rotation slows slightly over long periods.
- When the gap grows, international bodies schedule a leap second in UTC.
- Operating systems and libraries must handle the extra or missing second; many applications assume 60 seconds per minute.

**Note:** UTC is planned to **stop using leap seconds** by around 2035, but the lesson remains: civil time is a **policy**, not a smooth physical measure.

# Tradeoffs

- **Pros (for humans):** keeps solar/civil time roughly aligned with the sky.
- **Cons (for software):** complicates logging, scheduling, databases, and any code that assumes monotonic wall time.

# When To Use

You do not “use” leap seconds in application design—you **design around** unreliable wall clocks: margins on comparisons, logical timestamps, or monotonic timers for durations.

# Example

Two events logged one second apart might sort incorrectly if a leap second makes `23:59:60` appear between them, or if the OS steps the clock backward during correction.

# Related Concepts

- [[concept-time-of-day-clock]]
- [[concept-ntp]]
- [[map-distributed-systems]]

# Related Patterns

- Prefer **logical clocks** (Lamport, version vectors) when defining “happened before” across nodes.
