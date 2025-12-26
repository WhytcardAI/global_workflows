---
name: pre-release-checklist
description: Checklist complète avant release (qualité, sécurité, perf, tests, docs)
---

# Pre-Release Checklist

## Objectif

Vérifier **tous les critères de qualité** avant de déployer en production : code, tests, sécurité, performance, documentation.

## Quand l'utiliser

- Avant chaque release en production.
- Avant merge d'une branche majeure.
- Avant livraison client.

## Checklist complète

### 1) Code Quality

- [ ] Build réussit sans erreur.
- [ ] Lint passe sans warning non justifié.
- [ ] Type-check passe (TypeScript).
- [ ] Pas de `console.log` de debug.
- [ ] Pas de code commenté.
- [ ] Pas de TODO critiques non résolus.
- [ ] Pas de fichiers temporaires ou de test.

```bash
npm run build
npm run lint
npm run type-check
grep -rn "console.log\|console.debug" --include="*.ts" --include="*.tsx" src/
```

### 2) Tests

- [ ] Tous les tests passent.
- [ ] Couverture ≥ seuil défini (ex: 80%).
- [ ] Tests e2e sur parcours critiques.
- [ ] Pas de tests skippés sans raison.

```bash
npm test
npm run test:e2e
npm run test:coverage
```

### 3) Sécurité

- [ ] `npm audit` sans vulnérabilité critique/haute.
- [ ] Pas de secrets dans le code.
- [ ] Variables d'environnement documentées.
- [ ] CORS configuré correctement.
- [ ] Headers de sécurité en place.

```bash
npm audit
git secrets --scan
```

### 4) Performance

- [ ] Bundle size acceptable.
- [ ] Lighthouse score ≥ 90.
- [ ] Pas de requêtes N+1.
- [ ] Images optimisées.
- [ ] Lazy loading en place.

### 5) Internationalisation

- [ ] Toutes les clés i18n présentes dans toutes les langues.
- [ ] Pas de strings hardcodées.
- [ ] Formats de date/nombre localisés.

### 6) Accessibilité

- [ ] Navigation clavier fonctionnelle.
- [ ] Contraste suffisant.
- [ ] Alt text sur les images.
- [ ] ARIA labels appropriés.
- [ ] Screen reader testé.

### 7) Documentation

- [ ] README à jour.
- [ ] CHANGELOG mis à jour.
- [ ] Variables d'environnement documentées.
- [ ] API documentée (si applicable).
- [ ] Procédure de déploiement documentée.

### 8) Configuration

- [ ] Variables d'environnement de prod définies.
- [ ] URLs de prod configurées.
- [ ] Analytics/monitoring configuré.
- [ ] Error tracking configuré (Sentry, etc.).

### 9) Base de données (si applicable)

- [ ] Migrations à jour.
- [ ] Backup récent.
- [ ] Rollback plan défini.

### 10) Déploiement

- [ ] Branche à jour avec main/develop.
- [ ] Conflicts résolus.
- [ ] PR reviewée et approuvée.
- [ ] CI/CD vert.

## Rapport

```
## Pre-Release Report
- Version : X.Y.Z
- Date : YYYY-MM-DD
- Branche : feature/xxx → main

## Checklist Status
| Catégorie | Status | Notes |
|-----------|--------|-------|
| Code Quality | ✅ | |
| Tests | ✅ | Coverage 85% |
| Sécurité | ✅ | 0 vulnérabilités |
| Performance | ⚠️ | Bundle +5% |
| i18n | ✅ | |
| a11y | ✅ | |
| Documentation | ✅ | |
| Configuration | ✅ | |
| Database | N/A | |
| Déploiement | ✅ | |

## Blockers
- [Si applicable]

## Risques identifiés
- [Si applicable]

## Rollback plan
1. [Étape 1]
2. [Étape 2]

## Approbation
- [ ] Tech Lead
- [ ] QA
- [ ] Product Owner
```

## Règles

- **Aucun blocker** = release autorisée.
- **Warning** = documenter et accepter le risque.
- **Blocker** = résoudre avant release.
- Garder une trace de chaque release (changelog, rapport).
