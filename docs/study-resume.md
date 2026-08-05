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
| **Ven** | ~1 h | **2×25** | **Mongo** corto **oppure** track-em-all |

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
3. Fine: spunta o sposta in **Fatto di recente**

---

## Come usarlo con le skill

| Vuoi… | Scrivi in chat |
|-------|----------------|
| Concetto confuso | `@learn-core-idea-first` + **Hook** |
| Pezzo pratico | `@learn-operational-fast` + obiettivo |
| Verificare | `@learn-error-simulator` + tema |
| Solo rileggere | path in **Dove** — niente skill |

**Nuovo → ripasso:** Mar (nuovo) → Mer/Gio (simulator su quel pezzo). Lun (app) → eventualmente Gio se hai messo un hook tecnico.

---

## Questa settimana — focus

*(settimana 2026-08-03 → 08-07)*

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | track-em-all: **Show smoke** (`show.smoke.spec.ts`) — seasons/episodes ✓ | ☑ |
| Mar | ripasso: **fetch** (`response.ok` / due passaggi / HTTP/2 status) ✓ | ☑ |
| Mer | DDIA: rilettura cap. 9 note a mano (1 sezione) — **saltato** → recupero Gio/Ven | ☐ saltato |
| Gio | **DDIA** 1 sezione note a mano (priorità) · ripasso CORS/Pages solo se surplus | ☐ |
| Ven | track-em-all: **About polish** (Person smoke già ✓) · se c’è 2° pomodoro: DDIA altra sezione | ☐ |

---

## Da ripassare (attivo — max 3)

*Mer/Gio: **1 voce per pomodoro**. In più → **Backlog ripasso**.*

### 1. DDIA — Atomic commit / 2PC (iniziato 2026-07-24)

| | |
|---|---|
| **Hook** | *2PC = prepare/commit + promesse. In practice: exactly-once ≈ atomicità messaggio+effetto; XA = 2PC standard; dopo prepare i lock restano “in doubt”; recovery dal log del coordinatore; limiti = costo/disponibilità. ≠ 2PL.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | libro DDIA cap. 9 · `raw/chapter-9.md` |
| **Bookmark** | 2PC practice + ripasso Mer ✓. Opzionale: micro-simulator se rivede. |

### 2. DDIA — Membership & coordination (ZK / etcd)

| | |
|---|---|
| **Hook** | *Servizi di coordinamento: leader election, config, lock distribuiti — consensus “in scatola” per le app. Non reinventare 2PC in casa.* |
| **Skill** | `@learn-error-simulator` (se vuoi verificare oggi) |
| **Dove** | libro DDIA cap. 9 § Membership · `raw/chapter-9.md` |
| **Bookmark** | Core-idea ✓ 2026-07-30 · raw § Membership + consensus idea |

### 3. (Opzionale) TOB / lin / causal — solo se confuso

| | |
|---|---|
| **Hook** | *TOB = stessa sequenza; lin = recency; causal = causa→effetto* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `raw/chapter-9.md` |

---

## Backlog ripasso

*Coda: non entra nel Mer/Gio finché non la promuovi in «Da ripassare».*

### track-em-all — useQuery search: draft vs committed + `enabled`

| | |
|---|---|
| **Hook** | *`textInput` = bozza (UI); `searchTerm` = chiave query. Submit aggiorna solo `searchTerm`. `useQuery` sta in top-level; `enabled: !!searchTerm` evita fetch a vuoto. Mai `useQuery` dentro l’handler.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | track-em-all `HomePage.tsx` |

### track-em-all — Playwright smoke: anti-flakiness su async UI (`isVisible` race)

| | |
|---|---|
| **Hook** | *in React con `useQuery`/async, evita branching su `isVisible()` al primo render: aspetta un outcome stabile (es. heading “Photos” oppure testo “No photos available”).* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | track-em-all `tests/person.smoke.spec.ts` · `src/pages/PersonPage/PersonPage.tsx` |

### track-em-all — Context solo se serve davvero

