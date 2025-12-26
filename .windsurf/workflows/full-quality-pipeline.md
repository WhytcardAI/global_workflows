---
name: full-quality-pipeline
description: Pipeline qualité complet - enchaîne tous les audits (code, sécurité, perf, tests, i18n, deps)
---

# Full Quality Pipeline

## Objectif

Exécuter **tous les audits qualité** de manière séquentielle pour garantir un code irréprochable avant release.

## Quand l'utiliser

- Audit complet d'un projet.
- Avant une release majeure.
- Onboarding sur un nouveau codebase.
- Audit périodique (trimestriel).

## Ordre d'exécution

Le pipeline s'exécute dans cet ordre (du plus critique au moins critique) :

```
1. /security-audit        → Vulnérabilités, secrets, injections
2. /dependency-audit      → Packages vulnérables, obsolètes, non utilisés
3. /code-quality-audit    → Code mort, duplication, conventions
4. /test-coverage         → Couverture, qualité des tests
5. /performance-audit     → Bundle, N+1, memory leaks
6. /i18n-audit            → Strings hardcodées, traductions
7. /pre-release-checklist → Validation finale
```

## Instructions d'exécution

### Mode automatique (recommandé)

Exécuter chaque workflow dans l'ordre. À chaque étape :

1. Générer le rapport.
2. Si **blockers critiques** → s'arrêter et corriger.
3. Sinon → passer à l'étape suivante.

### Mode manuel

Appeler chaque workflow individuellement selon le besoin :

- `/security-audit` — Focus sécurité uniquement
- `/code-quality-audit` — Focus qualité code uniquement
- etc.

## Critères de passage

| Étape        | Blocker si...                           |
| ------------ | --------------------------------------- |
| Security     | Vulnérabilité critique ou secret exposé |
| Dependencies | CVE critique non patchée                |
| Code Quality | Erreur de build ou type-check           |
| Tests        | Tests échouent ou couverture < seuil    |
| Performance  | Lighthouse < 50 ou bundle > 500KB       |
| i18n         | Strings hardcodées dans UI              |
| Pre-release  | Tout blocker non résolu                 |

## Rapport consolidé

À la fin du pipeline, générer un rapport consolidé :

```
## Full Quality Pipeline Report
- Projet : [nom]
- Date : YYYY-MM-DD
- Durée : Xh Xmin

## Résumé par étape

| Étape | Status | Blockers | Warnings |
|-------|--------|----------|----------|
| Security | ✅ | 0 | 2 |
| Dependencies | ✅ | 0 | 5 |
| Code Quality | ⚠️ | 0 | 12 |
| Tests | ✅ | 0 | 3 |
| Performance | ✅ | 0 | 1 |
| i18n | ✅ | 0 | 0 |
| Pre-release | ✅ | 0 | 0 |

## Score global
- Blockers : 0
- Warnings : 23
- Status : ✅ READY FOR RELEASE

## Top 5 actions prioritaires
1. [Sécurité] Mettre à jour lodash (CVE-2021-23337)
2. [Code] Supprimer 15 fonctions non utilisées
3. [Tests] Ajouter tests pour auth.ts (0% couverture)
4. [Perf] Lazy load composant Dashboard (-50KB)
5. [Code] Refactorer UserService (complexité 15)

## Détails par étape
[Voir rapports individuels]
```

## Workflows disponibles

| Workflow              | Description                               | Slash command            |
| --------------------- | ----------------------------------------- | ------------------------ |
| Premium Pipeline      | Pipeline standard (triage → impl → vérif) | `/premium-pipeline`      |
| Debug Systematic      | Debugging méthodique                      | `/debug-systematic`      |
| Code Quality Audit    | Code mort, duplication, conventions       | `/code-quality-audit`    |
| Security Audit        | Vulnérabilités, secrets, auth             | `/security-audit`        |
| Test Coverage         | Couverture et qualité des tests           | `/test-coverage`         |
| Performance Audit     | Bundle, lazy loading, N+1                 | `/performance-audit`     |
| i18n Audit            | Internationalisation                      | `/i18n-audit`            |
| MCP Diagnostic        | Status des serveurs MCP                   | `/mcp-diagnostic`        |
| Refactor Safe         | Refactoring sécurisé                      | `/refactor-safe`         |
| Dependency Audit      | Packages et licences                      | `/dependency-audit`      |
| Pre-Release Checklist | Validation finale                         | `/pre-release-checklist` |

## Règles

- **Ne pas skipper** les étapes critiques (security, tests).
- **Documenter** les exceptions avec justification.
- **Corriger les blockers** avant de continuer.
- **Sauvegarder** les rapports pour historique.
