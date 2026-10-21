---
id: map-agents-lab
title: "Agents Lab Map"
type: map
domain: ai
tags: [map, navigation, ai, agents, cursor-sdk]
status: evergreen
updated: 2026-10-21
---

# Purpose

Path snello per agenti **utili al lavoro**: Cursor SDK + vocabolario Udemy selezionato. Non sostituisce DDIA/track.

# Programma

- Indice + checklist pomodori: [`sources/ai-agents/README.md`](../sources/ai-agents/README.md) ([[source-ai-agents-index]])
- Lab notes: [`sources/ai-agents/raw/`](../sources/ai-agents/raw/)

# Core Concepts (wiki)

- [[concept-claude-md]] — project memory (AGENTS.md / CLAUDE.md)
- Agent vs workflow · harness vs model · AGENTS.md vs skills — **ripasso ✓ 21/10** (foto Max)
- *(TBD: local vs cloud dopo lab #5)*

# Practical Patterns

- Lab repo: `../ai-lab` — `#1` prompt ✓ · `#2` create+send ✓ · `#3` errori ✓ · `#4` real-repo ✓ 16/10  
  Trap #4: cwd foglia → SQLite `unable to open database file`; cwd = root repo + prompt stretto
- Cadenza: Ven 1×25, XOR track/Mongo

# Source / corso

- Udemy: *AI Agents & Workflows — The Practical Guide*
  - §1 Getting Started: **tutto ☑** (= A+B; niente blocco C separato)
  - §2 *Using & Steering* (numeri curriculum, riallineati 20/10):

| # | Titolo | Stato |
|---|--------|:-----:|
| 12 | *Three Main Ways…* | ☑ |
| 13–15 | *Managing Agent Tools…* · *Choosing The Right Model* (+ collegati) | ☑ |
| 16 | *Developer-provided System Instructions* | ☑ |
| 17–18 | *AGENTS.md / CLAUDE.md* · *Agent Skills* | ☑ **20/10** |
| 20 | *Humans In The Loop* | ☑ **20/10** |
| — | *Memory* (notes) | ☑ appunti 20/10 |

  **§2 video path chiuso** (E+F). Hook appunti skills: progressive disclosure · `name`+`description` only required · description = when to reach, not human docs · AGENTS.md = README for agents / shared convention.

- Docs: https://cursor.com/docs/sdk/typescript
- Articolo harness ☑ 09/10

# Open Threads

- Lab **#5** cloud · skill pratica (morning-agent / lab #7)
- Ripasso foto Mer ✓ 21/10
- **Workshop D-Edge 16/10** (appunti private): `docs/private/workshop-morning-agent-2026-10-16.md` · repo `C:\Users\dravasi\Documents\morning-agent` — riprendere per lab #7 / skill vs agent
- Surplus: foto spartito → INSERT (ok 02/10)
- Idea later: chat UI + tracking (dopo #5)
