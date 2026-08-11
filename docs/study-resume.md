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
| **Prossima scadenza** | **2026-08-20** (gio) |
| **Candidati** | fetch due passaggi · CORS/5 strati · 2PC vs consensus · linearizability |

**Track:** quando apri la chat, dimmi la **data del giorno** → controllo se `Prossima scadenza` ≤ oggi. Dopo sessione: Ultima volta + Prossima = +14 giorni.

**Mappa tecniche:** `.cursor/skills/learning-modes/tecniche-apprendimento.md`

---

## Questa settimana — focus

*(settimana 2026-08-10 → 08-14)*

| Giorno | Piano (1 riga) | Fatto? |
|--------|----------------|:------:|
| Lun | track-em-all: Favorites smoke — guest→login + empty seed ✓; lista con favorites **prox volta** | ☑ parziale |
| Mar | DDIA: prossima sezione lin. (impl/costi) — pagine 339–344 fatte; trascrizione in corso | ☑ |
| Mer | ripasso: **auth login/persist/PrivateRoute/favorites** (priorità — gap dallo smoke) | ☐ |
| Gio | ripasso: Pages 3 trap **oppure** RQ enabled/staleTime | ☐ |
| Ven | track-em-all: **Favorites smoke #3** (seed lista) + PR · poi Listing/Episode **oppure** `useMutation` | ☐ |

---

## Da ripassare (attivo — max 3)

*Prossima Mer/Gio (sett. 10–14 ago) o surplus.*

### 1. track-em-all — Auth: login · persist · PrivateRoute · favorites

| | |
|---|---|
| **Hook** | *Gate FE = solo `state.auth.user` (`PrivateRoute`). Persist: key `persist:root`, `auth` doppio-JSON. JWT = `tea-token` per API, non apre le route. Forgare storage ≠ auth server (add/remove favorites serve token valido). Gap emerso dallo smoke Favorites 2026-08-10.* |
| **Skill** | `@learn-core-idea-first` poi `@learn-error-simulator` |
| **Dove** | `PrivateRoute.tsx` · `store.ts` (persist) · `authSlice` / `authService` · `FavoritesPage` · `tests/favorite.smoke.spec.ts` |
| **Bookmark** | **Priorità Mer 12/08** — mappa a voce: login → user+token → persist → gate → favorites API |

### 2. tracking-ds — GitLab Pages: 3 trap meccanici

| | |
|---|---|
| **Hook** | *Modello Pages ok; rinforza: `mkdir -p` non azzera · artefatti stage precedenti arrivano da soli · deploy = snapshot che **sostituisce** (non accumula).* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | `sources/nodejs/raw/gitlab-pages-model.md` · tracking-ds `.gitlab-ci.yml` |
| **Bookmark** | Simulator 03/08: modello ✓; **da rifare solo i 3 trap** |

### 3. track-em-all — React Query: `enabled` + staleTime/refetch

| | |
|---|---|
| **Hook** | *`textInput` = bozza; `searchTerm` = chiave. `enabled: !!searchTerm`. `staleTime` = fresco → no refetch da solo; stesso termine → serve `refetch()`.* |
| **Skill** | `@learn-error-simulator` |
| **Dove** | HomePage · `sources/react/raw/react-query-stale-time-and-refetch.md` |

---

## Backlog ripasso

*Coda: promuovi in «Da ripassare» quando serve.*

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
| favorites / never trust client | 2026-07-22 | — |
| functional core / imperative shell | 2026-07-23 | — |

---

## In corso

| Tema | Stato | Prossimo |
|------|--------|----------|
| DDIA cap. 9 | Linearizability ✓ in raw (def + when useful) | Prossimo libro: impl/costi lin. → TOB… |
| Mongo find | confronti, elemMatch, `$and`/`$or` ✓ | prossima lezione (surplus) |
| track-em-all | Show+Person+About+Load more ✓ · **Favorites smoke 2/3** (manca seed lista) | Prox codice: test favorites popolati → PR · Listing/Episode · `useMutation` · Open Graph |
| tracking-ds | P0 lavoro | ripasso Pages trap (attivo #1) |
| Libri coda | Fowler, Makarevich, Head First SA… | dopo blocco DDIA |

---

## Fatto di recente

- **2026-08-10** — Favorites smoke parziale: guest→`/login` + logged empty (`persist:root`, await `addInitScript`, empty copy fix). **Prox:** seed `favorites[]` + assert cards · poi PR. Ripasso auth → attivo #1 Mer
- **2026-08-07** — Track'em All: Load more — `getNextPageParam` su `total_pages` TMDB, rimosso `maxPages` (cache drop); tipo `ShowResponse`; branch/PR `feat/showlist-infinite-total-pages`
- **2026-08-07** — Track'em All: About polish ✓ (copy 4 sezioni + layout + Footer repo) · PR `feat/updated-about-page`
- **2026-08-07** — DDIA Linearizability note a mano digitizzate (flip nel tempo; useful: locks/leader, uniqueness, cross-channel) → `raw/chapter-9.md`
- **2026-08-07** — DDIA Linearizability: lettura/note a mano finite (foto)
- **2026-08-06 (pomeriggio)** — Track'em All: About rename + PWA manifest (`vite.config.js`)
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
5. **Non-core:** Mongo e basso = solo surplus.
6. **Ogni ~14 giorni:** «Spiega come un lead» — dimmi la data del giorno.

*Ultimo aggiornamento: 2026-08-10 (Favorites smoke 2/3 · prox = seed lista + PR · Mer = ripasso auth)*
