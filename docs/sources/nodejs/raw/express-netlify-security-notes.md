---
title: Express su Netlify — sicurezza HTTP (rate limit, body, proxy, errori)
type: raw-note
tags: [nodejs, express, netlify, security, rate-limiting, body-parser, trust-proxy, cors, helmet]
provenance: Apprendimento backend con progetto Track'em All
related_app_repo: react-exp/track-em-all
see_also_raw: rate-limiting-and-client-ip.md
---

# Express su Netlify — note sicurezza (difesa in profondità)

Appunti per il wiki **cerebro** (cartella `nodejs/raw`): cosa fa ogni strato e perché, con riferimento a un’API Express montata come **Netlify Function** (`serverless-http`).

Argomenti collegati: vedi anche **`rate-limiting-and-client-ip.md`** (dettaglio IP, `trust proxy`, `express-rate-limit`).

---

## 1. Rate limiting (volume tentativi)

- **Cosa limita:** quante richieste per **chiave** (di solito IP client risolto dietro proxy) in una **finestra** temporale.
- **Dove:** middleware **prima** del controller, es. `POST /user/login` e `POST /user/register` con limiti diversi (login più frequente della registrazione).
- **Perché:** mitiga brute-force e flood anche se la logica applicativa risponde 401.
- **Serverless:** store in memoria = “best effort” tra istanze; per limite globale forte servirebbe store condiviso (Redis / ecc.).

Dettaglio IP / `keyGenerator` / header `RateLimit`: nel file **`rate-limiting-and-client-ip.md`**.

---

## 2. Limite dimensione body (JSON **e** urlencoded)

### Perché due parser

Su Express spesso hai **entrambi**:

```text
express.json({ limit: '…' })           → Content-Type: application/json
bodyParser.urlencoded({ limit: '…' }) → Content-Type: application/x-www-form-urlencoded
```

Sono **due ingressi** distinti sullo stesso `app`. Se limiti **solo** il JSON, un client può ancora inviare un POST **form** enorme che passa dall’altro middleware.

### Cosa fare

Usare lo **stesso tetto** (o coerente) su **entrambi**, es. `10kb` per un’API solo auth / payload piccoli.

### Esempio

| Richiesta | Content-Type | Middleware che legge il body |
|-----------|----------------|------------------------------|
| `{"email":"…"}` | `application/json` | `express.json` |
| `email=…&password=…` | `application/x-www-form-urlencoded` | `bodyParser.urlencoded` |

Senza `limit` sul secondo: il form può essere molto più grande del JSON cappato.

### Effetto oltre soglia

Il parser smette di leggere e si ottiene un errore (es. **413** o messaggio del body-parser) che conviene far gestire al **middleware errori** globale in JSON `{ message }`, senza stack in produzione.

---

## 3. `trust proxy` e IP reale

Dietro Netlify / CDN la connessione TCP vede spesso il **proxy**.  
`app.set('trust proxy', 1)` aiuta Express a usare bene `req.ip` e gli header `X-Forwarded-*`.  
Per il rate limit, un **`keyGenerator`** che legge `x-forwarded-for` (primo IP) + fallback evita `req.ip` undefined in dev.

---

## 4. Middleware errori Express (4 argomenti)

Express riconosce il gestore errori solo se la funzione ha **arity 4**: `(err, req, res, next)`.  
Con tre parametri il tuo handler **non** viene usato per `next(err)`.  
Il quarto parametro può essere rinominato `_next` se ESLint segnala “unused”.

Ordine: registrare **dopo** tutte le route (incluso fallback SPA se presente).

---

## 5. Ordine suggerito (da imparare / applicare)

1. Validazione input + messaggi coerenti (`401` vs `400`).  
2. Rate limit su endpoint sensibili.  
3. Limite body **json + urlencoded**.  
4. `trust proxy` + risoluzione IP se serve rate limit per IP.  
5. Error handler centrale (JSON, no leak 5xx in prod).  
6. Poi: **§6** — CORS allowlist, variabili solo server, Helmet, hardening route secondarie (es. favorites).

---

## 6. CORS allowlist, Helmet, hardening “secondarie” (es. favorites)

Questo blocco chiude il percorso: dopo auth, body e rate limit sugli endpoint più esposti, si stringe la **superficie** dell’API (chi può chiamarla da browser, quali header HTTP, cosa succede sulle route meno “critiche” ma comunque costose o abusabili).

### 6.1 CORS — perché non basta “funziona con `cors()`”

Approfondimento **generico** (same-origin, preflight `OPTIONS`, esempi HTTP, `credentials`): **`cors-how-it-works.md`** nella stessa cartella `raw/`.

**Cosa fa CORS:** è una policy **del browser**. Il server risponde con header (`Access-Control-Allow-Origin`, ecc.); il browser decide se uno script su `https://sitoA.com` può leggere la risposta di una richiesta verso `https://apiB.com`.  
**Non** sostituisce l’autenticazione: un attaccante può sempre chiamare la tua API con `curl` o un server proprio senza CORS.

**`app.use(cors())` senza opzioni** (come spesso in dev) equivale in pratica a **consentire qualsiasi origine** per richieste “semplici” e a riflettere l’origine nelle preflight quando serve. Per un’API pubblica read-only può essere accettabile; per API che modificano dati utente con token nel header, conviene comunque **restringere** per:

- ridurre rischi legati a **combinazioni** con bug futuri (XSS che legge risposte, estensioni browser, scenari misti);
- allinearsi a una policy chiara: “solo il mio frontend Netlify e localhost in dev”.

