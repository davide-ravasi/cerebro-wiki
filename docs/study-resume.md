# Study resume — riprendi domani

> **Un solo file.** Leggilo quando riapri Cursor — non serve navigare il resto di cerebro.  
> Aggiorna **Questa settimana** + **Da ripassare** a fine sessione (2 minuti).

**Obiettivo:** full-stack teorico solido + traiettoria lead.  
**Core:** track-em-all · DDIA · Mongo (poco) · Max Node solo on-demand.

---

## Settimana tipo (pomodoro 25 min)

| Giorno | Tempo | Pomodoros | Slot |
|--------|-------|-----------|------|
| **Lun** | ~1 h | **2×25** | **track-em-all** — 1 pezzo codice |
| **Mar** | ~1 h | **2×25** | **DDIA** — nuovo (leggi / core-idea) |
| **Mer** | ~30 min | **1×25** | **Ripasso concetti** (ufficio) — chat only |
| **Gio** | ~30 min | **1×25** | **Ripasso concetti** (ufficio) — altro tema o stesso se debole |
| **Ven** | ~1 h | **2×25** | **Mongo** corto **oppure** track-em-all **oppure** lab agenti (1 pomodoro) — **mai due deep** |

**Pause:** 5 min tra pomodori; dopo 2 pomodori puoi fermarti (hai fatto l’ora).

**Max Node:** nessun slot fisso — 1 pomodoro solo se sblocca track-em-all.

**Ufficio (Mer / Gio):** solo ripasso / core-idea in chat — niente libro lungo, niente codice deep, niente lab. Tema della settimana ok (JSON-LD, Next, Fowler *idea*), ma formato leggero.

**Fowler vs Next:** lettura Fowler = **Mar** (o casa). Gio Next = checklist concetti. Se Fowler avanza, Mer/Gio possono **alternare** i temi (o entrambi in settimane diverse) — sempre in modalità ripasso, non dual deep lo stesso giorno.

### Come usare i 2 pomodori (Lun / Mar / Ven)

| Pomodoro | Cosa fare |
|----------|-----------|
| **1° (25)** | Obiettivo unico (codice / lettura / lezione) |
| **2° (25)** | Continua **lo stesso** obiettivo — oppure 15 min lavoro + 10 min aggiorna questo file |

### Mer / Gio (1 pomodoro — ufficio)

1. Tema dalla settimana o da **Da ripassare** (max 1). Oppure `Interleave: [tema A] + [tema B]` — **uno** scenario misto, non due lezioni
2. In chat: `@learn-core-idea-first` **oppure** `@learn-error-simulator` + hook
3. Chiudi con `Reflect:` — 3 righe: **imparato** / **obiettivo** / **buco** (il buco diventa una voce di ripasso se serve)
4. Spunta o sposta in **Fatto di recente** / **Ripasso chiuso**

---

## Come usarlo con le skill

| Vuoi… | Scrivi in chat |
|-------|----------------|
| Concetto confuso | `@learn-core-idea-first` + **Hook** |
| Pezzo pratico | `@learn-operational-fast` + obiettivo |
| Verificare | `@learn-error-simulator` + tema |
| Solo rileggere | path in **Dove** — niente skill |
| Spiega a parole tue | `Spiega-lead: [concetto]` (cadenza ~14 gg) |
| Chiudere la sessione | `Reflect:` — imparato / obiettivo / buco |
| Mischiare 2 temi | `Interleave: [A] + [B]` (1 scenario, Mer/Gio) |

**Nuovo → ripasso:** Mar (nuovo) → Mer/Gio (simulator su quel pezzo). Lun (app) → eventualmente Gio se hai messo un hook tecnico.

---

## Cadenza bi-settimanale — “Spiega come un lead” (Feynman inverso)

**Cosa:** ~10 min. Tu spieghi **un** concetto a parole tue (come a un mid). Io rispondo solo con: (1) cosa ok (2) imprecisioni (3) cosa manca. **Non** partire con una lezione.

**Come avviare in chat:**  
`Spiega-lead: [concetto]`  
es. `Spiega-lead: CORS 200 vs policy` oppure `Spiega-lead: linearizability`

| | |
|---|---|
| **Ultima volta** | — (non ancora fatto) |
| **Prossima scadenza** | **2026-09-16** (mer) — recupero: non fatto lun 14 |
| **Candidati** | **Lamport vs TOB** (forte) · 2PC vs consensus · linearizability · auth bollino FE vs JWT |

