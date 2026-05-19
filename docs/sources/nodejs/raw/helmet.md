Cos’è Helmet.js Helmet.js è un middleware per Express/Node.js che migliora la sicurezza dell’app impostando automaticamente vari header HTTP utili per mitigare vulnerabilità comuni (XSS, clickjacking, MIME sniffing, ecc.).

Perché usarlo Riduce il rischio di attacchi senza dover configurare manualmente ogni singolo header; è semplice da integrare e altamente configurabile.

Installazione

Con npm: npm install helmet
Con yarn: yarn add helmet
Uso base con Express Esempio minimale: const express = require('express'); const helmet = require('helmet');

const app = express(); app.use(helmet());

app.get('/', (req, res) => { res.send('Hello, la tua app è protetta con Helmet.js!'); });

app.listen(3000, () => console.log('Server in ascolto sulla porta 3000'));

Nota: chiamando app.use(helmet()) si applicano automaticamente molte protezioni preconfigurate.

Principali funzionalità (con breve descrizione ed esempi)

Content Security Policy (CSP) Limita le origini da cui possono essere caricati script, stili, ecc., per prevenire XSS. Esempio: app.use(helmet.contentSecurityPolicy({ directives: { defaultSrc: ["'self'"], scriptSrc: ["'self'", "trusted-cdn.com"] } }));
X-Frame-Options (frameguard) Previene l’inclusione del sito in iframe (protezione da clickjacking). Esempio: app.use(helmet.frameguard({ action: 'deny' }));
X-XSS-Protection Aiuta a bloccare alcuni tipi di XSS riflessi (nota: alcuni browser moderni hanno comportamenti differenti). Esempio: app.use(helmet.xssFilter());
Strict-Transport-Security (HSTS) Forza l’uso di HTTPS per il sito per un periodo specificato. Esempio: app.use(helmet.hsts({ maxAge: 31536000, includeSubDomains: true }));
X-Content-Type-Options (noSniff) Impedisce ai browser di “indovinare” il MIME type, riducendo rischi. Esempio: app.use(helmet.noSniff());
Referrer-Policy Controlla quante informazioni del referrer vengono inviate con le richieste. Esempio: app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
Expect-CT Aiuta a rilevare certificati SSL/TLS emessi in modo improprio (Certificate Transparency). Esempio: app.use(helmet.expectCt({ maxAge: 30 }));
Personalizzazione Helmet è flessibile: puoi disattivare singoli moduli o passare opzioni per adattarlo alle esigenze dell’app. Esempio: app.use(helmet({ contentSecurityPolicy: false, // disattiva CSP frameguard: { action: 'sameorigin' }, hsts: { maxAge: 31536000, includeSubDomains: true } }));

Best practice

Mantieni Helmet aggiornato.
Personalizza gli header in base ai requisiti della tua app (CSP spesso richiede modifiche per consentire CDN, inline scripts, ecc.).
Usa Helmet insieme ad altre misure di sicurezza: rate-limiting, autenticazione, HTTPS obbligatorio.
Testa la sicurezza con strumenti come OWASP ZAP o servizi di controllo degli header di sicurezza.
Conclusione Helmet.js è uno strumento fondamentale e semplice da usare per aumentare la sicurezza delle applicazioni Node/Express con poche righe di codice. Integrare Helmet e seguire buone pratiche aiuta a ridurre molti rischi comuni.

Ecco esempi concreti per ciascuno dei principali punti di Helmet.js, con codice e spiegazioni pratiche per capire come usarli in un’app Node.js con Express:

1) Content Security Policy (CSP)
Scopo: Limitare le origini da cui il browser può caricare risorse (script, stili, immagini, font, ecc.) per prevenire attacchi XSS.

const helmet = require('helmet');

app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"], // consente solo risorse dal proprio dominio
    scriptSrc: ["'self'", "https://trusted-cdn.com"], // script solo dal proprio dominio e CDN fidato
    styleSrc: ["'self'", "'unsafe-inline'"], // stili dal proprio dominio e inline (se necessario)
    imgSrc: ["'self'", "https://images.example.com"], // immagini dal proprio dominio e da un dominio esterno
    fontSrc: ["'self'", "https://fonts.googleapis.com"] // font da Google Fonts
  }
}));
Uso pratico: Blocca script o risorse non autorizzate, riducendo il rischio che codice malevolo venga eseguito.

