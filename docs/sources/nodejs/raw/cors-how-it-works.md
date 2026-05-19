---
title: CORS — come funziona (browser, header, preflight, esempi HTTP)
type: raw-note
tags: [web, http, cors, security, fetch, axios, express]
provenance: Appunti generici per wiki cerebro (Node / browser / API)
see_also_raw: express-netlify-security-notes.md
---

# CORS — guida tecnica sintetica

**CORS** (Cross-Origin Resource Sharing) è un meccanismo in cui il **browser** applica regole aggiuntive quando una **pagina web** (JavaScript) richiede una risorsa su un’**origine** diversa da quella della pagina. Il **server** risponde con header HTTP che il browser interpreta per decidere se lo script può **leggere** la risposta.

Non sostituisce l’autenticazione: richieste fatte con `curl`, Postman o da un backend non sono soggette alla stessa policy CORS del codice in una scheda del browser.

---

## 1. Origine (same-origin vs cross-origin)

Un’**origine** è la terna **schema + host + porta** (es. `https://esempio.com:443` — la porta default per HTTPS è implicita).

| Confronto | Stesso origine? |
|-----------|-----------------|
| `https://a.com/foo` → `https://a.com/bar` | Sì |
| `https://a.com` → `http://a.com` | No (schema) |
| `https://a.com` → `https://b.com` | No (host) |
| `http://localhost:5173` → `http://localhost:3000` | No (porta) |

Per richieste **same-origin**, il browser **non** applica il controllo CORS “classico” sullo stesso modo delle richieste cross-origin tra siti diversi (il modello mentale utile: CORS è centralissimo quando frontend e API sono su origini diverse o quando vuoi stringere esplicitamente le policy).

---

## 2. Chi fa cosa

| Attore | Ruolo |
|--------|--------|
| **Browser** | Invia `Origin` sulla richiesta (quando applicabile); può inviare prima una richiesta **OPTIONS** (preflight); dopo la risposta, **blocca o consente** allo script l’accesso al corpo/headers sensibili in base agli header `Access-Control-*`. |
| **Server** | Elabora la richiesta e aggiunge (o omette) header `Access-Control-Allow-*` nella risposta. |
| **Libreria server** (es. middleware `cors` in Express) | Imposta quegli header in base alla configurazione (`origin`, `methods`, `headers`, `credentials`). |

Il codice JavaScript sulla pagina **non** “valida” CORS a mano: è il runtime del browser che applica la policy dopo aver ricevuto la risposta.

---

## 3. Header principali (richiesta e risposta)

### Richiesta (dal browser verso l’API)

```http
GET https://api.esempio.com/v1/profile HTTP/1.1
Host: api.esempio.com
Origin: https://app.esempio.com
```

Su richieste “non semplici” (vedi §4) il browser può aggiungere, nella preflight:

```http
Access-Control-Request-Method: POST
Access-Control-Request-Headers: authorization, content-type
```

### Risposta (dall’API verso il browser)

Esempi tipici impostati dal server quando l’origine è consentita:

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://app.esempio.com
Access-Control-Allow-Methods: GET, POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Vary: Origin
```

- **`Access-Control-Allow-Origin`**: deve **coincidere** con l’`Origin` della richiesta (oppure `*` in scenari senza credenziali cookie cross-site; vedi §6).
- **`Vary: Origin`**: utile per cache HTTP/CDN quando la risposta cambia in base all’origine.

---

## 4. Richieste “semplici” vs preflight (OPTIONS)

**Richiesta semplice** (regola storica, semplificata): metodi come `GET`/`HEAD`/`POST` con certi `Content-Type` limitati (es. `application/x-www-form-urlencoded`, `multipart/form-data`, `text/plain`) e senza header “non semplici” personalizzati. Il browser può inviare direttamente la richiesta e controllare la risposta.

**Richiesta non semplice** (molto comune in API moderne): ad esempio `POST` con **`Content-Type: application/json`** o con header tipo **`Authorization`**. Il browser invia prima:

```http
OPTIONS /v1/login HTTP/1.1
Host: api.esempio.com
Origin: https://app.esempio.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type, authorization
```

Il server deve rispondere alla **OPTIONS** con status `204` o `200` e gli header `Access-Control-Allow-*` coerenti. Solo dopo il browser invia il **POST** reale.

Esempio risposta preflight:

```http
HTTP/1.1 204 No Content
Access-Control-Allow-Origin: https://app.esempio.com
Access-Control-Allow-Methods: POST, OPTIONS
Access-Control-Allow-Headers: Content-Type, Authorization
Access-Control-Max-Age: 86400
```

---

## 5. Esempio end-to-end: login JSON cross-origin

**Pagina:** `https://app.esempio.com`  
**API:** `https://api.esempio.com/v1/login`