**Track:** quando apri la chat, dimmi la **data del giorno** → controllo se `Prossima scadenza` ≤ oggi. Dopo sessione: Ultima volta + Prossima = +14 giorni.

**Mappa tecniche:** `.cursor/skills/learning-modes/tecniche-apprendimento.md`

---

## Questa settimana — focus

*(sett. chiusa 2026-10-12 → 10-16: track SEO Episode Helmet · MR+reduce-side · lab #4 · video §1 + §2 parziale.)*

### Questa settimana (2026-10-19 → 10-23)

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | track: **canonical Episode** ✓ · **JSON-LD Show** (`TVSeries`) ✓ pushato · wiki SEO in cerebro | ☑ |
| Mar | DDIA cap. 10 — **map-side joins** ✓ (broadcast / partizione allineata; altrimenti reduce-side) | ☑ |
| Mer | **Ripasso agenti** ✓ 21/10 — workflow vs agent · harness esegue tool · AGENTS.md vs skills · memory | ☑ |
| Gio | Cap. 10 / DDIA chiusi ✓ · **Fowler 1ª lettura → sett. prossima** (oggi no libro) | ☑* |
| Ven | lab **#5** cloud **oppure** skill pratica (morning-agent / pezzo skill) | ☐ |

*Fowler `reading` ma sessione avvio rimandata. Prox track = JSON-LD Episode/Person · sitemap surplus.*

*(Sett. 19–23/10 chiusa: SEO Show+canonical · map-side · ripasso agenti · DDIA path · §2 video · Fowler rimandato.)*

### Questa settimana (2026-10-26 → 10-30)

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | track: JSON-LD **Episode** (`TVEpisode`) + **Person** ✓ · agente track aggiorna docs | ☑ |
| Mar | **Fowler** — 1ª sessione: ~10/44 pp metodo (teatro · test · Extract Function) ✓ 27/10 | ◐ |
| Mer | **Ripasso ufficio:** JSON-LD ✓ 28/10 — tipo macchina ≠ canonical/OG; Show `TVSeries` · Episode `TVEpisode` · Person `Person` | ☑ |
| Gio | **Ripasso ufficio:** Next.js Tier 1 concetti (bass-sheets checklist) — SC/CC · App Router · fetch server · env · `sources/nextjs/raw/bass-sheets-gio-checklist.md` | ☐ |
| Ven | lab **#5** se non fatto · altrimenti skill pratica | ☐ |

**28/10 cambio PC:** il filo di questa chat è `docs/restart-prompt-2026-10-28.md` (incollalo in una chat nuova). Le altre chat, se servono, vanno esportate in Markdown sulla macchina vecchia.

**Prenotato — mer 4/11:** ripasso ufficio SEO, tutti i layer e perché ci sono (title/description, canonical, OG/Twitter, JSON-LD + campi minimi Show/Episode/Person). Non tutto schema.org.

---

## Da ripassare (attivo — max 3)

### 1. *(libero — MapReduce core-idea ✓ 13/10)*

### 2. *(libero)*

### 3. *(libero)*

---

## Backlog ripasso

*Coda: promuovi in «Da ripassare» quando serve.*

### DDIA — MapReduce + join (cap. 10)

| | |
|---|---|
| **Hook** | *map/shuffle/reduce · reduce-side vs map-side · output batch = risultato materializzato per job successivi (non “query live”).* |
| **Skill** | MR+join ✓ · map-side ✓ · path **archiviato 21/10** |
| **Dove** | `raw/chapter-10.md` |
| **Bookmark** | **Chiuso** path studio. Q&A joins+output = ripasso opz. se serve. Cap. 11–12 surplus. |

### track-em-all — SEO a layer (canonical · OG · JSON-LD)

| | |
|---|---|
| **Hook** | *4 layer: title/desc · canonical=`og:url`=route id · OG/Twitter share · JSON-LD = tipo schema.org. Episode: `useParams` id ≠ `episode_number`. Show: `TVSeries` solo se `showData`.* |
| **Skill** | rilettura / Spiega-lead se serve |
| **Dove** | [[map-frontend-seo]] · `sources/react/raw/track-em-all-seo-helmet-jsonld.md` |
| **Bookmark** | Ruolo JSON-LD ✓ 28/10. **Mer 4/11:** ripasso tutti i layer SEO e perché (inclusi campi minimi, non tutto schema.org). Poi sitemap surplus. |

### track-em-all — Redux Toolkit + TypeScript (`createAsyncThunk` generics)

| | |
|---|---|
| **Hook** | *`createAsyncThunk<A,B,C>`: **A** = payload successo · **B** = arg (senza B → spesso `void`) · **C** = `rejectValue`. Dispatch manuale: `.fulfilled(payload, requestId, arg)`. Payload = form unica (service `response.data` ≡ reducer `action.payload`, no Axios). `variables` mutation ≠ payload. `throw` → `payload` undefined; `rejectWithValue` riempie `payload`.* |
| **Skill** | core-idea + error-sim ✓ 07/10 — ripasso solo se serve |
| **Dove** | `authSlice.tsx` · `Login.tsx` · `UseFavorite.tsx` |
| **Bookmark** | **Chiuso** 07/10 (gate 3/3 + 2 scenari). Archiviabile. |

### tracking-ds — Derivare invece di ricalcolare (+ lo zero falsy)

| | |
|---|---|
| **Hook** | *Se due valori devono corrispondere, ricava il secondo dal primo: due calcoli paralleli possono divergere, uno derivato no. Corollari: quando togli un campo da un contratto, controlla quali decisioni esistevano solo per servirlo. Lo zero è falsy → `!indexOf(x)` e `!distance` mentono entrambi. Un test difficile da scrivere accusa il codice, non il test.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `sources/nodejs/raw/derivare-invece-di-ricalcolare.md` · tracking-ds `src/metrics/calculateMetricsPath.ts` |
| **Bookmark** | `learn-core-idea-first` fatto 11/08 (3 domande passate, correzione su assoluto/relativo). Da rifare: le 4 trappole di piattaforma e lo zero falsy |

### track-em-all — Playwright: anti-flakiness (`isVisible` race)

| | |
|---|---|
| **Hook** | *Con UI async non branchare su `isVisible()` al primo render: aspetta outcome stabile (es. “Photos” **oppure** “No photos available”).* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `tests/person.smoke.spec.ts` · PersonPage |

### track-em-all — Context solo se serve davvero

| | |
|---|---|
| **Hook** | *Context solo se molti discendenti lontani. Home + props → Context è rumore.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | HomePage |

### track-em-all — useInfiniteQuery: `getNextPageParam` vs `maxPages`

| | |
|---|---|
| **Hook** | *`getNextPageParam` = c’è ancora da fetchare (`total_pages`). `maxPages` = quante pagine tieni in **cache** (ne droppa di vecchie). Cap solo su maxPages + fetch illimitato → lista che perde card. Preview home = `useQuery` + `cardAmount`; listing = infinite.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `useQueryShow.tsx` · `ShowResponse.total_pages` |

### tracking-ds — paginazione API + "misura prima di costruire"

| | |
|---|---|
| **Hook** | *`per_page`/`page` finché pagina < 100. Prima di costruire: misura il gap con prova reale.* |
| **Skill** | `@learn-operational-fast` |
| **Dove** | tracking-ds `src/discover.ts` |

### Node / browser — leggere file: input file vs `fs`

| | |
|---|---|
| **Hook** | *Browser: input → `File`/`FileReader`/`file.text()`. Node: `fs` / stream; upload → multipart. Non confondere client e server.* |
| **Skill** | `@learn-operational-fast` → `@learn-error-simulator` |
| **Dove** | da creare: `sources/nodejs/raw/reading-files-browser-vs-node.md` |
| **Bookmark** | Lacuna 31/07 — da **costruire** (non solo ripasso) |

### DDIA cap. 9 — ripasso generale (dopo chiusura capitolo)

| | |
|---|---|
| **Hook** | *Quando la rilettura a mano del cap. 9 è **chiusa** (pagine mancanti + Raft se serve): ripasso **generale** su tutto il capitolo, non pezzo per pezzo isolato. Filo: lin. (def/cost/CAP) → ordering/causality → Lamport → TOB → 2PC → consensus → membership. Scenario misti che intrecciano i concetti.* |
| **Skill** | `@learn-error-simulator` misto (+ opzionale `Spiega-lead` su un pezzo debole) |
| **Dove** | `raw/chapter-9.md` intero · filo narrativo in coda al raw |
| **Bookmark** | **1° fatto mer 23/09** ✓. **2° schedulato mer 30/09** — superfici diverse (non rifare gli stessi 3 scenari). Poi archiviare. |

### DDIA cap. 9 — ripasso di rinforzo (settembre, post-vacanza)

| | |
|---|---|
| **Hook** | *Blocco 1 (12/08) superato ma con aiuto extra su 3 punti: (1) linearizability non è "magia" — serve routing esplicito a leader/quorum, non basta che il dato "esista" da qualche parte; (2) rinunciare a linearizability è un trade-off di **performance**, non di fault-tolerance (spesso confuso); (3) i sequence number risolvono la causalità **facendo aspettare il client** l'operazione da cui dipende, non ordinando tutto globalmente.* |
| **Skill** | `@learn-error-simulator` (ripetere scenari 1, 2, 3 con superfici diverse) |
| **Dove** | `raw/chapter-9.md` § Linearizability + Cost of linearizability + Ordering and causality |
| **Bookmark** | Blocco 2 (2PC/consensus/membership/TOB + filo narrativo) rimandato a settembre se non fatto Gio 13/08 prima della vacanza. Rileggere anche pp. 24–52 (mai fatte) prima di questa sessione. |

### DDIA — Membership/ZK: 5 gap tecnici (post-vacanza ~29/08)

| | |
|---|---|
| **Hook** | *Ripasso 13/08 pre-vacanza: core-idea ok, ma mancano dettagli critici. **5 punti da rafforzare:** (1) 2PC vs Consensus = scopi diversi (atomic commit vs coordinamento); (2) Fault tolerance: majority quorum vs single coordinator; (3) Feature ZK/etcd (watches, ephemeral nodes, linearizability built-in); (4) Chicken-egg problem (chi coordina Postgres?); (5) ZK/etcd = self-coordinating con consensus interno.* |
| **Skill** | Rileggi note + `@learn-error-simulator` con domande oggi |
| **Dove** | `raw/chapter-9.md` — sezioni "Membership and coordination" (318-349) + "Atomic commit / 2PC" (256-298) |
| **Bookmark** | **Chiuso** 5 gap 24/09 + **2° generale 01/10** ✓. Archiviabile. |

### DDIA — rinforzo opzionale (già fatti in chat)

| Tema | Note |
|------|------|
| 2PC / Membership / TOB–lin–causal | Compresi + simulator dove applicabile. Solo se confusione dopo rilettura a mano. |
| Linearizability (rilettura) | Digitata in `raw/chapter-9.md` ✓ 2026-08-07 (def + when useful) |

---

## Ripasso chiuso di recente (archivio corto)

| Tema | Quando | Dove |
|------|--------|------|
| fetch: due passaggi, `ok`, HTTP/2 | 2026-08-04 | raw `fetch-two-steps-and-http-errors.md` |
| CORS / origine / 5 strati | 2026-08-06 | raw `origin-cors-and-the-five-layers.md` |
| favorites / auth FE vs JWT (PrivateRoute / persist / tea-token) | 2026-09-09 | simulator ✓ (dopo core-idea 03/09) |
| Pages 3 trap | 2026-09-16 | raw gitlab-pages-model |
| RQ enabled + staleTime/refetch | 2026-09-17 | HomePage · raw react-query-stale-time |
| DDIA cap. 9 ripasso generale (filo misto) | 2026-09-23 | simulator ✓ |
| DDIA Membership/ZK 5 gap | 2026-09-24 | simulator ✓ |
| DDIA cap. 9 2° ripasso generale | 2026-10-01 | simulator ✓ |
| RTK+TS createAsyncThunk A/B/C + fulfilled manuale | 2026-10-07 | core-idea + error-sim ✓ |
| Lamport + TOB (core-idea) | 2026-09-09 / 10 | raw cap. 9 |
| favorites / never trust client | 2026-07-22 | — |
| functional core / imperative shell | 2026-07-23 | — |

---

## In corso

| Tema | Stato | Prossimo |
|------|--------|----------|
| DDIA cap. 9 | Lettura ✓ · generale **2/2** ✓ 01/10 · **5 gap ZK ✓ 24/09** | **Archivio** (chiuso studio doppio passaggio) |
| DDIA cap. 9–10 | Path **chiuso** 21/10 (9 doppio passaggio · 10 MR/joins/output) | Archivio · Q&A joins opz. · 11–12 surplus |
| **Fowler Refactoring 2ª** | `reading` · **27/10** ~10/44 pp first example/metodo | Prox: finire pezzo metodo/esempio → smells → 1 refactor su track |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione (surplus Ven) |
| track-em-all | OG · Helmet · canonical · JSON-LD Show/Episode/Person ✓ 26/10 | Prox: sitemap surplus · *campo prove Fowler* |
| **Agenti / cloud** *(surplus Ven, lavoro)* | Lab **#1–#4** ✓ · §2 video ☑ · ripasso foto ✓ 21/10 | Prox: lab **#5** cloud / skill pratica |
| tracking-ds | P0 lavoro | Pages trap ✓ 16/09 |
| Libri coda | HF SA · Makarevich · DDIA 11–12 surplus · Pragmatic | Dopo avvio Fowler |
| **Bass / Next.js** *(Gio ripasso + surplus casa)* | [[map-bass-theory]] · checklist `sources/nextjs/raw/bass-sheets-gio-checklist.md` | Gio = concetti (ufficio). Codice/tutorial deep = casa/surplus. Alterna con Fowler idea su Mer/Gio se serve. Sheet: foto → SQL → Supabase (ok 02/10) |

---

## Fatto di recente

- **2026-10-28** — JSON-LD core-idea ✓ + Reflect. Imparato: ruolo del tipo accanto a canonical/OG. Obiettivo: perché tutti i dati SEO. Buco in backlog: catalogo attributi (campi minimi, non tutto schema.org).
- **2026-10-27** — Fowler 2ª: ~**10/44** pp metodo/first example (teatro). Preso: test prima · Extract Function + rename. Precisato: refactor = struttura **senza** cambiare comportamento. Prox: finire pezzo metodo
- **2026-10-26** — Track: JSON-LD Episode + Person ✓. Backup Drive avviato (env+private; 3 report lunghi ancora da caricare). Prox: chiudere Drive · fase B push repo
- **2026-10-22** — Gio: no lettura. Fowler 1ª → sett. 26–30.
- **2026-10-21** — Ripasso agenti ✓. **DDIA path chiuso**. Fowler libro arrivato (`reading`).
- **2026-10-20** — Cap. 10: map-side ✓ · output batch letto. Udemy **§2 chiusa**: #20 · #17–18 · memory. Foto ripasso Mer ok
- **2026-10-19** — Track'em All: **canonical Episode** (`useParams` id) · **JSON-LD Show** (`TVSeries`, guard `showData`) pushato. Cerebro: [[map-frontend-seo]] · concept canonical / JSON-LD / OG · pattern Helmet layers · raw `sources/react/raw/track-em-all-seo-helmet-jsonld.md`. Prox track: JSON-LD Episode/Person
- **2026-10-16** — Lab **#4** ✓ · Udemy §2 #12–16 ☑ · **Workshop morning-agent** (note in `docs/private/workshop-morning-agent-2026-10-16.md`, repo `Documents/morning-agent`). Prox: #20 HITL · Mer learning · lab #5/#7 con skill pattern
- **2026-10-14** — Cap. 10: **reduce-side joins** core ✓. Prox DDIA: map-side joins
- **2026-10-13** — MapReduce + join core-idea ✓. Lista pezzi restanti cap. 10 annotata
- **2026-10-12** — Track'em All: **dedupe meta** · Show Twitter + canonical · **Episode Helmet**. Prox track: Person Helmet
- **2026-10-09** — Lab agenti **#3** ✓. Video Max **A+B** ☑. Articolo **agent harness** ☑ — appunti in arrivo (con A+B). Prox lab: **#4**. Prox media: **C**. MapReduce Q1 ancora aperta
- **2026-10-08/09** — Lavoro pesante → formazione ridotta. **MapReduce** `@learn-core-idea-first` avviato (idea + analogia) · **Q1 non risposta**. Harness articolo: iniziato, non chiuso
- **2026-10-07** — **RTK+TS** ripasso ✓ (`createAsyncThunk` A/B/C · `.fulfilled` · throw vs `rejectWithValue`). Coda articoli: [agent harness](https://medium.com/gitconnected/agent-harness-a-no-bs-guide-8f69e3c0a3da) (~15′)
- **2026-10-06** — DDIA cap. 10: ~**pp. 11–15 / 40** ✓ (MapReduce / DFS). Note a fine capitolo. Prox: continuare MR
- **2026-10-05** — Track'em All: OG **1200×630** + title · opengraph.to **64→89**. **Helmet** su ShowPage (`react-helmet-async`, `3e614fb`) — commit locale, **non pushato**. UI design review in TODO. Collega: **Layers** ([layers.jamiemill.com](https://layers.jamiemill.com)) — lettura per lavoro, poi ne riparliamo. Prox: push · JSON-LD
- **2026-10-02** — Lab agenti **#2** ✓ · sera: video Max **blocco A ☑** · test spartito foto→INSERT (prompt dedicato, ok). Prox media: **B** · Prox Ven: lab **#3**
- **2026-10-01** — DDIA cap. 9 **2° ripasso generale** ✓ (2PC≠FTC · Lamport=parziale/causale ≠ TOB totale · ZK meta≠2PC · ephemeral+watch · chicken-egg → quorum self-consensus). Cap. 9 **chiuso** doppio passaggio
- **2026-09-30** — Track'em All: **OG meta facili** (canonical, site_name, locale, description ≤160) · deploy · opengraph.to **53→64**
- **2026-09-29** — DDIA cap. 10: ~**pp. 6–11 / 40** ✓. Unix/batch chiuso a mano; MapReduce iniziato (map→KV, shuffle, reduce). Note a fine capitolo. Prox: continuare MR/DFS
- **2026-09-25** — Lab agenti **#1+#1b** ✓ (`../ai-lab`: `Agent.prompt` local · `finished`≠task ok). Path snello scritto: `docs/sources/ai-agents/` + [[map-agents-lab]]. Prox Ven: **#2** `create`+`send`
- **2026-09-28** — Track'em All: **OG+Twitter home deployed** (`499a24b`). Audit opengraph.to ~53: error = image 192px. + Register #132 · UseFavorite cleanup
- **2026-09-22** — Track'em All: **`Login.tsx` mergiato #131** — `.fulfilled(payload, requestId, arg)` · `response.data` · generics login/register. **Ripasso RTK+TS** → backlog. Cap. 10 inizio ✓
- **2026-09-21** — Track'em All: **favorites add/remove `useMutation` mergiato #130** — pair chiuso (`response.data` · `.fulfilled(payload, requestId, arg)` · loading `variables`). Prox track: Open Graph · later `Login.tsx`
- **2026-09-17** — RQ **enabled + staleTime** simulator ✓ (`textInput`≠fetch · fresh per-key · ritorno a termine già cercato = cache hit; staleTime = **5 min** non 30)
- **2026-09-16** — Pages **3 trap** simulator ✓ (snapshot sostituisce · artifact stage prec. arrivano da soli · `mkdir -p` non azzera). Spiega-lead ancora aperto
- **2026-09-15** — Track'em All: **favorites add `useMutation` ✓** · DDIA cap. 9 lettura/core chiusa (cap. 10 dal 22/09; chiusura 9 = 23–24/09)
- **2026-09-14** — Track'em All: **`useMutation` register ✓** (stesso pattern login: `throw` + `register.fulfilled` + `isPending`). Favorites avviato; chiuso add il 15/09. Spiega-lead → mer 16
- **2026-09-11** — Track'em All: **`useMutation` login ✓** — `mutationFn: loginUser` + `onSuccess` → `dispatch(login.fulfilled(response))`; token ok; Redux/PrivateRoute/favorites ripristinati. Prox: register / favorites mutation · opz. azione sync `setSession` al posto di fulfilled manuale
- **2026-09-10** — DDIA **TOB** `@learn-core-idea-first` ✓ (cancelliere: stessi elementi + stesso ordine; arbitraggio concorrenza; username = primo in lista vince). Coppia Lamport+TOB chiusa in core-idea
- **2026-09-09** — Auth error-simulator ✓ · **Lamport** `@learn-core-idea-first` ✓ (uffici/timbri; max+1; concorrenti incomparabili; username → serve accordo/TOB). **Prox:** TOB core-idea + esempi
- **2026-09-08** — DDIA cap. 9 a mano: rilettura cost/ordering/causality/seq + **Lamport timestamps** + **TOB** (props, log, async) + intro distributed tx/consensus (~p.32, single-node WAL → “not enough to send commit to all”) → `raw/chapter-9.md`
- **2026-09-07** — Track'em All: **Listing** #126 · **Episode smoke mergiato** #127 (nav show→Pilot; shell; S01E01; air date; overview; cast/photos `.first()`)
- **2026-09-04** — Track'em All: **Favorites smoke 3/3 ✓** mergiato (#124). Auth error-simulator ancora aperto (Mer/Gio)
- **2026-09-03** — Track'em All: ripasso auth **core-idea ✓** (`@learn-core-idea-first` — bollino FE vs biglietto server; persist/`PrivateRoute`/`tea-token`). Resta `@learn-error-simulator`
- **2026-08-12** — DDIA ripasso generale cap. 9 blocco 1 ✓ (`@learn-error-simulator` misto, 5 scenari: recency/routing, cost-CAP, causality vs lin. + seq. number fix, TOB vs causal order, serializability vs lin.) — tutti superati
- **2026-08-11** — DDIA cap. 9 a mano (pp. 18–23): cost of linearizability, ordering guarantees, ordering & causality (total vs partial order), sequence number ordering (+ non-causal generators) → `raw/chapter-9.md`
- **2026-08-10** — Favorites smoke parziale: guest→`/login` + logged empty (`persist:root`, await `addInitScript`, empty copy fix). **Prox:** seed `favorites[]` + assert cards · poi PR. Ripasso auth → attivo #1 Mer
- **2026-08-07** — Track'em All: Load more — `getNextPageParam` su `total_pages` TMDB, rimosso `maxPages` (cache drop); tipo `ShowResponse`; branch/PR `feat/showlist-infinite-total-pages`
- **2026-08-07** — Track'em All: About polish ✓ (copy 4 sezioni + layout + Footer repo) · PR `feat/updated-about-page`
- **2026-08-07** — DDIA Linearizability note a mano digitizzate (flip nel tempo; useful: locks/leader, uniqueness, cross-channel) → `raw/chapter-9.md`
- **2026-08-07** — DDIA Linearizability: lettura/note a mano finite (foto)
- **2026-08-06 (pomeriggio)** — Track'em All: About rename + PWA manifest (`vite.config.js`)
- **2026-08-11** — tracking-ds: wiring del file derivato di metriche · lezione `learn-core-idea-first` su *derivare invece di ricalcolare* (3 domande passate) → raw `derivare-invece-di-ricalcolare.md` + voce nel backlog ripasso
- **2026-08-06** — Sync resume: archiviati fetch + CORS dal backlog; attivo ripasso = Pages trap · RQ enabled/staleTime · Playwright isVisible; cadenza Spiega-lead → 20/08
- **2026-08-06** — Ripasso CORS ✓ (200≠policy · curl≠browser · Origin diversa)
- **2026-08-05** — DDIA cap. 9 a mano: intro + inizio Linearizability → raw
- **2026-08-04** — Person smoke stabilizzata (`isVisible` race) · lezione CORS 6 tappe · fetch simulator ✓
- **2026-08-03** — Show smoke chiuso · Pages simulator (modello ✓, trap meccanici da rifare)
- **2026-07-31** — fetch raw + 3 concept · backlog file input vs `fs`
- **Lug 2026** — 2PC, Membership, TOB, track-em-all security, Mongo find ops

---

## Template voce ripasso

```markdown
### N. [Tema]

| | |
|---|---|
| **Hook** | *una frase tua* |
| **Skill** | `@learn-...` |
| **Dove** | path o libro |
```

---

## Regole

1. **Attivo ≈ max 3**; in più → **Backlog ripasso**.
2. **1 obiettivo per sessione** (anche con 2 pomodori).
3. Mer/Gio = solo ripasso da «Da ripassare» o dal piano settimana (non dal backlog intero). `Interleave` = 2 temi, 1 scenario.
4. Fine sessione: `Reflect:` (3 righe) + spunta tabella settimana + aggiorna ripasso (2 min).
5. **Non-core:** Mongo, basso, **lab agenti** = solo surplus (Ven). Agenti: **1 obiettivo**, 1 pomodoro; non + track deep lo stesso giorno. Path: `docs/sources/ai-agents/README.md` (Udemy = solo vocabolario selezionato).
6. **Ogni ~14 giorni:** «Spiega come un lead» — dimmi la data del giorno.

*Ultimo aggiornamento: 2026-10-28 — prompt ripartenza `docs/restart-prompt-2026-10-28.md` · Gio Next · mer 4/11 SEO*
