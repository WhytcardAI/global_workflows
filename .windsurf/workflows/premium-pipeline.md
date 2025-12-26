---
name: premium-pipeline
description: Pipeline qualité premium (triage → plan → impl → vérifs → knowledge)
---

# Premium Pipeline

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

- [ ] Reformuler l’objectif en 1 phrase.
- [ ] Lister les risques (régression, sécurité, perf, compat, UX).
- [ ] Si une info manque : poser 1–3 questions **fermées**.

### 1) Brain + Sources (zéro hallucination)

- [ ] Appeler `@brainConsult` avant toute décision.
- [ ] Si Brain insuffisant : doc officielle via `@context7` ou `@tavily`.
- [ ] Sauvegarder la source utile via `@brainSave` (URL obligatoire si strict mode).

### 2) Cartographie rapide du code

- [ ] Identifier 1–3 points d’entrée (routes, services, composants).
- [ ] Vérifier l’existant avant de créer quoi que ce soit (zéro duplication).
- [ ] Lire les fichiers autoritaires (source-of-truth) avant d’éditer.

### 3) Plan minimal (2–5 items)

- [ ] Créer/mettre à jour une todo list (1 item `in_progress`).
- [ ] Définir les fichiers touchés.
- [ ] Définir comment on valide (commande, test, reproduction).

### 4) Implémentation “safe by default”

- [ ] Changement minimal qui corrige la cause racine.
- [ ] Pas de code mort, pas de TODO “remove later”, pas de duplication.
- [ ] Nommage cohérent.
- [ ] Pas de strings UI hardcodées si i18n en place.

### 5) Quality Gates (obligatoire)

- [ ] Type-check / build (ou équivalent stack).
- [ ] Lint.
- [ ] Tests (unit/integration/e2e selon impact).
- [ ] Vérifier qu’aucune erreur n’est “laissée” : soit fix, soit blocage documenté + plan.

### 6) Vérification produit

- [ ] Reproduire le bug (si debug) puis confirmer fix.
- [ ] Vérifier UI/UX (a11y clavier, états loading/error) si concerné.

### 7) Write-back (obligatoire)

- [ ] Si bug résolu : `@brainBug`.
- [ ] Si code réutilisable : `@brainTemplateSave`.
- [ ] Fin de travail significatif : `@brainSession`.

## Format de sortie attendu

- **Changements** (fichiers)
- **Validation** (commandes / pages / preuves)
- **Risques** (restants)
- **À faire** (si blocage)