### (A) Preflight

```http
OPTIONS /v1/login HTTP/1.1
Host: api.esempio.com
Origin: https://app.esempio.com
Access-Control-Request-Method: POST
Access-Control-Request-Headers: content-type
```

Risposta consentita → il browser prosegue.

### (B) Richiesta effettiva

```http
POST /v1/login HTTP/1.1
Host: api.esempio.com
Origin: https://app.esempio.com
Content-Type: application/json

{"email":"user@esempio.com","password":"***"}
```

### (C) Risposta con CORS ok

```http
HTTP/1.1 200 OK
Access-Control-Allow-Origin: https://app.esempio.com
Content-Type: application/json

{"token":"eyJ..."}
```

Lo script sulla pagina può leggere il JSON.

### (D) Risposta senza CORS ok (origine non autorizzata)

```http
HTTP/1.1 200 OK
Content-Type: application/json

{"token":"eyJ..."}
```

Manca `Access-Control-Allow-Origin: https://app.esempio.com` → il browser **non** espone il body allo script (errore in console tipo “CORS policy”). Nota: il server potrebbe aver comunque **processato** il login; CORS non annulla la logica server, regola solo cosa il **browser** lascia vedere al JS.

---

## 6. Credenziali (`credentials`)

Se il frontend usa **`fetch(..., { credentials: 'include' })`** o axios **`withCredentials: true`**, il browser può inviare **cookie** di sessione cross-origin. In quel caso:

- **`Access-Control-Allow-Origin: *`** non è ammesso insieme a credenziali.
- Serve un’origine **esplicita** e **`Access-Control-Allow-Credentials: true`** lato server.

Con **solo token Bearer** nell’header `Authorization` (senza cookie cross-site), spesso si lavora con `credentials: false` e allowlist di `Origin` per stringere comunque la superficie.

---

## 7. Express (pacchetto `cors`) — pattern allowlist

Esempio didattico: consentire solo origini note (variabile d’ambiente comma-separated).

```js
const allowed = new Set(
  (process.env.CORS_ALLOWED_ORIGINS || '')
    .split(',')
    .map((s) => s.trim())
    .filter(Boolean),
);

app.use(
  require('cors')({
    origin(origin, callback) {
      if (!origin) return callback(null, true);
      if (allowed.has(origin)) return callback(null, true);
      return callback(null, false);
    },
    credentials: false,
  }),
);
```

- **`origin` assente**: strumenti senza header `Origin`; spesso si accetta per non rompere health check (valuta policy per il tuo contesto).
- **`callback(null, false)`**: il browser bloccherà l’accesso allo script per quell’origine.

---

## 8. Errori comuni (checklist)

1. **Mismatch esatto:** `http://localhost:5173` ≠ `http://127.0.0.1:5173` (host diverso → origine diversa).
2. **Preflight non gestita:** manca risposta corretta a `OPTIONS` (reverse proxy che non inoltra OPTIONS, 404 sulla route).
3. **Header custom:** se il client invia `X-My-Header`, la preflight deve includerlo in `Access-Control-Allow-Headers` (o il middleware `cors` va configurato con `allowedHeaders`).
4. **Credenziali + wildcard:** `*` con cookie cross-origin non funziona.
5. **Confondere CORS con auth:** API pubblica senza auth resta “chiamabile” da server arbitrari; CORS limita principalmente **lettura risposta da pagine web** su altre origini.

---

## 9. Riferimenti

- [MDN — CORS](https://developer.mozilla.org/en-US/docs/Web/HTTP/CORS)
- [Fetch spec — CORS protocol](https://fetch.spec.whatwg.org/#http-cors-protocol) (dettaglio normativo)
- [npm `cors`](https://www.npmjs.com/package/cors) — middleware Express

---

*Nota generica per il wiki cerebro; adatta esempi host/porte al tuo ambiente (Netlify, Vite, dominio custom).*
