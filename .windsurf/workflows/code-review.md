---
name: code-review
description: Code review systématique (correctness, security, perf, style, tests)
---

# Code Review

## Objectif

Effectuer une **code review rigoureuse** : vérifier la correctness, la sécurité, la performance, le style et les tests.

## Quand l'utiliser

- Review d'une PR/MR.
- Review de son propre code avant commit.
- Audit de code existant.

## Pipeline

### 1) Contexte

- [ ] Comprendre l'**objectif** du changement.
- [ ] Lire la **description** de la PR.
- [ ] Identifier le **scope** (fichiers, fonctions).

### 2) Correctness (le plus important)

- [ ] Le code fait-il ce qu'il est censé faire ?
- [ ] Les edge cases sont-ils gérés ?
- [ ] Les erreurs sont-elles correctement gérées ?
- [ ] La logique est-elle correcte ?

**Questions :**

- Que se passe-t-il si l'input est null/undefined ?
- Que se passe-t-il si l'API échoue ?
- Que se passe-t-il avec des données vides ?

### 3) Sécurité

- [ ] Pas d'injection (SQL, XSS, command).
- [ ] Pas de secrets hardcodés.
- [ ] Validation des inputs utilisateur.
- [ ] Autorisation vérifiée côté serveur.
- [ ] Données sensibles protégées.

### 4) Performance

- [ ] Pas de requêtes N+1.
- [ ] Pas de re-renders inutiles (React).
- [ ] Pas de calculs coûteux non mémoïsés.
- [ ] Pas de memory leaks.
- [ ] Lazy loading si approprié.

### 5) Lisibilité et maintenabilité

- [ ] Nommage clair et cohérent.
- [ ] Fonctions courtes et focalisées.
- [ ] Pas de code dupliqué.
- [ ] Pas de code mort.
- [ ] Commentaires utiles (pas évidents).
- [ ] Structure logique.

### 6) Style et conventions

- [ ] Respect des conventions du projet.
- [ ] Formatage cohérent.
- [ ] Imports organisés.
- [ ] Types explicites (TypeScript).
- [ ] Pas de `any`.

### 7) Tests

- [ ] Tests présents pour le nouveau code.
- [ ] Tests couvrent les cas importants.
- [ ] Tests lisibles et maintenables.
- [ ] Pas de tests fragiles.

### 8) Documentation

- [ ] Code auto-documenté (nommage clair).
- [ ] JSDoc pour les fonctions publiques complexes.
- [ ] README mis à jour si nécessaire.
- [ ] Changelog mis à jour.

## Feedback constructif

**Format recommandé :**

```
[NIVEAU] fichier:ligne — commentaire

Niveaux :
- [BLOCKER] — Doit être corrigé avant merge
- [SUGGESTION] — Amélioration recommandée
- [QUESTION] — Besoin de clarification
- [NITPICK] — Détail mineur, optionnel
- [PRAISE] — Bon travail à souligner
```

**Exemples :**

```
[BLOCKER] auth.ts:42 — Input utilisateur non validé, risque d'injection.

[SUGGESTION] UserService.ts:15 — Cette logique pourrait être extraite
dans une fonction utilitaire pour réutilisation.

[QUESTION] api/route.ts:30 — Pourquoi ce timeout de 5s ?
Est-ce documenté quelque part ?

[NITPICK] Button.tsx:8 — Préférer `const` à `let` ici.

[PRAISE] hooks/useAuth.ts — Excellente gestion des edge cases !
```

## Checklist rapide

```
## Review : [PR/fichier]

### Correctness
- [ ] Logique correcte
- [ ] Edge cases gérés
- [ ] Erreurs gérées

### Security
- [ ] Pas d'injection
- [ ] Pas de secrets
- [ ] Inputs validés

### Performance
- [ ] Pas de N+1
- [ ] Pas de memory leak
- [ ] Memoization appropriée

### Quality
- [ ] Code lisible
- [ ] Pas de duplication
- [ ] Conventions respectées

### Tests
- [ ] Tests présents
- [ ] Cas importants couverts

### Verdict
- [ ] ✅ Approve
- [ ] 🔄 Request changes
- [ ] 💬 Comment
```

## Anti-patterns de review

- ❌ Critiquer sans proposer de solution.
- ❌ Se concentrer uniquement sur le style.
- ❌ Ignorer les problèmes de sécurité.
- ❌ Approuver sans vraiment lire.
- ❌ Bloquer pour des nitpicks.

## Règles

- **Correctness > Security > Performance > Style**.
- **Proposer des solutions**, pas juste des critiques.
- **Être constructif** et respectueux.
- **Bloquer uniquement** pour les vrais problèmes.
