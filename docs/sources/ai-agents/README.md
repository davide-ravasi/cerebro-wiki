---
id: source-ai-agents-index
title: "AI agents — path snello (lavoro)"
type: source
domain: ai
tags: [source, ai, agents, cursor-sdk, udemy]
status: evergreen
updated: 2026-10-02
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

## Coda video / articoli (da smaltire)

Regola: **dopo ogni lab** → 15–20′ media. Mai Udemy + lab deep in parallelo.  
Corso: [AI Agents & Workflows — The Practical Guide](https://www.udemy.com/course/ai-agents-workflows-the-practical-guide/) (Max / Academind).

| Priorità | Dopo | Titoli Udemy (esatti) | ~min | Stato |
|:--------:|------|------------------------|-----:|:-----:|
| **A** | #1 | *What Are AI Agents?* · *General vs Task-specific Agents* · *Where Agents Run* · *AI Agents vs AI Workflows* | ~10 | ☑ **02/10** |
| **B** | #1–2 | *AI Agent Harnesses, LLMs & Limitations* · *How Agents Use Tools* · *Understanding Session Context* · *Core AI Agent Building Blocks - Overview* | ~13 | ☐ **prossimo** |
| **C** | #2 | *Analyzing The Agent Loop* · *How The Agent Learns About Tools & Behaves Correctly* (skip codice OpenAI; ascolta il concetto) | ~10 | ☐ dopo B |
| **C2** | #2 | Docs: [Cursor SDK](https://cursor.com/docs/sdk/typescript) — `create` / `send` / `wait` | ~10 | ☐ opz. |
| **D** | #3 | (si aggiorna a fine lab) | — | — |
| **E** | #4 | *Sometimes Important: Humans In The Loop* · *Managing Agent Tools & The Environment* · *How Agent Execution Is Constrained* | ~10 | ☐ più avanti |
| **F** | #7 | *Providing Your Own Instructions & Understanding AGENTS.md / CLAUDE.md* · *Understanding Agent Skills* | ~12 | ☐ con lab skills |
| — | skip | Welcome/Community · n8n deep · LEGACY Python/OpenAI/Ollama/Slack · Structured outputs · Multi-agent · CrewAI · eve · Memory deep finché non serve | — | skip |

**Prossima sessione media:** blocco **B** (~13′), poi **C** se resta tempo.

## Max (Udemy) vs docs ufficiali Cursor

| Fonte | A cosa serve |
|-------|----------------|
| **Video Max** | Vocabolario: cos’è un agent, tools, session context, agent vs workflow. Non sostituisce i lab. I titoli in coda sono mappati dal curriculum; il dettaglio frame-per-frame può variare. |
| **[Docs SDK TypeScript](https://cursor.com/docs/sdk/typescript)** | Come lo fai in `ai-lab`: `prompt` / `create`+`send`, `local` vs `cloud`, `wait`, errori, `resume`. Priorità per codice. |
| **[Cursor Learn — Agents](https://cursor.com/learn/agents)** (+ *Working with agents*) | Come usare l’agente **in IDE** (harness, prompt, context, delega). Complementa Max; non sostituisce lab SDK. |
| **[Claude Academy](https://academy.claude.com)** | Fluency / Claude Code / Platform. Utile in generale; **non** sul path SDK Cursor. Opz.: *AI Fluency* o pezzi Claude Code se usi Claude; Platform/MCP solo se serve al lavoro. |
| **Skill** `@sdk` / `~/.cursor/skills-cursor/sdk` | Trap comuni + pattern pronti in chat |

**Non aggiungere** Claude Academy / Cursor Learn come filo parallelo a Max + lab Ven — solo se A+B sono fatti e resta curiosità (15′).

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
| **2** | ☑ 02/10 | `Agent.create` + `send` + secondo messaggio (stesso agent) | Due turni, stesso contesto |
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
- [`raw/lab-2026-10-02-create-send.md`](./raw/lab-2026-10-02-create-send.md)

## Link

- Docs: [Cursor SDK TypeScript](https://cursor.com/docs/sdk/typescript)  
- Skill Cursor: `@sdk` / `~/.cursor/skills-cursor/sdk`  
- Mappa: [[map-agents-lab]]
