# Lab 2026-11-20 — #7 AGENTS.md (local + settingSources)

Repo lab: `../ai-lab` · comando: `npm run agents-md` · script: `agents-md.mjs` · file: `AGENTS.md`

## Setup

- `AGENTS.md` in root: prima riga risposta = `AGENTS-OK`; script npm solo da `package.json`
- `local: { cwd: ai-lab, settingSources: ["project"] }`
- Confronto: **senza** `settingSources` → niente `AGENTS-OK`, niente log rules progetto

## Esito

- Con `project`: `finished` · prima riga `AGENTS-OK` · lista script da `package.json` · log `LocalCursorRulesService ruleCount: 1` · id `agent-…` (local)
- Senza `settingSources`: può ancora leggere `package.json`, ma **non** segue la regola AGENTS.md

## Idee fissate

- `AGENTS.md` = sempre sul progetto (se caricato)
- Skill = solo se la richiesta le matcha (name/description)
- SDK local: senza `settingSources: ["project"]` default = solo inline → AGENTS.md non entra
- Cloud: `settingSources` non si applica allo stesso modo (carica sempre project/team/plugins)

## Reflect (Davide)

- **Imparato:** AGENTS.md caricato a sessione e seguito; skills solo se servono; in `create` va indicato `settingSources` per caricare le sources
- **Buco:** nessuno dichiarato
