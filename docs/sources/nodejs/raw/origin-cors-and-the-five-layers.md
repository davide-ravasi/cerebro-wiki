# Origine, CORS e i cinque strati — "è arrivato" ≠ "funziona"

> Raw, italiano. Imparato il **04/08/2026** collegando il CSS del DS Overnight Hotelier alla
> dashboard di `tracking-ds` pubblicata su GitLab Pages. Lezione in 6 tappe.
> ⚠️ La parte più utile non è la teoria CORS — è **la sequenza di errori di metodo che ho visto
> fare all'assistente**, tutti riconducibili a "testare lo strato sbagliato nell'ambiente sbagliato".
> Caso reale: il CSS del DS carica da Pages, i **font no** (CORS error).

---

## 1. Che cos'è un'origine

Tripletta **schema + host + porta**. Tutti tre devono coincidere per essere *same-origin*.

```
https :// ds-tracker-9c2c7d.gitlab.io : 443
http  :// localhost                   : 3000
```

| URL vs `http://localhost:3000` | Same-origin? | Perché |
|---|:--:|---|
| `http://localhost:3000/latest.json` | ✅ | cambia solo il percorso — **il path non conta mai** |
| `http://localhost:4000/` | ❌ | porta |
| `https://localhost:3000/` | ❌ | schema |
| `http://127.0.0.1:3000/` | ❌ | host — **anche se è la stessa macchina** |

Sottodomini diversi = origini diverse (`a.example.com` ≠ `b.example.com`).

Due conseguenze già incontrate:
- **`file://` non ha un'origine utilizzabile** → il fetch è bloccato a monte. Da qui `npx serve`.
- `latest.json` è same-origin → nessun problema. Il CSS del DS è cross-origin → inizia CORS.

Nome della regola di base: **same-origin policy**. Tutto il resto sono le eccezioni.

## 2. Cosa è libero e cosa richiede permesso

> **Incorporare è libero. Leggere è vietato.**

Il browser lascia *usare* una risorsa altrui, ma non lascia il **JavaScript** leggerne il contenuto.
Storico: `<img>`, `<script>`, `<link>` sono nati cross-origin (è così che esistono i CDN); CORS è
arrivato dopo, per ciò che espone **dati** al codice.

| Risorsa | Senza CORS? | Perché |
|---|:--:|---|
| `<img>` | ✅ | il browser disegna, JS non vede i pixel |
| `<script src>` | ✅ | il browser esegue, JS non legge il sorgente |
| `<link rel="stylesheet">` | ✅ | il browser applica, JS non legge le regole |
| `fetch` / `XHR` | ❌ | JS legge il corpo per definizione |
| **`@font-face`** | ❌ | **l'eccezione** |

**La prova che la regola è "leggere" e non "usare"** — tre facce dello stesso principio:
- immagine cross-origin: si mostra, ma su `<canvas>` + `getImageData()` → *tainted canvas*, errore
- script cross-origin: si esegue, ma `window.onerror` riceve solo `"Script error"` sanificato
- foglio di stile cross-origin: si applica, ma `document.styleSheets[i].cssRules` → **`SecurityError`**

**Perché i font sono l'eccezione:** un font è solo renderizzato, JS non ne legge i byte — per la regola
dovrebbe essere libero. La specifica CSS impone comunque la modalità CORS, e **il motivo non è di
sicurezza ma commerciale**: i produttori di font volevano poter bloccare l'hotlinking. Unico punto
dove il modello si piega per ragioni di licenza.

## 3. Il meccanismo CORS

Non è un blocco: è un **protocollo di autorizzazione**.

```
1. BROWSER  manda la richiesta con:   Origin: http://localhost:3000
2. SERVER   risponde, e decide se:    Access-Control-Allow-Origin: ...
3. BROWSER  confronta e decide se consegnare il contenuto al codice
```

Tre punti controintuitivi:
- **La richiesta parte comunque** e il server la serve. CORS non impedisce il viaggio, impedisce la
  *consegna al codice*. Per questo si vede `200` anche dove poi fallisce.
- **Blocca il browser, non il server.** `curl` non applica CORS perché non ha origini da proteggere
  → **`curl` "funziona sempre" e non dimostra che funzionerà nella pagina.**
