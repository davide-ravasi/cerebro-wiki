# Software Architecture Monday — Source Notes

Mark Richards' 10-minute video series on software architecture patterns and tradeoffs.

**Queue entry:** [`reading/queue/software-architecture-monday.md`](../../reading/queue/software-architecture-monday.md)  
**Channel:** [Software Architecture Monday (@markrichards5014)](https://www.youtube.com/@markrichards5014)  
**Website:** [developertoarchitect.com/lessons](https://developertoarchitect.com/lessons)

---

## ⚠️ Status: Optional / Nice-to-have (2026-08-10)

After time/ROI analysis: **SAM is optional, not core learning path.**

- **Issue:** 222 videos = 2+ years with your 4h/week budget; significant overlap with Head First SA (same author)
- **Recommendation:** If interested, cherry-pick **10-15 videos** on specific patterns (see queue entry for list)
- **Alternative:** Go straight to Head First SA book after Fowler + Pragmatic (better structured, less time)

**Use this folder only if you decide to watch some SAM videos.**

---

## Structure

- **`raw/`** — Italian notes, one file per lesson (format: `lesson-NNN-topic-slug.md`)
- **`promoted/`** — English summaries when a lesson introduces a key reusable pattern
- Extracted patterns → `concepts/architecture/` or `patterns/architecture/`

---

## Workflow

### 1. Watch + capture (Friday pomodoro 1)

1. Watch one 10-min video (at 1x or 1.25x speed)
2. Take rough notes in `raw/lesson-NNN-topic-slug.md` (Italian)
3. Frontmatter template:

```yaml
---
id: sam-raw-lesson-NNN
title: "Lesson NNN - [Title from video]"
type: source-note
source: software-architecture-monday
episode: NNN
date_published: YYYY-MM-DD
duration: "MM:SS"
tags: [sam, [main-pattern], raw-notes]
status: raw
language: it
updated: YYYY-MM-DD
---
```

4. Sections:
   - `# Context` — what problem does this solve?
   - `# Key ideas` — main points (bullet list)
   - `# Tradeoffs` — pros/cons, when to use/avoid
   - `# Examples` — Mark's examples + how it applies to your projects
   - `# Links` — related concepts, other lessons, DDIA chapters

### 2. Promote (optional — Friday pomodoro 2 or later)

If a lesson introduces a **reusable architectural pattern**:

1. Create English note in `concepts/architecture/[pattern-slug].md` or `patterns/architecture/[pattern-slug].md`
2. Follow existing concept/pattern template structure
3. Link back to raw lesson: `sources: ["[[sam-raw-lesson-NNN]]"]`
4. Update relevant maps (e.g., `maps/architecture-map.md` when it exists)

Not every lesson needs promotion — focus on patterns you'll reference or explain to others.

---

## Priority topics (given your path)

Focus on lessons covering:

- ✅ Event-driven architecture
- ✅ Microservices vs modular monolith
- ✅ API gateway / BFF patterns
- ✅ Saga pattern (distributed transactions)
- ✅ CQRS / Event Sourcing
- ✅ Layered architecture
- ✅ Architectural documentation

Skip or skim: Enterprise-heavy patterns (ESB, SOA) unless curious.

---

## Integration with study rhythm

| Week | Activity |
|------|----------|
| **Before starting SAM** | Complete DDIA Ch. 9-12 first |
| **Weeks 1-4** | 1 video/week — get comfortable with format and Mark's style |
| **Weeks 5-12** | 1-2 videos/week — build pattern vocabulary |
| **After ~12 weeks** | Consider starting Head First Software Architecture book |

---

## Chat hooks

**Start session:**
> "Riprendo Software Architecture Monday — [[reading-software-architecture-monday]] — lesson NNN su [topic]."

**Using skills:**
- Confused: `@learn-core-idea-first` + lesson link
- Test understanding: `@learn-error-simulator` + pattern name
- Apply to project: `@learn-operational-fast` + "apply [pattern] to tracking-ds/track-em-all"

**Spiega-lead (bi-weekly):**  
After 6-8 lessons, add architecture topics to your "Spiega-lead" rotation.

---

## Notes

- **Don't binge** — 1-2 lessons/week is sustainable; patterns need time to absorb
- Watch at 1.25x-1.5x if Mark's pace feels slow
- After each lesson: "How does this apply to my current projects?"
- Most valuable when alternating theory (SAM) with practice (building)

---

## Index (update as you go)

*When you have 10+ lessons, create a simple index here:*

| Lesson | Title | Date watched | Key pattern | Promoted? |
|--------|-------|--------------|-------------|-----------|
| — | — | — | — | — |

Or use frontmatter + `rg` to query: `rg "^episode: " raw/ --no-filename | sort -n`

---

*Created: 2026-08-08 — Ready for when you start after DDIA Ch. 9-12*