| | |
|---|---|
| **Hook** | *Context utile se molti discendenti lontani condividono stato. Se i dati restano in Home (o bastano props a SearchBar/Search), Context è rumore — toglilo.* |
| **Skill** | `@learn-error-simulator` (scenario: “metto tutto in Context”) |
| **Dove** | HomePage (prima Provider, ora props locali) |

### track-em-all — React Query: staleTime vs refetch

| | |
|---|---|
| **Hook** | *staleTime = "ancora fresco, non rifetchare da solo". Stesso searchTerm + submit = niente in Network. refetch() = forza queryFn anche se la cache è fresh.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | cerebro `sources/react/raw/react-query-stale-time-and-refetch.md` · track-em-all `HomePage.tsx` |

### tracking-ds — paginazione API + "misura prima di costruire"

| | |
|---|---|
| **Hook** | *Paginazione API GitLab (`per_page`/`page`, loop finché una pagina torna < 100) — pattern riusabile per qualsiasi API paginata. + euristica di metodo: prima di costruire una feature, **misura il gap con una prova reale** (DEdge-Pay assente dai risultati della discovery → conferma il monorepo-gap) invece di stimare a naso.* |
| **Skill** | `@learn-operational-fast` (pratico/API) |
| **Dove** | tracking-ds `src/discover.ts` (da rileggere lunedì con calma) |

### tracking-ds — fetch: due passaggi, errori HTTP, HTTP/2

| | |
|---|---|
| **Hook** | *`FileReader` = byte che hai già · `fetch` = rete, non interscambiabili. Due passaggi perché gli **header arrivano prima del corpo**: 1° = `Response` (busta + stream non letto), 2° = `.json()` finisce di leggere e parsa. `fetch` **non** rifiuta su 404 → `response.ok` nel **primo** `.then()`; `throw` lì arriva al `.catch()` e copre rete + HTTP + parse. `statusText` è **vuoto in HTTP/2** → usa `status`. `file://` non ha origine → fetch bloccato, serve un server statico.* |
| **Skill** | `@learn-error-simulator` — 3 trap da simulare: check dopo `.json()`, `response` fuori scope nel 2° `.then()`, messaggio d'errore vuoto solo in produzione |
| **Dove** | cerebro `sources/nodejs/raw/fetch-two-steps-and-http-errors.md` · wiki EN `concepts/web/{fetch-response-model,http-errors-in-fetch,http2-reason-phrase}.md` · codice tracking-ds `dashboard/index.html` |
| **Bookmark** | Imparato 31/07 · **ripasso simulator ✓ 2026-08-04** (ok nel 1° then, status non statusText, due passaggi Response→json) |

### tracking-ds — Origine, CORS e i 5 strati

