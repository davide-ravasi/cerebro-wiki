---
id: reading-software-architecture-monday
title: "Software Architecture Monday"
subtitle: "10-minute video series on software architecture patterns and tradeoffs"
type: reading-queue
authors: ["Mark Richards"]
format: video-series
year_published: 2020-present
episodes: 222
platform: YouTube
language: en
domain: software-engineering
tags: [reading-queue, video-series, software-architecture, patterns, system-design]
status: queued
priority: low
updated: 2026-08-08
found_via: "YouTube discovery during architecture research"
why: "10-min architectural patterns and tradeoffs — practical companion to DDIA theory; prepares for Head First SA book. Mark Richards is co-author of Head First SA."
start_when: "After DDIA Ch. 9-12 complete; before or alongside Head First Software Architecture book"
related_skills: []
related_concepts: []
source_slug: sources/software-architecture-monday/
urls:
  channel: "https://www.youtube.com/@markrichards5014"
  lessons: "https://developertoarchitect.com/lessons"
related_books:
  - "Head First Software Architecture (Gandhi, Richards, Ford, 2024)"
  - "Fundamentals of Software Architecture (Richards & Ford)"
---

# About

**Software Architecture Monday** is a free YouTube series by Mark Richards (51.2K subscribers, 222 videos) delivering focused 10-minute lessons on software architecture topics.

| Field | Value |
|-------|--------|
| **Author** | Mark Richards (@markrichards5014) |
| **Format** | Video series (10-min episodes) |
| **Platform** | YouTube + developertoarchitect.com |
| **Episodes** | 222+ (ongoing) |
| **Cadence** | Weekly (Mondays) |
| **Level** | Intermediate to advanced |

Mark Richards is an independent software architect, author, and conference speaker — co-author of **Head First Software Architecture** and **Fundamentals of Software Architecture**.

# What it covers

- Architectural patterns (event-driven, microservices, layered, etc.)
- System design tradeoffs and decision frameworks
- Practical architecture problems and solutions
- Distributed systems challenges
- Communication and documentation for architects

**Not covered:** Deep distributed data theory (that's DDIA), code-level patterns (that's Fowler)

# Fit with current learning path

## ✅ Strategic value

| Dimension | Why this resource |
|-----------|-------------------|
| **Complements DDIA** | DDIA = how systems work internally; SAM = how to design and choose patterns |
| **Prepares for Head First SA** | Same author; video format = gentle intro before book deep dive |
| **Supports "traiettoria lead"** | Architecture vocabulary and tradeoff thinking for senior/lead conversations |
| **Format fits pomodoros** | 10-min video + 15-min notes/reflection = perfect 25-min slot |

## ⚠️ Important constraints

**DO NOT start alongside DDIA** — risk of focus dispersion.

### Recommended integration strategy

```
Timeline:
1. ✅ Finish DDIA Ch. 9 (in progress)     ← YOU ARE HERE
2. Complete DDIA Ch. 10-12 (consensus, batch/stream processing)
3. 🎬 START SAM: 1-2 videos/week in Friday slot (instead of Mongo when done)
4. 📚 After 8-12 weeks of SAM: Head First Software Architecture (full book)
```

### Weekly rhythm (when active)

| Slot | Activity |
|------|----------|
| **Friday pomodoro 1** | Watch 1 SAM video (10 min) + take notes in Italian raw (15 min) |
| **Friday pomodoro 2** (optional) | Watch 2nd video OR promote previous lesson to concepts/patterns |

Aim for **1-2 lessons per week** = sustainable pace over 2-3 months.

# Workflow when you start

## 1. Update status

Set `status: reading` and `updated: YYYY-MM-DD`

## 2. Create source structure

```bash
mkdir -p sources/software-architecture-monday/raw
mkdir -p sources/software-architecture-monday/promoted
```

## 3. Note template

**Raw notes** (Italian, one file per lesson):

```
sources/software-architecture-monday/raw/lesson-NNN-topic-slug.md
```

Frontmatter:
```yaml
---
id: sam-raw-lesson-NNN
title: "Lesson NNN - [Title]"
type: source-note
source: software-architecture-monday
episode: NNN
date_published: YYYY-MM-DD
duration: "13:13"
tags: [sam, [pattern-name], raw-notes]
status: raw
language: it
updated: YYYY-MM-DD
---
```

**Promoted notes** (English, when a lesson introduces a key pattern):

Extract to `concepts/architecture/` or `patterns/architecture/` following existing templates.

## 4. Link from maps

Add SAM lessons to:
- `maps/reading-map.md` (Active section)
- Consider creating `maps/architecture-map.md` after 10+ lessons

# Topics to prioritize (when scanning)

Given your DDIA background and tracking-ds/track-em-all projects:

- Event-driven architecture patterns
- Microservices vs modular monolith tradeoffs
- API gateway patterns
- Saga pattern (distributed transactions)
- CQRS and event sourcing
- Architectural documentation
- Layered architecture evolution

Skip or skim: Enterprise-specific patterns (ESB, SOA) unless curious.

# Chat hooks

## Starting a session

> "Riprendo Software Architecture Monday — [[reading-software-architecture-monday]] — lesson NNN su [topic]."

## Using learning modes

- First pass: just watch + rough notes (no skill needed)
- If confused about a pattern: `@learn-core-idea-first` + lesson hook
- To test understanding: `@learn-error-simulator` + pattern name
- For practical application: `@learn-operational-fast` + "apply [pattern] to tracking-ds"

## Spiega-lead integration

After 4-6 weeks of SAM, add to your bi-weekly "Spiega-lead" candidates:
- "Event-driven architecture tradeoffs"
- "When to split a monolith"
- "Saga vs 2PC for distributed transactions"

# When finished (222 videos = ~1 year if weekly)

1. Set `status: done`
2. You'll have:
   - ~50-100 raw lesson notes
   - ~15-25 promoted architecture patterns/concepts
   - Strong foundation for Head First SA book
3. Consider: **Fundamentals of Software Architecture** (Richards & Ford) for deeper dive

# Notes

- Don't binge — architecture patterns need time to absorb and connect to real projects
- Watch videos at 1.25x-1.5x if Mark's pace feels slow
- After each video, ask: "How does this apply to tracking-ds or track-em-all?"
- Most valuable when you alternate: 1 week theory (DDIA/SAM) → 1 week practice (projects)

---

*Created: 2026-08-08 — Queued after YouTube discovery; start after DDIA Ch. 9-12*
