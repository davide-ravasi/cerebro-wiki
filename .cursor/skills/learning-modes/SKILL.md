---
name: learning-modes
description: >-
  Router for three learning/teaching modes: operational fast path, error
  simulator, core-idea-first gate. Use when the user asks which learning mode
  to use, mentions "AI Geniale" prompts, or wants help studying DDIA/Mongo/wiki
  topics without default lecture style.
disable-model-invocation: true
---

# Learning modes (picker)

Choose **one** mode based on user state:

| Mode | Skill | When |
|------|-------|------|
| **Operational fast** | `learn-operational-fast` | "I need to use this soon", exam, interview, ship feature |
| **Error simulator** | `learn-error-simulator` | "Test me", "I think I get it but…", practice over theory |
| **Core idea first** | `learn-core-idea-first` | "I'm confused", dense chapter, linearizability-style fog |

## How to invoke

User can `@learn-operational-fast`, `@learn-error-simulator`, `@learn-core-idea-first`, or say:

- "learning curve destroyer" → operational fast
- "error simulator" / "simulatore errori" → error simulator  
- "traduttore" / "core idea" → core idea first

## Cerebro wiki

After understanding, offer to capture in `docs/` (raw IT → concept EN) per [docs/README.md](../../docs/README.md) workflow — only if user wants wiki promotion.

## Child skills

- [learn-operational-fast](../learn-operational-fast/SKILL.md)
- [learn-error-simulator](../learn-error-simulator/SKILL.md)
- [learn-core-idea-first](../learn-core-idea-first/SKILL.md)

## Guida articoli e lettura

Per studiare **articoli / capitoli** con le tre fasi (essenza → applicazione → verifica): [guida-articoli-e-lettura.md](./guida-articoli-e-lettura.md)

In chat: `@learning-modes guida articoli` oppure `Segui guida-articoli-e-lettura.md`.

## Tecniche esterne (mappa)

Feynman / Socratico / test attivo / analogie ↔ le nostre skill: [tecniche-apprendimento.md](./tecniche-apprendimento.md)