2) X-Frame-Options (frameguard)
Scopo: Evitare che il sito venga caricato in un iframe da altri siti, prevenendo attacchi di clickjacking.

app.use(helmet.frameguard({ action: 'deny' }));
deny: blocca completamente l’inclusione in iframe.
Alternativa: sameorigin permette iframe solo dallo stesso dominio.
Uso pratico: Protegge pagine con contenuti sensibili da essere “incorniciate” e cliccate in modo fraudolento.

3) X-XSS-Protection
Scopo: Attivare il filtro anti-XSS integrato in alcuni browser.

app.use(helmet.xssFilter());
Nota: Alcuni browser moderni hanno disabilitato o modificato questo filtro, ma può ancora offrire una protezione aggiuntiva.

4) Strict-Transport-Security (HSTS)
Scopo: Forzare il browser a usare solo HTTPS per il sito per un certo periodo.

app.use(helmet.hsts({
  maxAge: 31536000, // 1 anno in secondi
  includeSubDomains: true, // applica anche ai sottodomini
  preload: true // permette l’inclusione nella lista preload dei browser
}));
Uso pratico: Evita che utenti accedano accidentalmente via HTTP non sicuro, riducendo attacchi di tipo man-in-the-middle.

5) X-Content-Type-Options (noSniff)
Scopo: Impedire ai browser di “indovinare” il tipo MIME di una risorsa, riducendo rischi di esecuzione di codice malevolo.

app.use(helmet.noSniff());
Uso pratico: Se un file viene servito con un tipo MIME errato, il browser non tenterà di interpretarlo come un altro tipo.

6) Referrer-Policy
Scopo: Controllare quante informazioni sull’origine della richiesta (referrer) vengono inviate ai siti esterni.

app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
Alcune opzioni comuni:

no-referrer: non invia mai il referrer.
same-origin: invia referrer solo per richieste allo stesso dominio.
strict-origin-when-cross-origin: invia referrer completo solo per richieste sicure allo stesso dominio.
Uso pratico: Protegge la privacy degli utenti evitando di rivelare URL interni o dati sensibili tramite referrer.

7) Expect-CT
Scopo: Aiutare a rilevare certificati SSL/TLS emessi in modo improprio tramite Certificate Transparency.

app.use(helmet.expectCt({
  maxAge: 86400, // durata in secondi (es. 1 giorno)
  enforce: true, // applica la policy
  reportUri: 'https://example.com/report' // endpoint per i report
}));
Uso pratico: Migliora la sicurezza SSL/TLS rilevando certificati non conformi.

Esempio completo di integrazione Helmet.js in Express
const express = require('express');
const helmet = require('helmet');

const app = express();

app.use(helmet()); // applica tutte le protezioni di default

// Personalizzazioni specifiche
app.use(helmet.contentSecurityPolicy({
  directives: {
    defaultSrc: ["'self'"],
    scriptSrc: ["'self'", "https://trusted-cdn.com"],
    styleSrc: ["'self'", "'unsafe-inline'"],
    imgSrc: ["'self'", "https://images.example.com"],
    fontSrc: ["'self'", "https://fonts.googleapis.com"]
  }
}));

app.use(helmet.frameguard({ action: 'deny' }));
app.use(helmet.xssFilter());
app.use(helmet.hsts({ maxAge: 31536000, includeSubDomains: true, preload: true }));
app.use(helmet.noSniff());
app.use(helmet.referrerPolicy({ policy: 'no-referrer' }));
app.use(helmet.expectCt({ maxAge: 86400, enforce: true, reportUri: 'https://example.com/report' }));

app.get('/', (req, res) => {
  res.send('App protetta con Helmet.js e configurazioni personalizzate!');
});

app.listen(3000, (


