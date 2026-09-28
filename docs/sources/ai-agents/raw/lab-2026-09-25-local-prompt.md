# Lab 2026-09-25 — Cursor SDK local `Agent.prompt`

Repo: `../ai-lab` (sibling di cerebro)  
Comando: `npm run prompt` (= `node --env-file=.env prompt.mjs`)

## Fatto

1. Setup: `@cursor/sdk`, `CURSOR_API_KEY` in `.env`, toy cwd `playground/`
2. One-shot `Agent.prompt` → `status: finished`, riassunto di `hello.txt`
3. Fallimento voluto: chiedere `manca.txt` → ancora `status: finished`, ma `result` spiega che il file non c’è

## Idee fissate

- **Local** = executor sul PC + `cwd`; modello via API Cursor. Non serve l’IDE aperto.
- Tu scegli la cartella con `local: { cwd }`.
- `finished` = il **run** è completo; non = il **task** è riuscito.
- `CursorAgentError` (throw) = non è partito; `status === "error"` = partito e rotto a runtime.

## Prossimo lab

Programma: [[source-ai-agents-index]] step **#2** — `Agent.create` + `send` (due turni).
