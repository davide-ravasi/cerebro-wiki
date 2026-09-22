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
| **Mer** | ~30 min | **1×25** | **Ripasso** con Cursor (`@learn-error-simulator`) |
| **Gio** | ~30 min | **1×25** | **Ripasso** con Cursor (altro tema o stesso se debole) |
| **Ven** | ~1 h | **2×25** | **Mongo** corto **oppure** track-em-all **oppure** lab agenti (1 pomodoro) — **mai due deep** |

**Pause:** 5 min tra pomodori; dopo 2 pomodori puoi fermarti (hai fatto l’ora).

**Max Node:** nessun slot fisso — 1 pomodoro solo se sblocca track-em-all.

### Come usare i 2 pomodori (Lun / Mar / Ven)

| Pomodoro | Cosa fare |
|----------|-----------|
| **1° (25)** | Obiettivo unico (codice / lettura / lezione) |
| **2° (25)** | Continua **lo stesso** obiettivo — oppure 15 min lavoro + 10 min aggiorna questo file |

### Mer / Gio (1 pomodoro)

1. Apri questa sezione **Da ripassare**
2. In chat: `@learn-error-simulator` + hook della voce
3. Fine: spunta o sposta in **Fatto di recente** / **Ripasso chiuso**

---

## Come usarlo con le skill

| Vuoi… | Scrivi in chat |
|-------|----------------|
| Concetto confuso | `@learn-core-idea-first` + **Hook** |
| Pezzo pratico | `@learn-operational-fast` + obiettivo |
| Verificare | `@learn-error-simulator` + tema |
| Solo rileggere | path in **Dove** — niente skill |
| Spiega a parole tue | `Spiega-lead: [concetto]` (cadenza ~14 gg) |

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

*(settimana 2026-09-21 → 09-25)*

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | track-em-all: **favorites add/remove `useMutation` ✓** mergiato **#130** | ☑ |
| Mar | **DDIA cap. 10** inizio ✓ · surplus track: **`Login.tsx` mergiato #131** | ☑ |
| Mer | **Chiusura cap. 9:** ripasso generale `@learn-error-simulator` | ☐ |
| Gio | **Chiusura cap. 9:** Membership/ZK **5 gap** | ☐ |
| Ven | **Lab agenti** 1×25 (Cursor SDK local — utile lavoro) **oppure** track leggero / Mongo — **mai entrambi deep**; se Mar–Gio pesanti → skip lab | ☐ |

*(Sett. 14–18: register ✓ · 2PC+FTC ✓ · Pages 3 trap ✓ · RQ enabled/stale ✓ · favorites add ✓. Sett. 21: remove + merge #130.)*

---

## Da ripassare (attivo — max 3)

*Mer/Gio sett. 14–18: Pages ✓ · RQ ✓. Slot liberi fino a chiusura cap. 9 (23–24/09).*

### 1–3. *(liberi — chiusura DDIA 9 mer/gio prossima sett.)*

---

## Backlog ripasso

*Coda: promuovi in «Da ripassare» quando serve.*

### track-em-all — Redux Toolkit + TypeScript (`createAsyncThunk` generics)

| | |
|---|---|
| **Hook** | *`createAsyncThunk<A,B,C>`: **A** = payload successo · **B** = arg (senza B → spesso `void`) · **C** = `rejectValue`. Dispatch manuale: `.fulfilled(payload, requestId, arg)`. Payload = form unica (service `response.data` ≡ reducer `action.payload`, no Axios). `variables` mutation ≠ payload.* |
| **Skill** | `@learn-core-idea-first` **poi** `@learn-error-simulator` (payload vs arg vs rejectValue) |
| **Dove** | `authSlice.tsx` · `Login.tsx` · `UseFavorite.tsx` · chat 22/09 (login TS) |
| **Bookmark** | Pratica fatta su login/favorites; **da rispiegare** quando c’è slot Mer/Gio libero (dopo chiusura cap. 9) |

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
| **Bookmark** | **Schedulato mer 23/09.** Filo intero. Incorpora 3 punti deboli lin. Non rifare blocco 1 (12/08). |

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
| **Bookmark** | **Schedulato gio 24/09.** 5 punti; 1–2 già toccati da FTC 15/09, restano watch/ephemeral/chicken-egg. |

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
| Lamport + TOB (core-idea) | 2026-09-09 / 10 | raw cap. 9 |
| favorites / never trust client | 2026-07-22 | — |
| functional core / imperative shell | 2026-07-23 | — |

---

## In corso

| Tema | Stato | Prossimo |
|------|--------|----------|
| DDIA cap. 9 | Lettura/core **chiusa 15/09** | **Chiusura studio:** mer 23/09 generale · gio 24/09 5 gap. Poi archivio. Wiki = coda |
| DDIA cap. 10 | Raw aperto · **~pp. 1–6 / ~40** (22/09) | Prox Mar: continuare a mano → digitare intro/Unix in `raw/chapter-10.md` |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione (surplus Ven) |
| track-em-all | Smoke ✓ · mutations #128–#130 · **`Login.tsx` #131 ✓** | Prox: Register.tsx · Open Graph · ripasso RTK+TS (backlog) |
| **Agenti / cloud** *(surplus Ven, lavoro)* | Udemy posseduto = vocabolario | **Ven 26/09:** lab Cursor SDK **local** 1 pomodoro (task concreto). Poi cloud. Max 1 deep/settimana. Mar–Gio pesanti → skip |
| tracking-ds | P0 lavoro | Pages trap ✓ 16/09 |
| Libri coda | Fowler, Makarevich, Head First SA… | dopo blocco DDIA |
| **Bass theory** *(idea, non attivo)* | Piano discusso 12/08 → [[map-bass-theory]] | Riprendere a settembre (post-vacanza); **non-core/surplus**, non compete con la settimana tipo |

---

## Fatto di recente

- **2026-09-22** — DDIA cap. 10: inizio lettura ✓ (~**6/40** pp.). Raw `chapter-10.md` scheletro. Digitazione note quando le mandi.
- **2026-09-21** — Decisione: **Ven surplus = lab agenti** (Cursor SDK → cloud; utile lavoro). Regola anti-overload: 1×25, XOR track/Mongo; skip se settimana DDIA pesante. Udemy = vocabolario, non filo principale.
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
3. Mer/Gio = solo ripasso da «Da ripassare» (non dal backlog intero).
4. Fine sessione: spunta tabella settimana + aggiorna ripasso (2 min).
5. **Non-core:** Mongo, basso, **lab agenti** = solo surplus (Ven). Agenti: **1 obiettivo**, 1 pomodoro; non + track deep lo stesso giorno.
6. **Ogni ~14 giorni:** «Spiega come un lead» — dimmi la data del giorno.

*Ultimo aggiornamento: 2026-09-22 — Login.tsx **#131** su main · ripasso RTK+TS in backlog · Mer = chiusura cap. 9*
