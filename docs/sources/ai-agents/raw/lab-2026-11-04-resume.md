# Lab 2026-11-04 — #6 resume (cloud)

Repo lab: `../ai-lab` · comando: `npm run resume` · script: `resume.mjs`  
*(Sett. 2–6/11: #6 mer al posto del SEO; SEO → ven.)*

## Setup

- `Agent.resume("bc-a6a6e799-…", { apiKey })` — stesso id del lab #5 (30/10)
- Nessun `cloud: { repos }` al resume: runtime dal prefisso `bc-`
- Prompt: sola lettura; chiede quali npm scripts aveva elencato nel turno precedente

## Esito

- `resumed agentId` = stesso `bc-…`
- Run `finished`; ha ricordato: prompt · conversation · errors:startup · errors:run · real-repo
- Contesto del #5 ancora vivo ~5 giorni dopo (script Node chiuso; sessione agente Cursor sì)

## Idee fissate

- `create` apre · `resume(id)` riprende · id lo salvi tu (file/DB), non Node
- Sessione Node ≠ sessione agente; `bc-` può restare riprendibile giorni, non garantito per sempre

## Reflect (Davide)

- **Imparato:** riprendo una sessione con l’`agentId` restituito da `Agent.create`.
- **Obiettivo:** botta e risposta con l’agente nello stesso contesto.
- **Buco:** nessuno.

## Prossimo

Path: **#7** skills / AGENTS.md. Ven 6/11: ripasso SEO (spostato da mer).
