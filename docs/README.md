# Personal Knowledge Base

License for original material in this repository: **MIT** — see [`LICENSE`](../LICENSE) in the repo root. (Summaries and notes are yours; do not treat third-party books or course materials as covered by this license for republication.)

This folder is a Markdown-first knowledge base designed for:

- fast retrieval in Cursor and AI chats
- long-term maintainability across many books
- clean publication on a public GitHub repository

## Folder Structure

- `inbox/`: quick captures, rough notes, ideas to process later
- `sources/`: source-bound notes (book chapter summaries, lecture notes, article notes)
- `sources/designing-data-intensive-applications/raw/`: raw DDIA chapter notes while reading (promote to formal `source` notes when you review)
- `sources/mongodb-nodejs-developer-path/`: MongoDB (Node.js path) course — see `README.md` there; raw lessons under `raw/`
- `concepts/`: atomic evergreen notes (one concept per file); subfolders by domain (e.g. `distributed-systems/`, `databases/`)
- `patterns/`: practical decision notes and tradeoffs (when to use what)
- `maps/`: map of content (MOC) pages that organize related notes
- `glossary/`: short canonical definitions
- `templates/`: reusable templates for every note type

## Core Principles

1. One note, one idea.
2. Separate source notes from concept notes.
3. Prefer links over duplication.
4. Keep metadata consistent with YAML frontmatter.
5. Optimize for future search and AI retrieval, not just for reading.

## Suggested Naming

- `sources/<book-slug>/ch-01-reliability-scalability-maintainability.md`
- `concepts/<topic>/<concept-slug>.md`
- `patterns/<domain>/<pattern-slug>.md`
- `maps/<domain>-map.md`
- `glossary/<term-slug>.md`

Use lowercase kebab-case file names.

## Required Frontmatter Fields

Every note should include at least:

- `id`
- `title`
- `type`
- `tags`
- `status`
- `updated`

Optional but recommended:

- `sources`
- `domain`
- `confidence`

## Recommended Workflow

1. Capture quickly in `inbox/`.
2. Promote to `sources/` with cleaned chapter/article notes.
3. Extract reusable concepts into `concepts/`.
4. Write practical usage decisions in `patterns/`.
5. Link everything from a domain map in `maps/`.
6. Keep glossary terms short and canonical in `glossary/`.

## Publishing Notes Publicly

- Prefer paraphrased summaries and your own examples.
- Avoid large verbatim excerpts from copyrighted books.
- Add your interpretation, tradeoffs, and real usage context.

## First Migration Target

Start from `sources/designing-data-intensive-applications/raw/` (chapter files):

1. keep chapter files as working notes; add formal `source` notes in `sources/designing-data-intensive-applications/` when reviewed
2. extract 2-3 key concepts per chapter
3. add them to `maps/distributed-systems-map.md` and `maps/databases-map.md` where relevant

This keeps migration incremental and low effort.
