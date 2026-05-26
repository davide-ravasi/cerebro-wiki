# Gestione degli Headers nelle API REST e CORS

## 1. Headers CORS (Cross-Origin Resource Sharing)

Quando un client (es. browser) fa una richiesta a un’API REST che risiede su un dominio diverso, il server deve gestire gli headers CORS per autorizzare o negare l’accesso.

### Headers principali da gestire:

- **Access-Control-Allow-Origin**  
  Specifica quali origini sono autorizzate ad accedere alla risorsa. Può essere un dominio specifico (es. `http://example.com`) o `*` per permettere tutte le origini (meno sicuro).

- **Access-Control-Allow-Methods**  
  Indica quali metodi HTTP sono permessi (es. `GET, POST, PUT, DELETE`).

- **Access-Control-Allow-Headers**  
  Elenca gli headers personalizzati o standard che il client può inviare (es. `Content-Type, Authorization, X-Custom-Header`).

- **Access-Control-Allow-Credentials**  
  Se impostato a `true`, permette l’invio di cookie o credenziali di autenticazione nelle richieste cross-origin.

- **Access-Control-Expose-Headers**  
  Specifica quali headers possono essere letti dal client JavaScript. Di default, solo alcuni headers standard sono accessibili.

- **Access-Control-Max-Age**  
  Indica per quanto tempo la risposta alla preflight request (OPTIONS) può essere memorizzata in cache.

### Esempio di configurazione CORS in Express:

```javascript
app.use(cors({
  origin: 'http://example.com',
  methods: ['GET', 'POST', 'PUT', 'DELETE'],
  allowedHeaders: ['Content-Type', 'Authorization', 'X-Custom-Header'],
  credentials: true,
  exposedHeaders: ['X-Custom-Header']
}));
