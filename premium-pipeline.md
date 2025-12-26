---
description: Pipeline qualité complet pour tout changement de code (triage → plan → impl → debug → refactor → review → vérifs → knowledge)
---

# Premium Pipeline

Pipeline universel pour **tout changement de code** : feature, debug, refactor, config, dépendance.

## Objectif

Appliquer une chaîne de contrôle qualité systématique pour livrer un changement **correct, maintenable, sans duplication, sans code mort**, et avec validation (tests/lint/build) ou un blocage explicitement documenté.

## Quand l’utiliser

- Toute demande qui implique : modification de code, debug, refactor, ajout de feature, changement de config, ajout de dépendance.

## Entrées minimales attendues

- Contexte : repo / stack / commande de build
- Résultat attendu (Definition of Done)
- Contraintes : perf/sécurité/compat/backward/temps

## Pipeline (obligatoire)

### 0) Triage (30–90s)

- [ ] Reformuler l'objectif en 1 phrase
- [ ] Lister les risques (régression, sécurité, perf, compat, UX)
- [ ] Si info manque : poser 1–3 questions **fermées**

### 1) Brain + Sources (zéro hallucination)

- [ ] `@brainConsult` avant toute décision
- [ ] Si insuffisant : `@context7` ou `@tavily` pour doc officielle
- [ ] `@brainSave` avec URL si nouvelle info utile

### 2) Cartographie du code

- [ ] Identifier 1–3 points d'entrée (routes, services, composants)
- [ ] Vérifier l'existant avant de créer (zéro duplication)
- [ ] Lire les fichiers source-of-truth avant d'éditer

### 3) Plan minimal (2–5 items)

- [ ] Todo list avec 1 item `in_progress`
- [ ] Fichiers à créer/modifier
- [ ] Comment valider (commande, test, reproduction)

### 4) Implémentation

#### Si Feature

- [ ] Clarifier le besoin utilisateur et les critères d'acceptation
- [ ] Définir types/interfaces d'abord
- [ ] Implémenter : services → composants → intégration
- [ ] Gérer les états : loading, error, empty
- [ ] i18n : pas de strings hardcodées

#### Si Debug

- [ ] **Reproduire** : étapes exactes, environnement, erreur
- [ ] **Isoler** : fichier/fonction suspect, console.log contextuels
- [ ] **Cause racine** : données? timing? état? logique? dépendance?
- [ ] **Fix upstream** : corriger la cause, pas le symptôme
- [ ] **Vérifier** : bug disparu, pas de régression

#### Si Refactor

- [ ] Tests existants passent AVANT de commencer
- [ ] Ajouter tests si couverture insuffisante
- [ ] Un seul type de changement à la fois (renommage, extraction, déplacement)
- [ ] Commit après chaque étape validée

#### Règles communes

- [ ] Changement minimal qui corrige la cause racine
- [ ] Pas de code mort, pas de TODO "remove later"
- [ ] Nommage cohérent (camelCase, PascalCase, kebab-case)

### 5) Quality Gates

- [ ] Type-check / build passe
- [ ] Lint sans nouveaux warnings
- [ ] Tests passent (unit/integration/e2e selon impact)
- [ ] Aucune erreur ignorée : fix ou blocage documenté

### 6) Code Review (auto-review)

#### Correctness

- [ ] Le code fait ce qu'il doit faire
- [ ] Edge cases gérés (null, undefined, vide, erreur API)

#### Sécurité

- [ ] Pas d'injection (SQL, XSS, command)
- [ ] Pas de secrets hardcodés
- [ ] Inputs validés

#### Performance

- [ ] Pas de N+1 queries
- [ ] Pas de re-renders inutiles
- [ ] Lazy loading si approprié

#### Qualité

- [ ] Code lisible, fonctions courtes
- [ ] Pas de duplication
- [ ] Conventions respectées

### 7) Vérification produit

- [ ] Reproduire le scénario initial → comportement attendu
- [ ] UI/UX : a11y clavier, états loading/error si concerné

### 8) Write-back (obligatoire)

- [ ] Bug résolu → `@brainBug`
- [ ] Code réutilisable → `@brainTemplateSave`
- [ ] Fin de travail → `@brainSession`

## Format de sortie

```
## Changements
- [fichier] — [modification]

## Validation
- [commande/test/preuve]

## Risques résiduels
- [si applicable]

## À faire
- [si blocage]
```
