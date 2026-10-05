# Lab 2026-10-02 — `Agent.create` + due `send`

Repo: `../ai-lab` · `npm run conversation`

## Fatto

1. `Agent.create` + `local: { cwd: playground }`
2. Turno 1: leggi `hello.txt` → `finished` + riassunto
3. Turno 2: conta parole della frase precedente **senza** rileggere → `28 words` → contesto tenuto

## Idee fissate

- `prompt` = one-shot; `create`+`send` = multi-turno sullo stesso agent
- Ogni `send` → `wait()` per lo status terminale
- Warning SQLite = rumore SDK, ignoralo

## Da ripulire (opz. prossimo lab)

- `await using agent = ...` per dispose automatico
- Messaggio errore env ancora cita `prompt.mjs`

## Video / articoli

- ☑ Blocco **A** (02/10 sera): *What Are AI Agents?* · *General vs Task-specific* · *Where Agents Run* · *AI Agents vs AI Workflows*
- ☐ Blocco **B** (prossimo): harness · tools · session context · building blocks
- ☐ Blocco **C**: agent loop (+ docs create/send opz.)

Vedi tabella completa in [[source-ai-agents-index]].

## Prossimo

Media: **B**. Lab Ven: **#3** errori.