| | |
|---|---|
| **Hook** | *Origine = **schema+host+porta** (path irrilevante, sottodominio sì; `file://` non ne ha). Regola: **incorporare è libero, leggere è vietato** — `<img>/<script>/<link>` passano, `fetch` e **`@font-face`** no (i font sono l'eccezione per **licenza**, non sicurezza). CORS **blocca il browser, non il server**: `curl` non lo applica → non prova nulla. **`vary: Origin` = la risposta cambia per origine** → test su localhost non vale per il dominio vero. E i **5 strati**: DNS / TCP / **HTTP 200** / **policy** / **uso** — il 200 parla solo del terzo.* |
| **Skill** | `@learn-error-simulator` — **i 4 errori di metodo, più utili della teoria**: ① campione non rappresentativo (testato il subset cirillico invece del latino); ② `"unloaded".includes("loaded") === true` → filtro rotto, 3 diventava 28; ③ `document.fonts.check()` dice `true` anche per font **installati nel sistema** → usare `document.fonts.load()` e leggere `status`; ④ `curl -I` manda **HEAD** e testato su **localhost** per concludere su `gitlab.io` |
| **Dove** | cerebro `sources/nodejs/raw/origin-cors-and-the-five-layers.md` · wiki EN `concepts/web/origin-as-security-boundary.md` · codice tracking-ds `dashboard/index.html` (self-hosting font) |
| **Bookmark** | Lezione in 6 tappe il **04/08**. Caso reale: CSS del DS carica da Pages, **font no** (allowlist CDN copre localhost, non gitlab.io) → self-hosting di `inter-latin-400/600/700`. **Non ancora ripassato col simulator.** |

### tracking-ds — GitLab Pages: il modello in 4 pezzi

| | |
|---|---|
| **Hook** | *`public/` = **cosa** · `artifacts: paths:[public]` = **come sopravvive** (+ trasporto tra stage + è ciò che si pubblica) · il **nome del job `pages`** = chi attiva il deploy · **ogni run riuscito, su qualunque branch** = quando → la `rules` sul default branch è obbligatoria. Ogni deploy è uno **snapshot completo che sostituisce**: per cambiare un file ri-produci tutti i file. URL non indovinabile (domini unici = origine separata) → `$CI_PAGES_URL`.* |
| **Skill** | `@learn-error-simulator` — **3 trap su cui sono inciampato** (i mechanismi, non il modello): ① `mkdir -p` **non azzera** la cartella; ② gli artefatti degli stage precedenti **arrivano da soli** nel job successivo (quindi un file prodotto da un altro job finisce pubblicato); ③ deploy che **sostituisce** invece di accumulare (un job che copia solo `latest.json` fa sparire `index.html`) |
| **Dove** | cerebro `sources/nodejs/raw/gitlab-pages-model.md` · codice tracking-ds `.gitlab-ci.yml` |
| **Bookmark** | Pomodoro simulator fatto il **03/08** (4 scenari). Modello ✓ senza esitazione su `rules`, accesso `private`, 404 come information-hiding. **Da rifare solo i 3 mechanismi sopra.** Non promosso in `concepts/` — è specifico di piattaforma, non evergreen. |

### Node / browser — leggere file: input file vs `fs`

| | |
|---|---|
| **Hook** | *Browser: `<input type="file">` → `File`/`Blob` già in memoria → `FileReader` o `file.text()`. Node: path su disco → `fs.readFile` / stream; upload HTTP → multipart (`multer`/`busboy`). Non confondere FileReader (client) con `fs` (server).* |
| **Skill** | `@learn-operational-fast` (primo passaggio) → poi `@learn-error-simulator` |
| **Dove** | da creare: `sources/nodejs/raw/reading-files-browser-vs-node.md` · contrast già in `fetch-two-steps-and-http-errors.md` §1 |
| **Bookmark** | Lacuna dichiarata 2026-07-31 — materiale da costruire settimana prossima (non solo ripasso) |

---

## In corso

| Tema | Stato | Prossimo |
|------|--------|----------|
| DDIA cap. 9 | Blocco principale ✓ (lin, TOB, 2PC, consensus idea, Membership) | **Rilettura completa + note a mano → raw stile ch.8** · poi promote EN |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione |
| track-em-all | Security ✓ · Home RQ ✓ · home smoke ✓ · **Show smoke ✓** | **About polish** (testo + layout, nome Track'em All) → Load more / auth mutation · Open Graph |
| tracking-ds | P0 | lavoro (non serale) |
| Libri coda | Fowler, Makarevich, Head First SA… | dopo blocco DDIA |

---

## Fatto di recente

- **2026-08-04** — Track'em All: Person smoke stabilizzata (evitato `isVisible()` race; attendo “Photos” oppure “No photos available”) + ripasso tests in backlog
- **2026-08-03** — Track'em All: Show smoke chiuso; coda aggiornata: Person smoke → **About polish** (testo+layout) → Load more / mutation
- **2026-07-31** — Backlog: voce **leggere file (input browser vs `fs` Node)** — da costruire raw + ripasso sett. prossima
- **2026-08-04** — **Lezione in 6 tappe su origine/CORS** (teoria richiesta a tappe): origine, embed-vs-read, meccanismo CORS + `vary: Origin`, custom properties + `getComputedStyle`, i 5 strati. Raw + concept EN `origin-as-security-boundary` + voce in Backlog con i **4 errori di metodo**. Caso reale: font del DS bloccati da Pages → self-hosting
- **2026-08-03** — **Pomodoro simulator su GitLab Pages** (4 scenari, ~25 min): modello ✓ (rules, accesso private, 404 deliberato); sbagliati i mechanismi (`mkdir -p`, artefatti tra stage, snapshot che sostituisce) → raw `gitlab-pages-model.md` + voce in Backlog con i 3 trap
- **2026-07-31** — tracking-ds (lavoro, ma materiale di studio estratto): `fetch` lato browser — due passaggi, `response.ok`, `throw`→`catch`, `statusText` vuoto in HTTP/2, `file://` senza origine, `npx`. Raw + 3 concept EN + voce in **Backlog ripasso** (da fare col simulator)
- **2026-07-31** — Track'em All: home smoke Search Results + `test.describe`; **Show smoke iniziato** (`tests/show.smoke.spec.ts` — title + h1 Breaking Bad su `/show/1396`); da finire
- **2026-07-30** — Membership & coordination (ZK/etcd): core-idea + simulator ✓ · raw aggiornato · **piano:** rilettura cap. 9 da capo con note a mano (come ch.8) → raw completo
- **2026-07-29** — Ripasso 2PC + fault-tolerant consensus (vs 2PC) · book-club sezione consensus · 3 refining programma
- **2026-07-28** — DDIA: *Distributed transactions in practice* ✓. **Resta:** Fault-Tolerant Consensus → Membership/coordination (non “8 pp di 2PC”).
- **2026-07-28** — Track'em All: same-term search `refetch()` (`staleTime` 5 min → forza `queryFn` se submit identico); nota cerebro `sources/react/raw/react-query-stale-time-and-refetch.md`
- **2026-07-27** — Track'em All Home: search `useQuery` + Context rimosso; Bugbot fix empty Search on home; **follow-up annotato:** same-term no-refetch (`staleTime` 5 min → `refetch` o `staleTime: 0`)
- **2026-07-27 (mattina, pre-session)** — DDIA 2PC ~20 min: single node→distributed atomic commit, intro 2PC, system of promises, coordinator failure
- **2026-07-24 (mattina)** — Track'em All: mirror `src/utils/favoriteValidation.js` + `useFavorite` (early return, toast max/API, RTK `.unwrap()`, type `ShowData`); keep-in-sync comments front/back
- **2026-07-24** — DDIA: bookmark cap. 9 dopo TOB → **atomic commit / 2PC** (poche pagine); ven = track-em-all mattina, Mongo quiz pomeriggio se possibile
- **2026-07-23** — Ripasso functional core / imperative shell ✓: I/O al bordo · `aggregateRawReport` puro · token iniettati (no `process.env` nel core) → **tolto dall’attivo**
- **2026-07-22 (sera)** — Ripasso favorites ✓: never trust client · duplicato su `showId` · check-then-push non atomico → **tolto dall’attivo**
- **2026-07-22** — Track'em All: chiuso logging + dependency hygiene; security path base completo; aggiornato questo resume
- **2026-07-20** — Track'em All: favorites hardening (formato, duplicato `409`, max 100, `FavoriteSchema` condiviso) + health check
- **2026-07-22** — TOB simulator + confronto lin/TOB/causal; Mongo `$and`/`$or` + `$elemMatch`; piano settimana + pomodoro
- **Lug 2026** — Track'em All security: HTTP hardening, env server-only, JWT expiry, response minimization favorites, Redux `state.auth.favorites`; cerebro raw CORS/CSP/Helmet/rate-limit
- **Lug 2026** — TOB raw; Mongo wiki; CAP, write skew, causal vs lin

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

1. **Attivo ≈ max 3**; in più → **Backlog ripasso** (poi discriminare).
2. **1 obiettivo per sessione** (anche con 2 pomodori).
3. Mer/Gio = solo ripasso da «Da ripassare» (non dal backlog intero).
4. Fine sessione: spunta tabella settimana + aggiorna ripasso (2 min).
5. **Non-core espliciti:** Mongo e basso sono **solo** surplus (non sostituiscono consensus/Raft ripasso).

*Ultimo aggiornamento: 2026-08-05 (DDIA recovery Gio/Ven · Mer saltato)*
