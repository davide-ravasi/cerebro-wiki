# Reading queue

Books and long-form sources you plan to read, are reading, or have finished — with a **trace** you can pick up in chat or months later.

**Navigation hub:** [[map-reading]] (`maps/reading-map.md`)

## Lifecycle

| `status` | Meaning |
|----------|---------|
| `queued` | Want to read; not started |
| `reading` | In progress — notes under `sources/<book-slug>/` |
| `paused` | Stopped mid-way; resume notes still valid |
| `done` | Finished; source + concept notes remain the artifact |
| `dropped` | Decided not to read; keep one-line `why` for future you |

```text
queued → reading → done
           ↓ ↑
         paused
```

## Where things live

| Stage | Location |
|-------|----------|
| **Intent** (why, priority, links) | `reading/queue/<book-slug>.md` |
| **While reading** (raw IT notes) | `sources/<book-slug>/raw/` |
| **Promoted** (English wiki) | `sources/<book-slug>/`, `concepts/`, `patterns/` |
| **Navigation** | `maps/reading-map.md` + domain maps |

## Add a book

1. Copy [`templates/reading-queue-entry.md`](../templates/reading-queue-entry.md) → `reading/queue/<book-slug>.md`
2. Fill frontmatter (`why`, `found_via`, `priority`).
3. Add a row under **Queue** (or **Active**) in [`maps/reading-map.md`](../maps/reading-map.md).

Quick capture: drop title + link in `inbox/`, promote to `reading/queue/` when you have 2 minutes.

## Learning modes

When you start a queued book, use [`.cursor/skills/learning-modes`](../../.cursor/skills/learning-modes/SKILL.md):

- confused → `@learn-core-idea-first`
- practice → `@learn-error-simulator`
- ship / apply → `@learn-operational-fast`

Articles: [`learning-modes/guida-articoli-e-lettura.md`](../../.cursor/skills/learning-modes/guida-articoli-e-lettura.md)

## Language

- **Queue entries** — English (public wiki); `why` can be a short personal note.
- **Raw notes while reading** — Italian in `sources/**/raw/`, same as DDIA.
