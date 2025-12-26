---
name: debug-systematic
description: Debugging systématique (reproduire → isoler → corriger → vérifier → documenter)
---

# Debug Systématique

## Objectif

Résoudre un bug en suivant une méthodologie rigoureuse : **cause racine**, pas symptômes.

## Quand l'utiliser

- Bug signalé (erreur, comportement inattendu, crash).
- Régression après changement.
- Comportement intermittent à stabiliser.

## Pipeline

### 1) Reproduire (obligatoire)

- [ ] Obtenir les **étapes exactes** pour reproduire.
- [ ] Identifier l'**environnement** (OS, Node, browser, versions).
- [ ] Capturer l'**erreur** (message, stack trace, logs).
- [ ] Si non reproductible : demander plus d'infos, ne pas deviner.

### 2) Isoler

- [ ] Identifier le **fichier/fonction** suspect.
- [ ] Ajouter des `console.log` contextuels : `[Module] var=value`.
- [ ] Utiliser breakpoints si nécessaire.
- [ ] Réduire le scope : commenter/désactiver pour isoler.

### 3) Comprendre la cause racine

- [ ] Pourquoi ce bug existe-t-il ? (pas juste "où").
- [ ] Vérifier si c'est un problème de :
  - Données (null, undefined, type incorrect)
  - Timing (race condition, async)
  - État (state stale, cache)
  - Logique (condition incorrecte)
  - Dépendance (version, breaking change)

### 4) Corriger (changement minimal)

- [ ] Fix **upstream** (cause) plutôt que **downstream** (symptôme).
- [ ] Changement le plus petit possible.
- [ ] Pas de refactor opportuniste dans le même commit.

### 5) Vérifier

- [ ] Reproduire le scénario initial → bug disparu.
- [ ] Vérifier qu'aucune régression n'est introduite.
- [ ] Lancer les tests existants.
- [ ] Si pas de test : en ajouter un pour ce cas.

### 6) Documenter

- [ ] Appeler `@brainBug` avec :
  - `symptom` : description du problème
  - `error` : message d'erreur exact
  - `solution` : fix appliqué
  - `library` : technologie concernée

## Anti-patterns

- ❌ Deviner sans reproduire
- ❌ Corriger les symptômes au lieu de la cause
- ❌ Ajouter du code sans comprendre le problème
- ❌ Supprimer des tests pour "faire passer" le build
- ❌ Laisser des `console.log` de debug en prod

## Format de sortie

```
## Bug
[Description]

## Cause racine
[Explication]

## Fix
[Fichier:ligne] — [changement]

## Vérification
[Commande/étapes pour confirmer]

## Régression
[Tests ajoutés/passés]
```
