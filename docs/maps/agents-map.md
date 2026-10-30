---
id: map-agents-lab
title: "Agents Lab Map"
type: map
domain: ai
tags: [map, navigation, ai, agents, cursor-sdk]
status: evergreen
updated: 2026-10-30
---

# Purpose

Path snello per agenti **utili al lavoro**: Cursor SDK + vocabolario Udemy selezionato. Non sostituisce DDIA/track.

# Programma

- Indice + checklist pomodori: [`sources/ai-agents/README.md`](../sources/ai-agents/README.md) ([[source-ai-agents-index]])
- Lab notes: [`sources/ai-agents/raw/`](../sources/ai-agents/raw/)

# Core Concepts (wiki)

- [[concept-claude-md]] — project memory (AGENTS.md / CLAUDE.md)
- Agent vs workflow · harness vs model · AGENTS.md vs skills — **ripasso ✓ 21/10** (foto Max)
- local vs cloud ✓ 30/10 — `cwd` disco vs `repos` VM; ID `agent-` vs `bc-`

# Practical Patterns

- Lab repo: `../ai-lab` — `#1` prompt ✓ · `#2` create+send ✓ · `#3` errori ✓ · `#4` real-repo ✓ 16/10 · `#5` cloud ✓ 30/10 (`bc-…`, VM vs `cwd`)  
  Trap #4: cwd foglia → SQLite `unable to open database file`; cwd = root repo + prompt stretto  
  Trap #5: senza `cloud` l’SDK fa local in silenzio
- Cadenza: Ven 1×25, XOR track/Mongo

# Source / corso

- Udemy: *AI Agents & Workflows — The Practical Guide*
  - §1 Getting Started: **tutto ☑**
  - §2 *Using & Steering*: **tutto ☑** 20/10 (E+F). Hook skills: progressive disclosure · `name`+`description` · AGENTS.md = README for agents
  - §3 Overview (#21–25, ~10′): **dopo #5** — ☐; pezzo utile #23 WHO/WHERE
  - §4 Workflows (~2h30) · §5 Building Agents (~2h): **skip path** (curiosità 1 video max)
  - §6 CrewAI: **dopo #8b** only

- Docs: https://cursor.com/docs/sdk/typescript
- Articolo harness ☑ 09/10

# Open Threads

- Lab **#6** resume / run id. Path 7 → 7b HITL → 8 se serve → **8b capstone** → 9. Non un secondo corso.
- Ripasso foto Mer ✓ 21/10
- **Workshop D-Edge 16/10** (appunti private): `docs/private/workshop-morning-agent-2026-10-16.md` · repo `C:\Users\dravasi\Documents\morning-agent` — riprendere per lab #7 / skill vs agent
- Surplus: foto spartito → INSERT (ok 02/10)
- Idea later: chat UI + tracking (dopo #5)
