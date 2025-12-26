# Windsurf Premium Workflows

Suite complète de workflows pour garantir une **qualité de code irréprochable**.

## Utilisation

Invoquer un workflow via slash command dans Cascade :

```
/[nom-du-workflow]
```

## Workflows disponibles

### Pipelines (orchestration)

| Workflow                  | Commande                 | Description                                                                                          |
| ------------------------- | ------------------------ | ---------------------------------------------------------------------------------------------------- |
| **Premium Pipeline**      | `/premium-pipeline`      | Pipeline standard pour tout changement (triage → plan → impl → vérifs → knowledge)                   |
| **Full Quality Pipeline** | `/full-quality-pipeline` | Audit complet enchaînant tous les workflows (security → deps → code → tests → perf → i18n → release) |

### Développement

| Workflow                   | Commande                  | Description                                                      |
| -------------------------- | ------------------------- | ---------------------------------------------------------------- |
| **Feature Implementation** | `/feature-implementation` | Implémenter une feature de bout en bout                          |
| **Debug Systematic**       | `/debug-systematic`       | Debugging méthodique (reproduire → isoler → corriger → vérifier) |
| **Refactor Safe**          | `/refactor-safe`          | Refactoring sécurisé avec tests                                  |
| **Code Review**            | `/code-review`            | Review systématique (correctness, security, perf, style)         |

### Audits qualité

| Workflow               | Commande              | Description                                     |
| ---------------------- | --------------------- | ----------------------------------------------- |
| **Code Quality Audit** | `/code-quality-audit` | Code mort, duplication, conventions, types      |
| **Security Audit**     | `/security-audit`     | Vulnérabilités, secrets, injections, auth, CORS |
| **Test Coverage**      | `/test-coverage`      | Couverture et qualité des tests                 |
| **Performance Audit**  | `/performance-audit`  | Bundle size, lazy loading, N+1, memory leaks    |
| **i18n Audit**         | `/i18n-audit`         | Strings hardcodées, traductions, formats        |
| **Dependency Audit**   | `/dependency-audit`   | Packages obsolètes, vulnérables, licences       |

### Outils

| Workflow                  | Commande                 | Description                              |
| ------------------------- | ------------------------ | ---------------------------------------- |
| **MCP Diagnostic**        | `/mcp-diagnostic`        | Status des serveurs MCP, debug connexion |
| **Pre-Release Checklist** | `/pre-release-checklist` | Validation complète avant déploiement    |

## Ordre recommandé pour un audit complet

```
1. /security-audit        → Critique : vulnérabilités, secrets
2. /dependency-audit      → Packages vulnérables/obsolètes
3. /code-quality-audit    → Code mort, duplication
4. /test-coverage         → Couverture des tests
5. /performance-audit     → Performance
6. /i18n-audit            → Internationalisation
7. /pre-release-checklist → Validation finale
```

Ou utiliser `/full-quality-pipeline` pour tout enchaîner automatiquement.

## Critères de blocage

| Catégorie    | Blocker si...                         |
| ------------ | ------------------------------------- |
| Security     | Vulnérabilité critique, secret exposé |
| Dependencies | CVE critique non patchée              |
| Code Quality | Build/type-check échoue               |
| Tests        | Tests échouent, couverture < seuil    |
| Performance  | Lighthouse < 50, bundle > 500KB       |
| i18n         | Strings UI hardcodées                 |

## Intégration avec Brain

Tous les workflows utilisent le système Brain :

- `@brainConsult` — Consulter avant toute décision
- `@brainSave` — Sauvegarder les nouvelles connaissances
- `@brainBug` — Documenter les bugs résolus
- `@brainSession` — Logger les sessions de travail

## Structure des fichiers

```
.windsurf/workflows/
├── README.md                  # Ce fichier
├── premium-pipeline.md        # Pipeline standard
├── full-quality-pipeline.md   # Pipeline complet
├── feature-implementation.md  # Nouvelle feature
├── debug-systematic.md        # Debugging
├── refactor-safe.md           # Refactoring
├── code-review.md             # Code review
├── code-quality-audit.md      # Audit qualité code
├── security-audit.md          # Audit sécurité
├── test-coverage.md           # Audit tests
├── performance-audit.md       # Audit performance
├── i18n-audit.md              # Audit i18n
├── dependency-audit.md        # Audit dépendances
├── mcp-diagnostic.md          # Diagnostic MCP
└── pre-release-checklist.md   # Checklist release
```

## Principes

1. **Zéro code mort** — Supprimer tout code non utilisé
2. **Zéro duplication** — DRY, réutiliser l'existant
3. **Zéro erreur ignorée** — Corriger ou documenter explicitement
4. **Zéro string hardcodée** — i18n pour tout texte UI
5. **Tests obligatoires** — Pas de feature sans test
6. **Documentation continue** — Brain enrichi à chaque session
