---
name: feature-implementation
description: Implémentation de feature complète (spec → design → impl → tests → review)
---

# Feature Implementation

## Objectif

Implémenter une **nouvelle fonctionnalité** de bout en bout avec qualité production : spécification, design, implémentation, tests, documentation.

## Quand l'utiliser

- Nouvelle feature demandée.
- User story à implémenter.
- Amélioration significative.

## Pipeline

### 1) Clarification (obligatoire)

- [ ] Comprendre le **besoin utilisateur** (pas juste la demande technique).
- [ ] Définir les **critères d'acceptation** (Definition of Done).
- [ ] Identifier les **edge cases**.
- [ ] Lister les **dépendances** (autres features, APIs, data).

**Questions à poser :**

- Qui utilise cette feature ?
- Quel problème résout-elle ?
- Comment saura-t-on qu'elle fonctionne ?
- Quels sont les cas limites ?

### 2) Recherche (Brain + docs)

- [ ] `@brainConsult` — Vérifier si pattern similaire existe.
- [ ] `@context7` — Doc officielle du framework/lib.
- [ ] `@tavily` — Best practices 2025.
- [ ] Scanner le codebase pour patterns existants.

### 3) Design technique

- [ ] Identifier les fichiers à créer/modifier.
- [ ] Définir les interfaces/types.
- [ ] Choisir les patterns (hooks, services, etc.).
- [ ] Estimer la complexité.

**Template de design :**

```
## Feature : [nom]

### Fichiers
- Créer : [liste]
- Modifier : [liste]

### Types/Interfaces
[définitions]

### Dépendances
- Packages : [liste]
- APIs : [liste]

### Risques
- [liste]
```

### 4) Implémentation

**Ordre recommandé :**

1. Types/interfaces
2. Services/logique métier
3. Composants UI
4. Intégration
5. Styles

**Règles :**

- [ ] Pas de strings UI hardcodées → i18n.
- [ ] Pas de `any` → types explicites.
- [ ] Pas de duplication → réutiliser l'existant.
- [ ] Gestion des erreurs → try/catch, error boundaries.
- [ ] États loading/error/empty → UX complète.

### 5) Tests

- [ ] Tests unitaires pour la logique.
- [ ] Tests d'intégration pour les interactions.
- [ ] Tests e2e pour le parcours utilisateur (si critique).

**Couverture minimale :**

- Cas nominal (happy path)
- Cas d'erreur
- Edge cases identifiés

### 6) Validation

- [ ] Build réussit.
- [ ] Lint passe.
- [ ] Tests passent.
- [ ] Review visuelle (si UI).
- [ ] Critères d'acceptation validés.

### 7) Documentation

- [ ] Ajouter clés i18n dans toutes les langues.
- [ ] Mettre à jour STRUCTURE.md si nouveau composant.
- [ ] Documenter l'API si applicable.
- [ ] `@brainSave` si nouveau pattern utile.

### 8) Commit

Format : `feat(scope): description`

```bash
git add .
git commit -m "feat(auth): add password reset flow"
```

## Rapport

```
## Feature : [nom]

### Résumé
[Description courte]

### Fichiers modifiés
- [fichier] — [changement]

### Tests ajoutés
- [test] — [couverture]

### i18n
- Clés ajoutées : N
- Langues : [liste]

### Validation
- [ ] Build ✅
- [ ] Lint ✅
- [ ] Tests ✅
- [ ] Review ✅

### Critères d'acceptation
- [x] [critère 1]
- [x] [critère 2]
```

## Anti-patterns

- ❌ Coder sans comprendre le besoin.
- ❌ Ignorer les edge cases.
- ❌ Oublier les états loading/error.
- ❌ Hardcoder des strings.
- ❌ Skipper les tests.
- ❌ Commit sans validation.

## Règles

- **Clarifier avant de coder**.
- **Tester ce qu'on code**.
- **Documenter ce qu'on crée**.
- **Réutiliser avant de créer**.
