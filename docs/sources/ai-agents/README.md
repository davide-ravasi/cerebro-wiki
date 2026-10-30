---
id: source-ai-agents-index
title: "AI agents — path snello (lavoro)"
type: source
domain: ai
tags: [source, ai, agents, cursor-sdk, udemy]
status: evergreen
updated: 2026-10-30
---

# AI agents — path snello (lavoro)

Repo lab: `../ai-lab` (sibling di cerebro).  
Corso Udemy: [AI Agents & Workflows — The Practical Guide](https://www.udemy.com/course/ai-agents-workflows-the-practical-guide/) = **vocabolario**, non filo principale.

Cadenza: **1×25 min / Ven** (XOR track/Mongo). Mar–Gio pesanti → skip.  
Mappa: [[map-agents-lab]]

## Mappa Udemy → tuo path (sezioni curriculum)

Corso: [AI Agents & Workflows — The Practical Guide](https://www.udemy.com/course/ai-agents-workflows-the-practical-guide/) (Max / Academind).  
Regola: **1 obiettivo Ven** = lab SDK. Udemy = vocabolario, max 15–20′ dopo lab o in surplus corto. Non rifare in OpenAI/Python/n8n.

| § | Titolo corso | ~ | Quando | Stato |
|---|--------------|--:|--------|:-----:|
| **1** | Getting Started | 25′ | Prima / con lab **#1–2** | ☑ |
| **2** | Using & Steering AI Agents | 27′ | Con lab **#3–4** (+ HITL/skills) | ☑ |
| **3** | Building Agents & Workflows — Overview (#21–25) | 10′ | **Dopo lab #5** (oggi ok) — overview; pezzo utile = *WHO / WHERE* (#23) | ☐ |
| **4** | Building AI Workflows & Applications | ~2h30 | **Skip path** (implementazione corso). Ok 1 video se curiosità, mai + lab deep | skip |
| **5** | Building AI Agents | ~2h | **Skip path** stesso motivo; concetti → Cursor SDK lab | skip |
| **6** | Using CrewAI | ~53′ | Solo **dopo lab #8b** (capstone). Prima = no | skip |

**§3 lezioni:** 21 Module intro · 22 Options for building · **23 WHO builds / WHERE runs** · 24 Options with code · 25 Choosing building blocks.

## Coda video / articoli (da smaltire)

Regola: **dopo ogni lab** → 15–20′ media. Mai Udemy + lab deep in parallelo.  
Corso: [AI Agents & Workflows — The Practical Guide](https://www.udemy.com/course/ai-agents-workflows-the-practical-guide/) (Max / Academind).

| Priorità | Dopo | Titoli Udemy (esatti) | ~min | Stato |
|:--------:|------|------------------------|-----:|:-----:|
| **A** | #1 | *What Are AI Agents?* · *General vs Task-specific Agents* · *Where Agents Run* · *AI Agents vs AI Workflows* | ~10 | ☑ **02/10** |
| **B** | #1–2 | *AI Agent Harnesses, LLMs & Limitations* · *How Agents Use Tools* · *Understanding Session Context* · *Core AI Agent Building Blocks - Overview* | ~13 | ☑ **09/10** |
| **C** | — | *(non presente in questo curriculum Udemy — loop/tools già in §1 + lab)* | — | ☑ skip |
| **C2** | #5 | Docs: [Cursor SDK](https://cursor.com/docs/sdk/typescript) — `local` vs `cloud` · `resume` / run id (prepara #6) | ~10 | ☐ opz. **dopo #5** |
| **E** | #4 | §2: *Three Main Ways…* (#12) · tools/model (#13–15) · *Humans In The Loop* (#20) | ~10 | ☑ **20/10** |
| **F** | #7 | *System Instructions* (#16) · *AGENTS.md / CLAUDE.md* · *Agent Skills* (#17–18) · Memory (notes) | ~12–15 | ☑ **20/10** (appunti foto) |
| — | skip | Welcome/Community · n8n deep · LEGACY Python/OpenAI/Ollama/Slack · Structured outputs · Multi-agent · CrewAI · eve · Memory deep finché non serve | — | skip |

**Coda video Max §1–§2:** **chiusa** 20/10.  
**§3:** prossima (opz., ~10′) dopo #5 — dimmi quando la inizi / chiudi.  
**Mer 21/10:** ripasso chat foto (harness/loop/workflow + AGENTS.md vs skills + memory).

## Coda articoli (lavoro / surplus — non in parallelo al lab Ven)

Un articolo per sessione (~15–20′), dopo retrieval (RTK, DDIA) o come lettura lavoro.

| Priorità | Articolo | Tema | ~min | Stato |
|:--------:|----------|------|-----:|:-----:|
| **1** | [Agent harness — no-BS guide](https://medium.com/gitconnected/agent-harness-a-no-bs-guide-8f69e3c0a3da) | Harness = instructions + tools + model (collega lab SDK + Max B) | ~15 | ☑ **09/10** (appunti → learning) |
| **2** | [Claude come tech lead, non senior engineer](https://medium.com/codetodeploy/how-to-run-claude-like-a-tech-lead-not-a-senior-engineer-fba295ade37d) | Delega, review, deliverable — carriera | ~15 | ☐ |
| **3** | [AI + design system guardrails (Evil Martians)](https://evilmartians.com/chronicles/ai-makes-design-system-guardrails-mandatory-this-framework-delivers-them) | Token, regole, AI su UI — track / lavoro | ~15 | ☐ |
| — | [Layers](https://layers.jamiemill.com) | 7 strati product design (skill opz.) | ~20 | ☐ (Mer 07/10 = RTK; Layers in coda) |

**Ordine suggerito:** harness (1) ☑ → tech lead (2) → design guardrails (3) · Layers quando previsto in calendario.  
**Dopo lab #5 (~15′):** docs SDK **C2** (cloud + `resume`) **oppure** articolo **#2** tech lead. Non aprire moduli Udemy nuovi.

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
| **3** | ☑ 09/10 | Modello errori: `CursorAgentError` vs `status === "error"` | Sai quale fix per quale |
| **4** | ☑ 16/10 | Stesso pattern su **repo reale** (cwd root track-em-all, prompt su EpisodePage, read-only) | Esito utile su codice che conosci |
| **5** | ☑ 30/10 | **Cloud**: `cloud: { repos }` su un repo tuo, task piccolo | Capisci VM vs disco locale |
| **6** | ☐ | **Resume** / run id / osservabilità minima | Riapri un agent dopo |
| **7** | ☐ *opz* | Skills / AGENTS.md sul progetto lavoro | L’agente rispetta regole progetto |
| **7b** | ☐ | **HITL / permessi** — cosa può toccare; quando si ferma e chiede te | Sai dove sta il cancello umano vs il loop |
| **8** | ☐ *opz* | Tool custom o MCP **solo se** un caso lavoro lo chiede | — |
| **8b** | ☐ | **Capstone** — *un* agent a più passi (piano → tool → verifica → stop) | Complesso = composto, non più API |
| **9** | ☐ *opz* | Claude Managed Agents / multi-agent | Solo dopo **8b** (e 5–6) stabili |

Regola: **1–6** obbligatori in ordine. **7 → 7b → 8 (se serve) → 8b → 9**. Non un secondo corso in parallelo. CrewAI / Udemy multi-agent: no finché 8b non è solido.

## Orizzonte ~3 mesi (successo)

1. Disegni agent vs workflow per un caso lavoro  
2. Un agente **cloud** (o local CI) fa un job ripetibile utile  
3. Spieghi tools / sandbox / errori senza slide  

## Note lab

- [`raw/lab-2026-09-25-local-prompt.md`](./raw/lab-2026-09-25-local-prompt.md)
- [`raw/lab-2026-10-02-create-send.md`](./raw/lab-2026-10-02-create-send.md)
- [`raw/lab-2026-10-09-errors.md`](./raw/lab-2026-10-09-errors.md)
- [`raw/lab-2026-10-16-real-repo.md`](./raw/lab-2026-10-16-real-repo.md)
- [`raw/lab-2026-10-30-cloud.md`](./raw/lab-2026-10-30-cloud.md)

## Idea later (non path)

Chat UI su tracking + agente: surplus **dopo** lab #3–#4. Opzioni UI: assistant-ui / AI SDK `useChat`; runtime: backend + Cursor SDK (separato dalla chat).

## Link

- Docs: [Cursor SDK TypeScript](https://cursor.com/docs/sdk/typescript)  
- Skill Cursor: `@sdk` / `~/.cursor/skills-cursor/sdk`  
- Mappa: [[map-agents-lab]]
