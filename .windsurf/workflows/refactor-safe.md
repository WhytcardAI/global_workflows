---
name: refactor-safe
description: Refactoring sécurisé (tests d'abord, petits pas, validation continue)
---

# Refactor Safe

## Objectif

Effectuer un **refactoring sans régression** : tests en place, changements incrémentaux, validation à chaque étape.

## Quand l'utiliser

- Améliorer la structure du code existant.
- Réduire la dette technique.
- Préparer une évolution majeure.
- Après un audit qualité.

## Pipeline

### 1) Définir le scope

- [ ] Identifier précisément ce qui doit être refactoré.
- [ ] Définir le résultat attendu (structure, nommage, patterns).
- [ ] Estimer l'impact (fichiers, fonctions, tests).

**Questions clés :**

- Pourquoi ce refactor ?
- Quel est le bénéfice mesurable ?
- Quels sont les risques ?

### 2) Vérifier la couverture de tests

- [ ] Lancer les tests existants → doivent passer.
- [ ] Identifier les zones non couvertes.
- [ ] **Ajouter des tests AVANT de refactorer** si couverture insuffisante.

```bash
# Vérifier la couverture
npx jest --coverage --collectCoverageFrom='src/path/to/refactor/**'
```

### 3) Créer un point de restauration

- [ ] Commit propre avant de commencer.
- [ ] Ou créer une branche dédiée.

```bash
git checkout -b refactor/[scope]
```

### 4) Refactorer par petits pas

**Règle : un seul type de changement à la fois.**

| Étape | Type           | Exemple                      |
| ----- | -------------- | ---------------------------- |
| 1     | Renommage      | Variable, fonction, fichier  |
| 2     | Extraction     | Fonction, composant, module  |
| 3     | Déplacement    | Fichier vers nouveau dossier |
| 4     | Simplification | Réduire complexité           |
| 5     | Abstraction    | Créer interface/type commun  |

**Après chaque étape :**

- [ ] Tests passent.
- [ ] Build réussit.
- [ ] Commit intermédiaire.

### 5) Patterns de refactoring

**Extract Function :**

```typescript
// Avant
function process() {
  // 50 lignes de code
}

// Après
function process() {
  validateInput();
  transformData();
  saveResult();
}
```

**Replace Conditional with Polymorphism :**

```typescript
// Avant
if (type === 'A') { ... }
else if (type === 'B') { ... }

// Après
const handlers = { A: handleA, B: handleB };
handlers[type]();
```

**Introduce Parameter Object :**

```typescript
// Avant
function create(name, email, age, country) { ... }

// Après
function create(user: UserInput) { ... }
```

### 6) Validation finale

- [ ] Tous les tests passent.
- [ ] Build réussit.
- [ ] Lint sans nouveaux warnings.
- [ ] Review du diff complet.
- [ ] Pas de code mort introduit.
- [ ] Pas de duplication introduite.

### 7) Rapport

```
## Refactoring effectué
- Scope : [description]
- Fichiers modifiés : N
- Lignes ajoutées : +N
- Lignes supprimées : -N

## Changements
1. [Type] [Description]
2. [Type] [Description]

## Tests
- Existants : N (tous passent)
- Ajoutés : N

## Bénéfices
- [Métrique avant] → [Métrique après]
- Complexité réduite de X à Y
- Duplication éliminée : N lignes

## Risques résiduels
- [Si applicable]
```

## Anti-patterns

- ❌ Refactorer sans tests.
- ❌ Gros changements en un seul commit.
- ❌ Mélanger refactor et nouvelles features.
- ❌ Refactorer du code qu'on ne comprend pas.
- ❌ Refactorer "au cas où".

## Règles

- **Tests d'abord** : pas de refactor sans filet de sécurité.
- **Petits pas** : chaque commit doit être fonctionnel.
- **Pas de feature** : refactor ≠ nouvelle fonctionnalité.
- **Mesurer** : prouver le bénéfice avec des métriques.
