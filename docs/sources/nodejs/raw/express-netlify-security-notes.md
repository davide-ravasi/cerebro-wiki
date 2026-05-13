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
6. Poi: CORS allowlist, variabili solo server (`JWT_SECRET` / `MONGODB_URI`), Helmet, hardening route secondarie.

---

## Riferimenti nel repo app

- `functions/express.js` — configurazione pratica (Netlify Functions + Express).
- `docs/RATE-LIMITING-AND-CLIENT-IP.md` (track-em-all) — approfondimento rate limit + IP.

---

*Ultimo aggiornamento: appunti allineati al percorso Track'em All; rivedi i valori numerici (`limit` body, rate limit) se cambi policy.*
