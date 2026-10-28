# Prompt di ripartenza — 2026-10-28

Incolla questo in una chat nuova, nel repo `cerebro-wiki`, dopo il clone.

---

Leggi `docs/study-resume.md` e questo file. Rispondi in italiano, breve. Non rifare la lezione di oggi. Dimmi solo da dove ripartiamo.

Questa chat è stata persa col cambio PC. Le conversazioni Cursor restano sulla macchina vecchia. Il filo è questo file più il resume.

## Chi sono e come studio

Mi chiamo Davide. Studio in questo repo (wiki + resume). Full-stack, traiettoria senior/lead. App principale: track-em-all. Libro in corso: Fowler, *Refactoring* (2ª ed.), circa 10/44 pagine del primo esempio (teatro, test prima, Extract Function). DDIA cap. 9–10 è chiuso.

Mercoledì e giovedì sono in ufficio: un pomodoro, solo ripasso concetti in chat. Niente libro lungo, niente codice deep. Fowler si legge martedì o a casa. Se avanza, su mer/gio si alterna un’idea, non due temi deep lo stesso giorno.

Skill già nel repo (`.cursor/skills/`): `@learn-core-idea-first` (1 idea, analogia, 3 domande una alla volta), `@learn-error-simulator`, `@learn-operational-fast`. Non installare skill esterne.

`Reflect:` lo scrivo io in tre righe (imparato / obiettivo / buco). Tu non le riempi. Il buco va in backlog o «Da ripassare», non in un file nuovo.

`Interleave: [A] + [B]` = uno scenario misto, non due lezioni.

## Cosa è chiuso il 28/10

Ripasso JSON-LD su track-em-all, superato.

- Canonical = indirizzo ufficiale. Open Graph = anteprima (titolo e immagine). JSON-LD = tipo per il motore (`TVSeries`, `TVEpisode`, `Person`).
- Nome e immagine nell’anteprima non dichiarano il tipo.
- Un `@type` sbagliato non sposta l’URL indicizzato. La pagina resta indicizzata a quell’indirizzo; la scheda dichiara un’altra entità.
- Il legame «a quale serie appartiene» è un campo sull’episodio, non il tipo.
- Reflect: ho capito il ruolo e perché sta accanto a canonical e OG. Il buco è il catalogo degli attributi. Non è il ripasso di domani.

Wiki: i concept ci sono già (`docs/concepts/web/json-ld.md`, canonical, open graph, pattern Helmet). Non aggiungere pagine. La frase sul tipo sbagliato si aggiunge al concept solo se dopo il 4/11 voglio tenerla. I campi minimi si decidono in quel ripasso.

## Prossimi slot

- **Gio 30/10** — Next.js Tier 1 su bass-sheets-library (Next 16, App Router, Supabase). Checklist: `docs/sources/nextjs/raw/bass-sheets-gio-checklist.md`. Ordine: Server vs Client Components, App Router, fetch sul server, env (`NEXT_PUBLIC_*` vs secret). Solo concetti. Il codice deep è per casa.
- **Ven** — lab agenti #5 se non fatto, oppure una skill pratica. Un solo obiettivo.
- **Mer 4/11** — ripasso ufficio di tutti i layer SEO e perché: title/description, canonical, OG/Twitter, JSON-LD e campi minimi Show/Episode/Person. Non tutto schema.org.

## Fuori da questa chat

Export Markdown delle altre chat, se le voglio tenere, va fatto sulla macchina vecchia prima di spegnerla. Un export non torna nella sidebar: sul PC nuovo si apre una chat nuova e si indica il file.
