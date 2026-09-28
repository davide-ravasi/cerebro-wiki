---
id: source-ai-agents-index
title: "AI agents — path snello (lavoro)"
type: source
domain: ai
tags: [source, ai, agents, cursor-sdk, udemy]
status: evergreen
updated: 2026-09-25
---

# AI agents — path snello (lavoro)

Repo lab: `../ai-lab` (sibling di cerebro).  
Corso Udemy: [AI Agents & Workflows — The Practical Guide](https://www.udemy.com/course/ai-agents-workflows-the-practical-guide/) = **vocabolario**, non filo principale.

Cadenza: **1×25 min / Ven** (XOR track/Mongo). Mar–Gio pesanti → skip.  
Mappa: [[map-agents-lab]]

## Mappa Udemy → tuo path

| Modulo corso (indicativo) | Tu |
|---------------------------|-----|
| Getting started · What are agents & workflows | **Guarda** (~10–15 min totali) |
| Workflows vs agentic · tools · HITL · memory light | **Guarda** i pezzi concetto; non rifare tutto in OpenAI/Python |
| OpenAI SDK / structured outputs / Slack / Ollama | **Salta** (stessi concetti → Cursor SDK) |
| Multi-agent + CrewAI section | **Salta** finché lab #5–6 non sono stabili |
| n8n / no-code deep | **Salta** come percorso; ok 1 video se curiosità |

## Quando guardare i video (dopo il lab, non prima)

Regola: max **15–20 min** video lo stesso giorno del lab, **dopo** lo script. Mai Udemy + lab deep in parallelo.

| Quando | Video (~min) |
|--------|----------------|
| Dopo lab **#1** (ora / weekend) | Intro + *What are agents & workflows?* + *Workflows vs agentic* |
| Dopo lab **#2** | Tools + loop agente (skip codice OpenAI) |
| Dopo lab **#4** | HITL + sandbox/permessi (light) |
| Dopo lab **#5–6** | Memory light se serve; poi valuta multi-agent |
| Mai in blocco | OpenAI SDK, Slack, Ollama, CrewAI |

## Concetti da fissare (Udemy o Spiega-lead)

| Idea | Perché ti serve | Dove |
|------|-----------------|------|
| Agent vs workflow | Quando serve un loop con tools | Lab + Spiega-lead |
| Tools + side effect + errori | Cosa può toccare disco / API | Lab SDK |
| Context / memory (light) | Limiti contesto, non magia | Docs + lab |
| Sandbox + permessi + HITL | `cwd`, review umana | Lab + lavoro |
| Skills / progressive disclosure | AGENTS.md / project memory | Cursor + [[concept-claude-md]] |  

## Programma lab (pomodori Ven) — ordine fisso

| # | Stato | Obiettivo (1 solo) | Done quando |
|---|:-----:|--------------------|-------------|
| **1** | ☑ 25/09 | `Agent.prompt` **local** + `cwd` toy + API key | Run `finished` + sai che local ≠ IDE open |
| **1b** | ☑ 25/09 | Fallimento voluto (file assente) | Capisci: `finished` ≠ task riuscito |
| **2** | ☐ | `Agent.create` + `send` + secondo messaggio (stesso agent) | Due turni, stesso contesto |
| **3** | ☐ | Modello errori: `CursorAgentError` vs `status === "error"` | Sai quale fix per quale |
| **4** | ☐ | Stesso pattern su **repo reale** (cwd stretto, task read-only) | Esito utile su codice che conosci |
| **5** | ☐ | **Cloud**: `cloud: { repos }` su un repo tuo, task piccolo | Capisci VM vs disco locale |
| **6** | ☐ | **Resume** / run id / osservabilità minima | Riapri un agent dopo |
| **7** | ☐ *opz* | Skills / AGENTS.md sul progetto lavoro | L’agente rispetta regole progetto |
| **8** | ☐ *opz* | Tool custom o MCP **solo se** un caso lavoro lo chiede | — |
| **9** | ☐ *opz* | Claude Managed Agents / multi-agent | Solo dopo 5–6 stabili |

## Orizzonte ~3 mesi (successo)

1. Disegni agent vs workflow per un caso lavoro  
2. Un agente **cloud** (o local CI) fa un job ripetibile utile  
3. Spieghi tools / sandbox / errori senza slide  

## Note lab

- [`raw/lab-2026-09-25-local-prompt.md`](./raw/lab-2026-09-25-local-prompt.md)

## Link

- Docs: [Cursor SDK TypeScript](https://cursor.com/docs/sdk/typescript)  
- Skill Cursor: `@sdk` / `~/.cursor/skills-cursor/sdk`  
- Mappa: [[map-agents-lab]]
