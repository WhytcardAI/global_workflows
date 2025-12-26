---
name: deep-research
description: Recherche approfondie et réflexion structurée sur un sujet complexe
---

# Deep Research Workflow

Workflow pour analyse approfondie avec réflexion structurée et recherches multiples.

## Étapes Obligatoires

### 1. Décomposition du Problème

```
@clear-thought
- Identifier les sous-questions clés
- Définir le périmètre exact
- Lister les inconnues
- Timeout: 60s max
```

### 2. Recherche Documentation Officielle

```
@context7
- Consulter docs officielles des technologies concernées
- Extraire patterns recommandés
- Noter versions et breaking changes
```

### 3. Recherche Best Practices 2025

```
@tavily
- Rechercher état de l'art actuel
- Comparer approches alternatives
- Identifier pièges courants
```

### 4. Recherche Workspace

```
@codebase
- Scanner fichiers existants pertinents
- Identifier patterns déjà utilisés
- Vérifier cohérence avec architecture actuelle
```

### 5. Recherche Complémentaire

```
@microsoftdocs (si TypeScript/Azure)
@perplexity (si besoin sources académiques)
```

### 6. Synthèse Structurée

Format de réponse obligatoire :

```
## Contexte
[Reformulation du problème]

## Analyse
[Points clés identifiés]

## Options
| Option | Avantages | Inconvénients | Effort |
|--------|-----------|---------------|--------|
| A      | ...       | ...           | ...    |
| B      | ...       | ...           | ...    |

## Recommandation
[Choix argumenté avec sources]

## Plan d'Action
1. [Étape 1]
2. [Étape 2]
...

## Sources
- [Doc officielle consultée]
- [Articles/ressources utilisés]
```

## Règles

- Jamais répondre sans avoir consulté minimum 2 sources
- Citer les sources explicitement
- Si information incertaine → le dire clairement
- Proposer alternatives si recommandation risquée
- Estimer effort/complexité de chaque option
