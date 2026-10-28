# Next.js — checklist Gio (bass-sheets-library)

> **Quando:** Gio (~1 pomodoro, **ufficio** = solo ripasso concetti in chat)  
> **Come:** `@learn-core-idea-first` (poi simulator se vuoi verificare). Codice deep / tutorial lunghi = casa o surplus.  
> **Stack:** Next **16** · App Router · Supabase · `react-exp/bass-sheets-library`  
> **Alternanza:** se Fowler avanza, Mer/Gio possono ruotare tema (Next ↔ idea Fowler) — mai dual deep.

## Ordine Gio (Tier 1)

1. [ ] **Server vs Client Components** — default server; `"use client"` solo per stato/eventi
2. [ ] **App Router** — `app/`, `layout.tsx`, `page.tsx`, nested
3. [ ] **Data fetching sul server** — async page / SC; niente `useEffect` per il primo load
4. [ ] **Env** — `NEXT_PUBLIC_*` vs secret solo server (`.env.local` Supabase)

Poi, se resta tempo: **routing** (`[id]`, `Link` / `redirect`).

## Tier 2 — dopo / surplus (uso + colloqui)

- [ ] Caching / revalidate (idea: quando i dati si aggiornano)
- [ ] Server Actions o Route Handlers `app/api`
- [ ] `loading.tsx` / Suspense / streaming (idea)
- [ ] Middleware (auth/redirect, se/quando login)

## Tier 3 — solo colloqui, non bloccanti

- SSR vs SSG vs CSR
- `next/image`
- Deploy + env in prod
- Pages Router = “il vecchio”; tu sei su App Router

## Fuori scope ora

Edge features, PPR dettagliato, i18n, micro-frontends.
