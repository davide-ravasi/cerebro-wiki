---
id: map-agents-lab
title: "Agents Lab Map"
type: map
domain: ai
tags: [map, navigation, ai, agents, cursor-sdk]
status: evergreen
updated: 2026-10-16
---

# Purpose

Path snello per agenti **utili al lavoro**: Cursor SDK + vocabolario Udemy selezionato. Non sostituisce DDIA/track.

# Programma

- Indice + checklist pomodori: [`sources/ai-agents/README.md`](../sources/ai-agents/README.md) ([[source-ai-agents-index]])
- Lab notes: [`sources/ai-agents/raw/`](../sources/ai-agents/raw/)

# Core Concepts (wiki)

- [[concept-claude-md]] — project memory (AGENTS.md / CLAUDE.md)
- *(TBD: agent vs workflow — learning Mer 21/10 su screenshot; local vs cloud dopo lab #5)*

# Practical Patterns

- Lab repo: `../ai-lab` — `#1` prompt ✓ · `#2` create+send ✓ · `#3` errori ✓ · `#4` real-repo ✓ 16/10  
  Trap #4: cwd foglia → SQLite `unable to open database file`; cwd = root repo + prompt stretto
- Cadenza: Ven 1×25, XOR track/Mongo

# Source / corso

- Udemy: *AI Agents & Workflows — The Practical Guide*
  - §1 Getting Started: **tutto ☑** (= A+B; niente blocco C separato)
  - §2 Using & Steering: **#12–16 ☑** 16/10 · prossimo **#20 HITL** · poi #17–18 (F)
- Docs: https://cursor.com/docs/sdk/typescript
- Articolo harness ☑ 09/10

# Open Threads

- Video: chiudere **E** con #20 · poi **F** (#17–18)
- Learning session ~25′ (**Mer 21/10**): screenshot A/B + harness → agente vs workflow
- Lab **#5** cloud
- Surplus: foto spartito → INSERT (ok 02/10)
- Idea later: chat UI + tracking (dopo #5)