- **Autorizza il server di destinazione.** Non aggirabile dal client: nessuna opzione di `fetch`,
  nessun attributo HTML.

Risposta reale del CDN D-EDGE su un font (con Origin localhost):
```
access-control-allow-origin: http://localhost:3000     ← riecheggia il dominio, non "*"
access-control-allow-methods: GET, HEAD
vary: Origin, Access-Control-Request-Headers, Access-Control-Request-Method
```

**`vary: Origin` è la chiave, e l'ho letta senza capirla.** È un header di *caching*: dice a proxy e
CDN "la mia risposta cambia in base a questo header della richiesta, non riusare la stessa copia".
Serve a evitare l'avvelenamento della cache (il primo visitatore da `localhost` riempirebbe la cache
con la SUA autorizzazione, e il successivo da `gitlab.io` riceverebbe quella sbagliata).

→ **Corollario che avrei dovuto trarre subito: la risposta misurata con `Origin: localhost` NON è
quella che riceverà Pages.** Ed era esattamente il caso: **localhost autorizzato, gitlab.io no.**

Due modalità: *simple request* (GET/HEAD/POST con header banali → una sola richiesta) e *preflight*
(metodi o header non banali → prima un `OPTIONS` di cortesia). CSS e font sono simple.

## 4. 🔴 I quattro errori di metodo (la parte da ricordare)

**① Campione non rappresentativo.** Ho testato la prima `url()` trovata nel CSS di Inter —
`inter-cyrillic-ext-400` — che non ha header CORS. Il file che la pagina usa davvero è
`inter-latin-400`, che li ha. Conclusione generalizzata da un campione sbagliato.

**② Confronto per sottostringa su un enum.**
```js
faces.filter(s => s.includes('loaded')).length   // → 28  (FALSO)
```
`"unloaded".includes("loaded") === true`! Contava tutto. Il dato vero era **25 unloaded, 3 loaded**.
→ **Sugli stati usa l'uguaglianza, mai la sottostringa.** Vale per `'inactive'/'active'`,
`'disabled'/'abled'`, `'unauthorized'/'authorized'`.

**③ API ambigua invece di quella diagnostica.**
```js
document.fonts.check('16px Inter')   // true ANCHE se Inter è installato nel SISTEMA
```
Su una macchina da sviluppatore Inter c'è spesso → `true` per un motivo che non riguarda il CDN.
Il test che distingue:
```js
const faces = await document.fonts.load('400 16px Inter');
faces.map(f => f.status);   // 'loaded' = arrivato dalla rete · 'error' = fallito
```

**④ `HEAD` invece di `GET`, e `localhost` invece dell'origine vera.** `curl -I` manda HEAD, che non
è la richiesta del browser; e testare su `localhost` per concludere su `gitlab.io` con `vary: Origin`
sotto gli occhi è l'errore che ha invalidato tutto.

### Caricamento lazy dei font (perché 25 su 28 erano `unloaded`)

Il CSS di Inter dichiara ~28 `@font-face`: subset (latin, latin-ext, cyrillic, greek…) × pesi.
**Il browser scarica solo quelli richiesti da testo effettivamente in pagina.** Le 3 `loaded` erano
esattamente `inter-latin-400/600/700` — i pesi usati dalla pagina, solo subset latino.

```
unloaded  →  loading  →  loaded
                      ↘  error
```
**`unloaded` = "mai richiesta", NON "fallita".** Distinzione che il filtro rotto aveva cancellato.

## 5. Custom properties CSS

```css
:root { --htl-color-background-brand-strong: #0057b8; }
.x   { background: var(--htl-color-background-brand-strong, #eee); }
```

Differenze da Sass/Less:
- **vivono a runtime**, non a compile time → modificabili da JS, reattive alle media query
- **ereditano lungo il DOM** — non globali: dichiarate su un elemento, valgono per i discendenti.
  Su `:root` (= `<html>`) valgono per tutta la pagina, ed è ciò che fa il DS.

**Come ho verificato che il CSS del DS era attivo:**
```js
getComputedStyle(document.documentElement)
  .getPropertyValue('--htl-color-background-brand-strong')   // non vuoto → il DS è applicato
```
Prova indiretta ma solida: quella proprietà non esiste nello `<style>` locale, quindi può venire
solo dal DS. **Non ho verificato che il file sia arrivato — ho verificato che il suo effetto ci sia.**