**Allowlist (origini esplicite):** passi a `cors` un elenco o una funzione che valida `Origin`:

```js
const allowed = new Set([
  'https://tuodominio.netlify.app',
  'http://localhost:5173',
]);

app.use(
  cors({
    origin: (origin, cb) => {
      // richieste same-origin / tool senza Origin: accetta
      if (!origin || allowed.has(origin)) return cb(null, true);
      return cb(null, false); // browser blocca la lettura della risposta cross-origin
    },
    credentials: true, // solo se usi cookie cross-site; con Bearer puro spesso false
  }),
);
```

**Cookie vs Bearer:** se un giorno usi **cookie** `HttpOnly` per la sessione, CORS + `credentials` diventano centrali: non puoi usare `Access-Control-Allow-Origin: *` con credenziali; serve origine specifica. Con **solo Bearer** in `Authorization`, molte app restano coerenti con allowlist stretta ma `credentials: false`.

**Netlify:** l’origine del browser è tipicamente l’URL del sito (es. `*.netlify.app` o dominio custom); la function ha un URL diverso ma il browser invia comunque `Origin: https://tuo-sito…` nella richiesta al dominio della function (a seconda di come configuri il client). Tieni in allowlist **esattamente** le origini che il frontend usa in build/preview.

---

### 6.2 Helmet + header statici Netlify (pattern adottato)

**`helmet`** non sostituisce validazione o auth; aggiunge header HTTP difensivi sulle **risposte** che escono da Express.

**Pattern consigliato (Track'em All):**

| Layer | File | Cosa |
|--------|------|------|
| **SPA + asset** | `netlify.toml` → `[[headers]]` `for = "/*"` | **CSP** completa (TMDB, font, YouTube `frame-src`), HSTS, `Permissions-Policy`, `X-Frame-Options`, ecc. |
| **API Function** | `functions/express.js` → `helmet({ contentSecurityPolicy: false, … })` | Solo header “generici” sulle risposte JSON (`noSniff`, `frameguard`, `hsts`, …) — **senza** duplicare la CSP |

La CSP conta per il **documento HTML** e per cosa il browser può caricare dalla pagina; ha più senso sui file statici serviti da Netlify che su ogni risposta `application/json`.

**Wiki:** concept [[concept-content-security-policy]] · raw `content-security-policy.md` · pattern [[pattern-csp-netlify-static-express-api]] · mappa [[map-web-security]].

**Ordine in Express:** `cors` → parser body → `helmet` → route.

---

### 6.3 Variabili d’ambiente solo server

Segreti e stringhe di connessione non devono essere esposti al bundle frontend. In Vite i prefissi `VITE_*` sono pensati per il **client**; usarli anche in `functions/express.js` “funziona” su Netlify se li imposti solo lato build/function, ma è **facile confondersi** e finire per documentare o duplicare segreti nel posto sbagliato.

**Pratica consigliata:** nomi dedicati lato server (`MONGODB_URI`, `JWT_SECRET`) nelle Netlify env, letti solo da `process.env` nelle functions, **senza** prefisso che il tooling espone al browser.

---

### 6.4 Hardening route “secondarie” — esempio **favorites**

Nel progetto di riferimento, login/register hanno **rate limit**; le route `POST /favorite/add` e `POST /favorite/remove` sono protette da **JWT** (`protect`) ma tipicamente **non** hanno limite di frequenza. Per un attaccante con token valido (rubato o XSS) o per uno script che automatizza click, questo apre:

- **abuso di scrittura** su MongoDB (`save()` ripetuti);
- **ingombro** dell’array `favorites` (limite massimo preferiti assente?);
- **payload**: `name`, `poster_path`, `vote_average` arrivano dal client — qualcuno potrebbe inserire stringhe enormi o contenuti indesiderati se non validi/ridimensionati (accanto al `10kb` body c’è già un tetto grezzo; resta la validazione **semantica**: lunghezza massima campi, tipo numerico per `vote_average`, `showId` coerente con regole TMDB o intero positivo).

**Misure incrementalmente utili:**

| Area | Idea |
|------|------|
| Rate limit | Un limiter dedicato (più permissivo di login) su `favorite/add` e `favorite/remove`, stessa chiave IP (o user id da token se preferisci bucket per utente). |
| Business rules | Max N preferiti per utente; rifiutare duplicati `showId` in `add` (idempotenza). |
| Validazione | Schema rigido (es. Joi/Zod lato server o validator manuali): `showId` formato atteso, stringhe con `maxLength`. |
| Risposta | Valutare se restituire l’intero `favorites` o solo un ack + conteggio (meno dati in transito; i TODO nel controller sul “rimuovere dati dalla response” vanno in questa direzione). |

**Autorizzazione:** con `req.user.id` preso dal token e `User.findById(userId)` non si “preferisce” un altro utente via body — bene. Resta da garantire che il **payload** non possa sovrascrivere campi sensibili altrove (qui il pattern è già “solo campi favorite espliciti”).

---

## Riferimenti nel repo app

- `functions/express.js` — configurazione pratica (Netlify Functions + Express).
- `docs/RATE-LIMITING-AND-CLIENT-IP.md` (track-em-all) — approfondimento rate limit + IP.

---

*Ultimo aggiornamento: appunti allineati al percorso Track'em All; rivedi i valori numerici (`limit` body, rate limit) se cambi policy.*
