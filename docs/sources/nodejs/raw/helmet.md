---
title: Helmet.js — middleware header HTTP di sicurezza (Express)
type: raw-note
tags: [web, http, helmet, security, express, xss, csp]
provenance: Appunti generici + contesto Track'em All (CSP su Netlify, Helmet sulla Function)
see_also_raw: content-security-policy.md, express-netlify-security-notes.md, cors-how-it-works.md
related_concept: docs/concepts/web/content-security-policy.md
---

# Cos’è Helmet.js

Helmet.js è un middleware per Express/Node.js che imposta header HTTP utili per mitigare vulnerabilità comuni (XSS, clickjacking, MIME sniffing, ecc.).

## Perché usarlo

Riduce il rischio di attacchi senza configurare manualmente ogni header; è semplice da integrare e altamente configurabile.

**Wiki:** per CSP sullo static vs API → [[concept-content-security-policy]], pattern [[pattern-csp-netlify-static-express-api]].

## Installazione

```bash
npm install helmet
# oppure
yarn add helmet
```

## Uso base con Express

```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();
app.use(helmet());

app.get('/', (req, res) => {
  res.send('Hello, la tua app è protetta con Helmet.js!');
});

app.listen(3000, () => console.log('Server in ascolto sulla porta 3000'));
```

Chiamando `app.use(helmet())` si applicano molte protezioni preconfigurate.

**Ordine consigliato in Express:** `cors` → parser body → `helmet` → route.

---

## Principali funzionalità

### Content Security Policy (CSP)

Limita le origini da cui il browser può caricare script, stili, immagini, font, ecc.

```javascript
app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", 'https://trusted-cdn.com'],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", 'https://images.example.com'],
      fontSrc: ["'self'", 'https://fonts.googleapis.com'],
    },
  })
);
```

Su **API JSON** dietro Netlify spesso conviene **`contentSecurityPolicy: false`** e mettere la CSP completa sui file statici (`netlify.toml`). Vedi `content-security-policy.md`.

### X-Frame-Options (frameguard)

Previene l’inclusione del sito in iframe (clickjacking).

```javascript
app.use(helmet.frameguard({ action: 'deny' }));
// deny: blocca tutti gli iframe
// sameorigin: solo stesso dominio
```

### X-XSS-Protection (xssFilter)

Filtro anti-XSS legacy in alcuni browser (comportamento variabile nei browser moderni).

```javascript
app.use(helmet.xssFilter());
```

### Strict-Transport-Security (HSTS)

Forza HTTPS per un periodo definito.

```javascript
app.use(
  helmet.hsts({
    maxAge: 31536000,
    includeSubDomains: true,
    preload: true,
  })
);
```

### X-Content-Type-Options (noSniff)

Impedisce al browser di “indovinare” il MIME type.

```javascript
app.use(helmet.noSniff());
```

### Referrer-Policy

Controlla quante informazioni del referrer vengono inviate.

```javascript
app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
```

Opzioni comuni: `no-referrer`, `same-origin`, `strict-origin-when-cross-origin`.

### Expect-CT

Certificate Transparency (header deprecato in molti browser; Helmet può ancora esporlo).

```javascript
app.use(
  helmet.expectCt({
    maxAge: 86400,
    enforce: true,
    reportUri: 'https://example.com/report',
  })
);
```

### Personalizzazione

Disattiva singoli moduli o passa opzioni per middleware:

```javascript
app.use(
  helmet({
    contentSecurityPolicy: false,
    frameguard: { action: 'sameorigin' },
    hsts: { maxAge: 31536000, includeSubDomains: true },
  })
);
```

---

## Best practice

- Mantieni Helmet aggiornato.
- Personalizza CSP per CDN, font, embed (YouTube, TMDB, ecc.).
- Non sostituisce validazione input, auth o rate limiting.
- Testa gli header (DevTools → Network, OWASP ZAP).

---

## Esempio completo (Express)

```javascript
const express = require('express');
const helmet = require('helmet');

const app = express();

app.use(helmet());

app.use(
  helmet.contentSecurityPolicy({
    directives: {
      defaultSrc: ["'self'"],
      scriptSrc: ["'self'", 'https://trusted-cdn.com'],
      styleSrc: ["'self'", "'unsafe-inline'"],
      imgSrc: ["'self'", 'https://images.example.com'],
      fontSrc: ["'self'", 'https://fonts.googleapis.com'],
    },
  })
);

app.use(helmet.frameguard({ action: 'deny' }));
app.use(helmet.xssFilter());
app.use(helmet.hsts({ maxAge: 31536000, includeSubDomains: true, preload: true }));
app.use(helmet.noSniff());
app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
app.use(
  helmet.expectCt({
    maxAge: 86400,
    enforce: true,
    reportUri: 'https://example.com/report',
  })
);

app.get('/', (req, res) => {
  res.send('App protetta con Helmet.js e configurazioni personalizzate!');
});

app.listen(3000, () => console.log('Server in ascolto sulla porta 3000'));
```

---

## Conclusione

Helmet è uno strumento semplice per aumentare la sicurezza di app Node/Express con poche righe. Integralo con CORS, limiti body, rate limit e — per le SPA — una CSP coerente sul layer che serve l’HTML.
