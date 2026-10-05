---
id: map-agents-lab
title: "Agents Lab Map"
type: map
domain: ai
tags: [map, navigation, ai, agents, cursor-sdk]
status: evergreen
updated: 2026-10-02
---

# Purpose

Path snello per agenti **utili al lavoro**: Cursor SDK + vocabolario Udemy selezionato. Non sostituisce DDIA/track.

# Programma

- Indice + checklist pomodori: [`sources/ai-agents/README.md`](../sources/ai-agents/README.md) ([[source-ai-agents-index]])
- Lab notes: [`sources/ai-agents/raw/`](../sources/ai-agents/raw/)

# Core Concepts (wiki)

- [[concept-claude-md]] — project memory (AGENTS.md / CLAUDE.md)
- *(TBD dopo lab 2–4: agent vs workflow, local vs cloud runtime)*

# Practical Patterns

- Lab repo: `../ai-lab` — `Agent.prompt` local ✓ · `create`+`send` multi-turno ✓ 02/10
- Cadenza: Ven 1×25, XOR track/Mongo

# Source / corso

- Udemy: *AI Agents & Workflows — The Practical Guide* → solo building blocks (vedi tabella “prendere / saltare” nell’indice)
- Docs: https://cursor.com/docs/sdk/typescript

# Open Threads

- Lab #3 errori (`CursorAgentError` vs `status === "error"`)
- Video Max: **A ☑** · prossimo **B**
- Cloud su repo reale
- Surplus: foto spartito → INSERT (prompt dedicato, testato 02/10)
- Collegare a tracking-ds / Playwright quando c’è un job ripetibile
