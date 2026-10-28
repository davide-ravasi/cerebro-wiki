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
- `Reflect:` → chiusura 3 righe (vedi sotto) — non è una skill a parte
- `Interleave:` → quiz misto su `@learn-error-simulator` (vedi sotto)

## Chiusure corte (nel flow, non skill nuove)

Ispirate a [agentic-learning](https://github.com/FavioVazquez/agentic-learning) (`reflect`, `interleave`). **Non** installare quella skill: scrivono `docs/revisit.md` e duplicano `study-resume`.

### Reflect

Trigger: `Reflect:` oppure fine sessione Mer/Gio se l’utente chiude.

Chiedi **solo** queste tre righe (l’utente le scrive; tu non le riempi al posto suo):

1. **Imparato** — una frase
2. **Obiettivo** — cosa volevo oggi
3. **Buco** — cosa è ancora opaco

Poi: se c’è un buco, proponi **una** riga in «Da ripassare» (o backlog). Non aprire un file nuovo.

### Interleave

Trigger: `Interleave:` + 2 temi (se manca il secondo, prendilo da «Da ripassare» o dal piano Mer/Gio).

Usa `@learn-error-simulator` con **uno** scenario che mescola i due temi (stessa domanda, due concetti). Una domanda, aspetta la risposta. Niente lezione prima. Un solo giro salvo richiesta di un altro.

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