Due dettagli di metodo:
- `document.documentElement` (= `<html>`), dove il DS le dichiara. Su `body` funziona per
  ereditarietà ma è più fragile.
- **`getComputedStyle`, non `element.style`** — quest'ultimo legge solo gli stili *inline*, non quelli
  che arrivano dai fogli. Errore classico.

Contrasto interessante con la tappa 2: JS **non può** leggere `cssRules` del foglio cross-origin
(`SecurityError`) **ma può** leggere il valore calcolato di una custom property. Coerente: nel secondo
caso non leggi il file, leggi lo stato dell'elemento dopo l'applicazione.

**Token semantici, non descrittivi** — 166 `--htl-*` nel DS:
```
--htl-color-background-critical-weak     non "quale colore" ma "a cosa serve"
--htl-color-background-brand-strong
```
Vantaggio: se il DS cambia tonalità, chi usa `critical-weak` si aggiorna gratis; chi ha scritto
`#dcfce7` resta indietro.

**Il fallback è il degradare invece di collassare:** `var(--token, #fef2f2)`. Senza, se il CSS del DS
non carica hai sfondi trasparenti e testo illeggibile. Stesso principio del
`` `HTTP ${response.status}` `` per i messaggi d'errore.

## 6. I cinque strati — il filo di tutto

```
1. DNS      il nome si risolve?
2. TCP/TLS  la connessione si apre?
3. HTTP     il server risponde 200?
4. POLICY   il browser autorizza l'uso?     ← CORS vive qui
5. USO      il codice/CSS lo consuma?
```

**Uno status 200 parla SOLO dello strato 3.** Dice "i byte sono stati consegnati". Niente sul 4 e 5.

I quattro fallimenti reali di quella sessione, uno per strato:

| Cosa | Strato ceduto | Sintomo |
|---|---|---|
| Font dal CDN | **4 policy** | 200 in `curl`, **CORS error** nel browser |
| CSS del DS | **5 uso** | caricato, zero classi `htl-` → 167 KB senza effetto |
| `addEventListener` su `null` | **5 uso** | script scaricato e parsato, interrotto alla prima riga rotta |
| `new File([nome], nome)` | **5 uso** | nessun errore di rete, dati semplicemente sbagliati |

→ **Tre su quattro sono invisibili al pannello rete.**

### La regola

**Testa allo strato dove vive l'affermazione, e nell'ambiente dove conta.**

| Affermazione | Test giusto |
|---|---|
| "il file esiste sul server" | `curl` con **GET** e l'`Origin` reale |
| "il browser lo accetta" | pannello rete **sull'origine di produzione** |
| "il font è davvero usato" | `document.fonts.load()` → `status` (non `check()`) |
| "il CSS ha effetto" | `getComputedStyle` su una proprietà che solo lui definisce |
| "la pagina funziona" | guardarla, all'URL vero |

**Corollario:** `localhost` non è un ambiente valido per testare nulla che dipenda dall'origine —
CORS, cookie, CSP, `Secure`, service worker si comportano tutti diversamente.

---

## Da ricordare (5 righe)

1. Origine = **schema + host + porta**; il path non conta, il sottodominio sì. `file://` non ne ha una.
2. **Incorporare è libero, leggere è vietato.** I font sono l'eccezione, per ragioni di licenza non di sicurezza.
3. CORS **blocca il browser, non il server**: `curl` non lo applica → non dimostra nulla sulla pagina.
4. **`vary: Origin` = la risposta cambia per origine** → un test su localhost non vale per il dominio vero.
5. **200 riguarda solo lo strato HTTP.** Policy e uso sono due strati dopo, e falliscono in silenzio.

## Esito pratico sul progetto

Font dal CDN D-EDGE **inutilizzabili da Pages** (allowlist copre `localhost`, non `gitlab.io`).
Il CSS invece funziona (i `<link>` non richiedono CORS). Soluzione: **self-hosting dei font** —
solo `inter-latin-400/600/700` (i pesi che la pagina rende), serviti dalla stessa origine
(`dashboard/fonts/` in locale, `public/fonts/` su Pages) → CORS non si applica affatto.
Montserrat non serve: lo usa solo `.htl-u-typography-heading-*`, che questa pagina non usa.
