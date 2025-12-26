---
name: code-quality-audit
description: Audit qualité code (dead code, duplication, conventions, types, erreurs)
---

# Code Quality Audit

## Objectif

Scanner le code pour détecter : **code mort**, **duplication**, **violations de conventions**, **erreurs de typage**, **mauvaises pratiques**.

## Quand l'utiliser

- Avant une release / merge important.
- Après un gros refactor.
- Audit périodique (hebdomadaire/mensuel).
- Onboarding sur un nouveau projet.

## Pipeline

### 1) Inventaire rapide

- [ ] Identifier le stack (package.json, tsconfig, etc.).
- [ ] Lister les outils de lint/format déjà configurés.
- [ ] Identifier les dossiers à scanner (exclure node_modules, dist, .next).

### 2) Code mort

- [ ] Fonctions/méthodes jamais appelées.
- [ ] Variables déclarées mais non utilisées.
- [ ] Imports non utilisés.
- [ ] Fichiers orphelins (non importés nulle part).
- [ ] Routes/endpoints non utilisés.
- [ ] Composants UI non rendus.

**Commandes suggérées :**

```bash
# TypeScript/JavaScript
npx depcheck
npx ts-prune
npx eslint . --ext .ts,.tsx --rule 'no-unused-vars: error'
```

### 3) Duplication

- [ ] Blocs de code identiques ou quasi-identiques.
- [ ] Fonctions avec logique similaire.
- [ ] Constantes/configurations dupliquées.

**Commande suggérée :**

```bash
npx jscpd . --min-lines 5 --min-tokens 50
```

### 4) Conventions de nommage

- [ ] Variables/fonctions : camelCase
- [ ] Classes/Types/Interfaces : PascalCase
- [ ] Constantes : SCREAMING_SNAKE_CASE
- [ ] Fichiers : kebab-case

### 5) Typage (TypeScript)

- [ ] Pas de `any` explicite.
- [ ] Pas de `@ts-ignore` sans justification.
- [ ] Types explicites sur les fonctions publiques.

**Commande :**

```bash
npx tsc --noEmit
```

### 6) Erreurs et warnings

- [ ] Lancer le linter complet.
- [ ] Zéro warning ignoré sans raison documentée.

**Commande :**

```bash
npx eslint . --ext .ts,.tsx,.js,.jsx
```

### 7) Complexité

- [ ] Fonctions > 50 lignes → signaler.
- [ ] Complexité cyclomatique > 10 → signaler.
- [ ] Imbrication > 4 niveaux → signaler.

### 8) Rapport

Format de sortie :

```
## Résumé
- Fichiers scannés : N
- Code mort détecté : N items
- Duplications : N blocs
- Violations conventions : N
- Erreurs typage : N
- Warnings lint : N

## Détails par catégorie
### Code mort
- [fichier:ligne] — [description]

### Duplication
- [fichier1] ↔ [fichier2] — [lignes]

### Conventions
- [fichier:ligne] — [violation]

### Typage
- [fichier:ligne] — [erreur]

## Actions recommandées
1. [Priorité haute] ...
2. [Priorité moyenne] ...
3. [Priorité basse] ...
```

## Règles

- Ne pas corriger automatiquement sans validation.
- Prioriser : sécurité > bugs > code mort > conventions.
- Documenter les exceptions justifiées.
