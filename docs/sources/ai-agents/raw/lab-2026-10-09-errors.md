# Lab 2026-10-09 — due fallimenti

Repo: `../ai-lab`

## Fatto

1. `npm run errors:startup` — chiave finta → `CursorAgentError`, `Invalid User API Key`, `isRetryable: false`. Il run non esiste.
2. `npm run errors:run` — chiave vera, legge `hello.txt` → `status: finished`, `error: (none)`, `result` con il riassunto.

`status === "error"` non forzato: è un guasto del run (modello/tool/runtime), non un prompt. Il file mancante del lab #1b resta `finished`.

Rumore Windows: warning SQLite; su startup anche `Assertion failed` in `async.c` dopo l’errore vero (chiusura handle). Non fa parte del modello.

## Idee fissate

- Throw `CursorAgentError` = non partito → ambiente / chiave. `isRetryable: false` = non riprovare.
- Oggetto da `wait()` = partito. `status === "error"` → `result.error`, non la chiave.
- `finished` + `error` vuoto = run chiuso. Il testo in `result` dice se il task è riuscito.

## Prossimo lab

Step **#4** — stesso pattern su repo reale, cwd stretto, task read-only.
