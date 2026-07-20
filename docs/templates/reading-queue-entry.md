---
id: reading-<book-slug>
title: "<Book Title>"
type: reading-queue
authors: ["<Author>"]
edition: "<e.g. 2nd ed. 2018 — optional>"
year_published: YYYY
publisher: "<Publisher or Independent>"
isbn_13: "<optional>"
pages: <optional>
language: en
domain: <software-engineering|distributed-systems|databases|web|ai|other>
tags: [reading-queue, books, <topic-slug>]
status: queued
priority: medium
updated: YYYY-MM-DD
found_via: "<optional — article, video, colleague>"
why: "<one sentence — why you want to read it>"
start_when: "<optional — trigger>"
related_skills: []
related_concepts: []
source_slug: "<optional — sources/<book-slug>/ when reading starts>"
urls:
  book: "<optional official URL>"
---

# Bibliography

| Field | Value |
|-------|--------|
| **Authors** | … |
| **Edition / year** | … |
| **Publisher** | … |
| **ISBN-13** | … |
| **Pages** | … |
| **URLs** | … |

# Notes

# When you start reading

1. Set `status: reading` and `updated`.
2. Create `sources/<book-slug>/` (copy structure from DDIA or MongoDB paths).
3. Add `raw/` for Italian working notes; promote to English `source` + `concepts/` when reviewed.
4. Link the new source index from [[map-reading]] **Active** section.

# When finished

1. Set `status: done`.
2. Keep `source_slug` pointing at your source notes.
3. Move link from **Queue** to **Done** on [[map-reading]].
