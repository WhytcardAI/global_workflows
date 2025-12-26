---
name: dependency-audit
description: Audit des dépendances (obsolètes, vulnérables, non utilisées, licences)
---

# Dependency Audit

## Objectif

Auditer les **dépendances du projet** : versions obsolètes, vulnérabilités, packages non utilisés, compatibilité des licences.

## Quand l'utiliser

- Avant une release majeure.
- Alerte de sécurité (Dependabot, npm audit).
- Audit périodique (mensuel).
- Avant mise à jour majeure de framework.

## Pipeline

### 1) Inventaire des dépendances

- [ ] Lister toutes les dépendances (prod + dev).
- [ ] Identifier les versions actuelles vs disponibles.

**Commandes :**

```bash
# Node.js
npm outdated
pnpm outdated

# Python
pip list --outdated
```

### 2) Dépendances non utilisées

- [ ] Scanner le code pour les imports réels.
- [ ] Identifier les packages installés mais jamais importés.

**Commandes :**

```bash
# Node.js
npx depcheck

# Résultat exemple :
# Unused dependencies: lodash, moment
# Missing dependencies: uuid
```

### 3) Vulnérabilités

- [ ] Scanner les CVE connues.
- [ ] Prioriser par sévérité.

**Commandes :**

```bash
# Node.js
npm audit
pnpm audit

# Avec fix automatique (prudence)
npm audit fix

# Python
pip-audit
safety check
```

**Sévérités :**
| Niveau | Action |
|--------|--------|
| Critical | Bloquer, fixer immédiatement |
| High | Fixer cette semaine |
| Moderate | Planifier |
| Low | Évaluer |

### 4) Compatibilité des versions

- [ ] Vérifier les peer dependencies.
- [ ] Identifier les conflits de versions.
- [ ] Vérifier la compatibilité Node.js/Python.

```bash
# Vérifier les peer deps
npm ls

# Voir les conflits
npm explain [package]
```

### 5) Licences

- [ ] Lister les licences de toutes les dépendances.
- [ ] Vérifier la compatibilité avec le projet.

**Commandes :**

```bash
# Node.js
npx license-checker --summary
npx license-checker --onlyAllow 'MIT;Apache-2.0;BSD-2-Clause;BSD-3-Clause;ISC'
```

**Licences à surveiller :**
| Licence | Risque | Note |
|---------|--------|------|
| MIT, Apache-2.0, BSD | ✅ Faible | Permissives |
| GPL, AGPL | ⚠️ Élevé | Copyleft, peut contaminer |
| Proprietary | ❌ Bloquant | Vérifier les termes |

### 6) Plan de mise à jour

**Stratégie recommandée :**

1. Mettre à jour les patches (x.x.PATCH) → faible risque.
2. Mettre à jour les mineurs (x.MINOR.x) → tester.
3. Mettre à jour les majeurs (MAJOR.x.x) → planifier, lire changelog.

```bash
# Mise à jour interactive
npx npm-check -u
```

### 7) Rapport

```
## Résumé Dépendances
- Total : N packages
- Prod : N
- Dev : N
- Obsolètes : N
- Vulnérables : N
- Non utilisées : N

## Vulnérabilités
| Package | Version | Sévérité | CVE | Fix |
|---------|---------|----------|-----|-----|
| lodash | 4.17.20 | High | CVE-2021-23337 | 4.17.21 |

## Non utilisées
- lodash (installé mais jamais importé)
- moment (remplacé par date-fns)

## Obsolètes (majeures)
| Package | Actuel | Disponible | Breaking changes |
|---------|--------|------------|------------------|
| next | 14.x | 15.x | Oui, voir changelog |

## Licences
- MIT : N packages
- Apache-2.0 : N packages
- ⚠️ GPL : N packages (vérifier)

## Plan d'action
1. [Immédiat] Fixer vulnérabilités critiques
2. [Cette semaine] Supprimer packages non utilisés
3. [Ce mois] Mettre à jour packages obsolètes
```

## Règles

- **Vulnérabilités critiques** = bloquer le déploiement.
- **Supprimer** les dépendances non utilisées.
- **Documenter** les raisons de garder une version ancienne.
- **Tester** après chaque mise à jour.
