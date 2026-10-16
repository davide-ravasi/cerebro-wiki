# Lab 2026-10-16 — #4 repo reale (read-only)

Repo lab: `../ai-lab` · comando: `npm run real-repo`

## Setup

- `cwd` = `react-exp/track-em-all` (root del progetto)
- Prompt: sola lettura su `src/pages/EpisodePage/EpisodePage.tsx` — come si costruisce `metaDescription` (overview assente vs lunga / SxxExx)

## Trap

- `cwd` troppo stretto (`…/EpisodePage` solo) → `status: error`, `error.message: [internal] unable to open database file` (SQLite stato SDK). Non era path file mancante.
- Fix: cwd = root repo; stringere il perimetro nel **prompt**, non nella cartella foglia.

## Esito

- Run `finished` (dopo retry con cwd root).
- L’agente ha interpretato correttamente la logica overview / fallback meta (confermato dall’utente 16/10).

## Idee fissate

- Lab su codice reale = stesso `create`+`send`+`wait`, ma `local.cwd` su un repo che conosci.
- Read-only nel prompt; “stretto” = cosa chiedi di aprire, più che una sottocartella minima senza diritto di scrittura per lo stato agente.

## Prossimo

Lab **#5** cloud, oppure video **C** / learning screenshot Mer 21/10.
