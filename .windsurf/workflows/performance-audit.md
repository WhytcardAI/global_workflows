---
name: performance-audit
description: Audit performance (bundle size, lazy loading, memoization, N+1, memory leaks)
---

# Performance Audit

## Objectif

Identifier et corriger les **problèmes de performance** : bundle trop gros, requêtes N+1, memory leaks, rendering inutile, assets non optimisés.

## Quand l'utiliser

- Avant une release en production.
- Quand les utilisateurs signalent des lenteurs.
- Après ajout de dépendances lourdes.
- Audit périodique (mensuel).

## Pipeline

### 1) Bundle Size (Frontend)

- [ ] Analyser la taille du bundle.
- [ ] Identifier les dépendances lourdes.
- [ ] Vérifier le tree-shaking.

**Commandes :**

```bash
# Next.js
npx @next/bundle-analyzer

# Vite
npx vite-bundle-visualizer

# Webpack
npx webpack-bundle-analyzer
```

**Seuils recommandés :**

- Initial JS : < 200KB gzipped
- Largest chunk : < 100KB gzipped

### 2) Lazy Loading

- [ ] Routes chargées dynamiquement.
- [ ] Composants lourds en `lazy()` / `dynamic()`.
- [ ] Images avec `loading="lazy"`.

**Patterns :**

```typescript
// Next.js
const HeavyComponent = dynamic(() => import('./HeavyComponent'), {
  loading: () => <Skeleton />,
});

// React
const HeavyComponent = lazy(() => import('./HeavyComponent'));
```

### 3) Requêtes N+1

- [ ] Identifier les boucles avec requêtes DB/API.
- [ ] Utiliser batch/bulk queries.
- [ ] Implémenter DataLoader si GraphQL.

**Pattern à éviter :**

```typescript
// ❌ N+1
for (const user of users) {
  const posts = await getPosts(user.id);
}

// ✅ Batch
const posts = await getPostsForUsers(users.map((u) => u.id));
```

### 4) Memoization

- [ ] `useMemo` pour calculs coûteux.
- [ ] `useCallback` pour fonctions passées en props.
- [ ] `React.memo` pour composants purs.
- [ ] Cache côté serveur (Redis, in-memory).

### 5) Memory Leaks

- [ ] Event listeners nettoyés dans `useEffect` cleanup.
- [ ] Subscriptions annulées (WebSocket, RxJS).
- [ ] Timers/intervals clearés.
- [ ] Références DOM relâchées.

**Pattern :**

```typescript
useEffect(() => {
  const handler = () => { ... };
  window.addEventListener('resize', handler);
  return () => window.removeEventListener('resize', handler);
}, []);
```

### 6) Images et Assets

- [ ] Format moderne (WebP, AVIF).
- [ ] Dimensions appropriées (pas de 4K pour un thumbnail).
- [ ] CDN pour assets statiques.
- [ ] Compression activée.

### 7) Core Web Vitals

- [ ] **LCP** (Largest Contentful Paint) : < 2.5s
- [ ] **FID** (First Input Delay) : < 100ms
- [ ] **CLS** (Cumulative Layout Shift) : < 0.1

**Commande :**

```bash
npx lighthouse https://example.com --view
```

### 8) Rapport

```
## Résumé Performance
- Bundle size : XXX KB (gzip)
- Lighthouse score : XX/100
- LCP : X.Xs
- FID : Xms
- CLS : X.XX

## Problèmes détectés

### Bundle
- [dépendance] — XXX KB — [action recommandée]

### N+1 Queries
- [fichier:ligne] — [description] — [fix]

### Memory Leaks
- [fichier:ligne] — [type] — [fix]

### Assets
- [fichier] — [taille] — [optimisation]

## Plan d'optimisation
1. [Impact élevé] ...
2. [Impact moyen] ...
3. [Impact faible] ...
```

## Règles

- Mesurer avant et après chaque optimisation.
- Ne pas optimiser prématurément (profiler d'abord).
- Documenter les trade-offs (ex: lazy loading vs UX).
