---
name: test-coverage
description: Audit et amélioration de la couverture de tests (unit/integration/e2e)
---

# Test Coverage

## Objectif

Évaluer et améliorer la **couverture de tests** : identifier les zones non testées, ajouter des tests pertinents, maintenir la qualité des tests existants.

## Quand l'utiliser

- Avant une release majeure.
- Après ajout de fonctionnalités critiques.
- Quand la couverture descend sous le seuil (ex: 80%).
- Audit périodique.

## Pipeline

### 1) État actuel

- [ ] Lancer les tests existants.
- [ ] Générer le rapport de couverture.
- [ ] Identifier le % global et par fichier.

**Commandes :**

```bash
# Jest
npx jest --coverage

# Vitest
npx vitest run --coverage

# NYC (Istanbul)
npx nyc npm test
```

### 2) Analyse des gaps

- [ ] Fichiers avec 0% de couverture.
- [ ] Fonctions critiques non testées.
- [ ] Branches (if/else) non couvertes.
- [ ] Edge cases manquants.

**Prioriser :**

1. Code critique (auth, paiement, données sensibles)
2. Code fréquemment modifié
3. Code avec bugs historiques

### 3) Qualité des tests existants

- [ ] Tests sans assertions (faux positifs).
- [ ] Tests fragiles (dépendant de l'ordre, du temps, de l'état global).
- [ ] Tests trop couplés à l'implémentation.
- [ ] Mocking excessif.

**Patterns à éviter :**

```typescript
// ❌ Test sans assertion
it("should work", () => {
  doSomething();
});

// ❌ Test fragile (timing)
it("should complete", async () => {
  await sleep(1000);
  expect(result).toBe(true);
});
```

### 4) Types de tests

| Type        | Scope           | Vitesse | Quand              |
| ----------- | --------------- | ------- | ------------------ |
| Unit        | Fonction/classe | Rapide  | Logique métier     |
| Integration | Module/API      | Moyen   | Interactions       |
| E2E         | User flow       | Lent    | Parcours critiques |

**Ratio recommandé :** 70% unit, 20% integration, 10% e2e.

### 5) Ajouter des tests

Pour chaque fonction non testée :

```typescript
describe('functionName', () => {
  it('should handle normal case', () => {
    // Arrange
    const input = ...;
    // Act
    const result = functionName(input);
    // Assert
    expect(result).toBe(expected);
  });

  it('should handle edge case', () => {
    // ...
  });

  it('should throw on invalid input', () => {
    expect(() => functionName(null)).toThrow();
  });
});
```

### 6) Rapport

```
## Couverture actuelle
- Global : XX%
- Statements : XX%
- Branches : XX%
- Functions : XX%
- Lines : XX%

## Fichiers critiques non couverts
| Fichier | Couverture | Priorité |
|---------|------------|----------|
| auth.ts | 0% | CRITIQUE |
| payment.ts | 30% | HAUTE |

## Tests ajoutés
- [fichier.test.ts] — N tests — couvre [fonctions]

## Tests à améliorer
- [test] — [problème] — [recommandation]

## Prochaines étapes
1. [Priorité] [fichier] — [type de test]
```

## Règles

- Ne jamais supprimer un test sans raison documentée.
- Un bug fixé = un test de régression ajouté.
- Tests doivent être déterministes (pas de random, pas de dates réelles).
- Mocker les dépendances externes (API, DB, filesystem).
