---
title: Rate limiting e IP client (Express + Netlify Functions)
type: raw-note
tags: [nodejs, express, netlify, security, express-rate-limit]
provenance: Track'em All — learning project
canonical_in_app_repo: react-exp/track-em-all/docs/rate-limiting-and-client-ip.md
---

# Rate limiting e IP client (Express + Netlify Functions)

Documento di riferimento per il wiki: riassume la discussione su **`express-rate-limit`**, **`getClientIp`**, **`trust proxy`**, e il comportamento nel progetto **Track'em All** (`functions/express.js`).

> Copia allineata al file omonimo in **track-em-all**; aggiorna entrambi se cambi configurazione o dettagli.

---

## Perché serve una “chiave” (key)

Il rate limiter risponde alla domanda: *quante richieste ha fatto questo client in un intervallo di tempo?*

Per raggruppare le richieste serve una **stringa chiave** (bucket), di solito derivata dall’**IP del client**. Se dietro un proxy l’IP visto da Node è sempre quello del proxy, **tutti gli utenti condividono lo stesso bucket** → limite inutile o troppo aggressivo verso tutti.

Per questo si usa un **`keyGenerator`** personalizzato che calcola l’IP “reale” del client in modo esplicito.

---

## `getClientIp(req)` — logica consigliata

Nel progetto la funzione:

1. Legge **`X-Forwarded-For`** (stringa).  
   Formato tipico dietro proxy: `client, proxy1, proxy2`.  
   Si prende il **primo** elemento (`split(',')[0].trim()`) come IP del browser originale.

2. Se l’header manca o non è una stringa utile, si usano fallback in ordine:
   - **`req.ip`** (meglio dopo `trust proxy`)
   - **`req.socket.remoteAddress`**
   - **`req.connection.remoteAddress`**
   - **`x-nf-client-connection-ip`** (header legato a Netlify quando presente)
   - **`'unknown'`** — evita di passare `undefined` al limiter (errore tipo `ERR_ERL_UNDEFINED_IP_ADDRESS` con `express-rate-limit`).

### Nota sicurezza (apprendimento)

In teoria un client potrebbe inviare un `X-Forwarded-For` falso se la richiesta colpisse il server **senza** un proxy che sanitizza gli header. In produzione su Netlify gli header sono in genere impostati dall’infrastruttura; in locale dipende da come gira la richiesta (`netlify dev`, proxy, ecc.).

---

## `app.set('trust proxy', 1)`

Quando l’app sta **dietro un proxy** (Netlify, load balancer, `netlify dev`):

- **`trust proxy`** dice a Express di considerare **un** hop di proxy fidato e di usare gli header `X-Forwarded-*` per calcoli come **`req.ip`**.
- **`1`** = un solo proxy davanti all’app (caso comune).

Non sostituisce da solo `getClientIp`, ma **migliora** l’affidabilità di `req.ip` nel ramo di fallback.

Implementazione nel repo: subito dopo `const app = express();` in `functions/express.js`.

---

## Opzioni `express-rate-limit` usate nel progetto

| Opzione | Ruolo |
|--------|--------|
| **`windowMs`** | Durata della finestra temporale (es. 10 minuti = `10 * 60 * 1000` ms). |
| **`limit`** | Numero massimo di richieste **per chiave** nella finestra. |
| **`keyGenerator`** | Funzione che restituisce la chiave del bucket (qui: `getClientIp(req)`). |
| **`standardHeaders: 'draft-8'`** | Header moderni `RateLimit` / `RateLimit-Policy` (bozza standard IETF). |
| **`legacyHeaders: false`** | Non inviare i vecchi `X-RateLimit-*`. |
| **`ipv6Subnet: 56`** | Per IPv6, tronca al prefisso /56 per raggruppare host nella stessa subnet sotto una stessa chiave (compromesso tra bloccare abusi e non penalizzare troppi utenti legittimi). |
| **`message`** | Body JSON quando la risposta è **429 Too Many Requests**. |

Due limiter separati (esempio attuale concettuale):

- **Login**: finestra più corta, più tentativi ammessi (tentativi di password).
- **Register**: finestra più lunga, meno tentativi (creazione account più sensibile).

I valori esatti sono definiti in `functions/express.js` (`loginLimiter`, `registerLimiter`).

---

## Dove entra nella catena Express

Esempio:

```text
POST /user/login  →  cors / json / …  →  loginLimiter  →  loginUser
```

1. I middleware globali (`cors`, `express.json`, …) girano prima.
2. **`loginLimiter`**: se il limite è superato → **429** + `message`, e **`loginUser` non viene eseguito`**.
3. **`loginUser`**: logica applicativa (DB, bcrypt, JWT).

Il limiter conta le richieste anche se la risposta finale è **401** (credenziali errate), a meno di non configurare opzioni tipo `skipSuccessfulRequests` (non usate qui): per anti brute-force è di solito **desiderabile** contare anche i 401.

---

## Esempio numerico (login)

Configurazione illustrativa: **`limit: 10`**, **`windowMs`: 10 minuti**, chiave = IP `203.0.113.10`.

| Richiesta | Esito dal limiter | Possibile esito da `loginUser` |
|-----------|-------------------|---------------------------------|
| 1–10 | Passa | Es. 401 o 200 a seconda delle credenziali |
| 11 | **429** + JSON `Too many requests…` | Non eseguito |

Dopo la scadenza/rotazione della finestra, il contatore per quell’IP si resetta e si possono fare di nuovo fino a `limit` richieste nella nuova finestra.

Header di risposta (valori **solo esemplificativi**): `RateLimit-Policy`, `RateLimit` con `remaining`, `reset` — dipendono dalla versione della libreria; l’idea è comunicare al client quanto margine resta.

---

## Limite noto: serverless e memoria

`express-rate-limit` di default usa uno **store in memoria** nel processo Node.

Su Netlify (e simili) possono esserci **più istanze** della function → **contatori non condivisi** tra istanze. Il rate limit resta utile (riduce flood sullo stesso worker / stesso percorso), ma non è equivalente a un limite globale centralizzato (Redis / Upstash, ecc.).

Per progetti in apprendimento o traffico moderato va bene; per hardening “forte” si valuta uno store condiviso in seguito.

---

## File correlati nel repo

- `functions/express.js` — `trust proxy`, `getClientIp`, `loginLimiter` / `registerLimiter`, montaggio su route.
- `functions/controllers/userController.js` — handler `loginUser` / `registerUser` dopo il limiter.

---

## Riferimenti esterni

- [express-rate-limit](https://github.com/express-rate-limit/express-rate-limit) — documentazione ufficiale e note su proxy / `trust proxy`.
- [Express behind proxies](https://expressjs.com/en/guide/behind-proxies.html) — `trust proxy` in dettaglio.

---

*Aggiorna i numeri (`limit`, `windowMs`) se modifichi la configurazione in `functions/express.js`; mantieni allineata la copia in track-em-all `docs/rate-limiting-and-client-ip.md` (se anche lì preferisci il nome in kebab-case).*
