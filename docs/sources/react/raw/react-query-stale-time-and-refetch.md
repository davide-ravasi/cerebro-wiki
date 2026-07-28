---
title: React Query — staleTime vs refetch (stesso searchTerm, niente Network)
type: raw-note
tags: [react, react-query, tanstack-query, cache, staleTime, refetch, track-em-all]
provenance: Track'em All HomePage search — 2026-07-28
see_also_app: track-em-all src/pages/HomePage/HomePage.tsx, src/App.tsx (QueryClient)
promote_later: docs/concepts/web/ (EN, se serve materiale generico)
---

# React Query — `staleTime` vs `refetch()`

**Idea centrale:** con una cache “fresca” (`staleTime`), React Query **non** rifà la rete da solo se la `queryKey` non cambia. Resubmit dello **stesso** termine di ricerca non basta: serve **`refetch()`** se vuoi forzare di nuovo `queryFn`.

---

## Contesto (Track'em All)

- **`HomePage.tsx`:** `useQuery({ queryKey: ['search', searchTerm], enabled: !!searchTerm })`
- **`App.tsx`:** `QueryClient` globale con `staleTime: 1000 * 60 * 5` (5 minuti)
- Submit: se termine **nuovo** → `setSearchTerm(trimmed)` (cambia chiave → fetch). Se termine **uguale** → prima niente utile in Network; fix → `refetch()`.

---

## Analogia (opzionale)

La cache è come **avanzo in frigo ancora buono per 5 minuti**: React Query non “ricucina” finché lo considera fresco. **`refetch()`** = *riscaldalo / cucinalo di nuovo lo stesso*, anche se tecnicamente era ancora commestibile.

---

## Flusso: 1° submit vs 2° stesso termine

| Momento | Cosa succede | Network |
|---------|----------------|---------|
| **1° submit** (`"breaking bad"`) | `setSearchTerm('breaking bad')` → `queryKey` cambia → `queryFn` parte | ✅ fetch |
| **2° submit stesso termine** (senza fix) | `setSearchTerm('breaking bad')` → React vede stesso state → RQ: chiave uguale + dati **fresh** → skip | ❌ niente |
| **2° submit stesso termine** (con fix) | `trimmed === searchTerm` → `refetch()` → `queryFn` eseguita **esplicitamente** | ✅ fetch |

---

## Cosa è `refetch()` / cosa **non** è

| È | Non è |
|---|--------|
| Comando esplicito: “riesegui `queryFn` adesso” | Cambiare `searchTerm` o invalidare tutte le query del mondo |
| Funziona **anche se** i dati sono ancora *fresh* dentro `staleTime` | Un reset della cache (`removeQueries`) — i dati vecchi restano finché non arriva la nuova risposta |
| Utile per UX “il pulsante ha fatto qualcosa” | Obbligatorio per ogni ricerca TMDB (catalogo stabile) |

---

## Quando serve vs quando è opzionale

**Utilità prodotto (TMDB search):** limitata — il catalogo TV non cambia ogni secondo; la cache 5 min è spesso ok.

**Valore didattico:** alto — capire *perché* il tab Network resta vuoto al secondo submit identico.

**Alternative al `refetch()` sullo stesso termine:**

1. **Accettare la cache** — nessuna chiamata extra (scelta ragionevole per TMDB).
2. **`staleTime: 0`** solo su quella query — ogni mount/focus può rifetchare (più rete).
3. **`queryClient.invalidateQueries({ queryKey: ['search', term] })`** — marca stale e tipicamente triggera refetch (pattern diverso, utile se più componenti condividono la chiave).

---

## Snippet mentale (fix in HomePage)

```tsx
if (trimmed === searchTerm) {
  refetch();
} else {
  setSearchTerm(trimmed);
}
```

---

## File di riferimento (track-em-all)

- `src/pages/HomePage/HomePage.tsx` — `useQuery`, handler submit, `refetch`
- `src/App.tsx` — `QueryClient` con `staleTime: 1000 * 60 * 5`

---

## Promote (opzionale)

Se serve materiale wiki generico in inglese: estrarre in `docs/concepts/web/` (cache RQ, stale vs fresh, quando usare `refetch` vs `invalidateQueries`).
