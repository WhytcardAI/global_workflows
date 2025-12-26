---
description: Workflow pour consulter et enrichir le Brain local avant toute réponse
---

# Brain Workflow

Workflow obligatoire pour exploiter le système de connaissance local (Brain).

## Étapes Obligatoires

### 1. Consultation Brain

```
@brainConsult
- Rechercher dans la base de connaissances locale
- Identifier informations pertinentes existantes
- Noter les lacunes éventuelles
```

### 2. Vérification Complétude

**Si Brain contient l'info :**

- Utiliser directement la source locale
- Citer : "Basé sur [Brain local]..."

**Si Brain incomplet/absent :**

- Passer à l'étape 3

### 3. Recherche Documentation Officielle

```
@context7
- Consulter docs officielles
- Extraire informations vérifiées
- Préparer sauvegarde Brain
```

### 4. Sauvegarde Brain

```
@brainSave
- Stocker nouvelle info utile
- Catégoriser correctement
- Inclure source et date
```

### 5. Gestion Bugs/Erreurs

```
@brainBug
- Si résolution d'un bug → sauvegarder solution
- Inclure : erreur, cause, fix, contexte
```

### 6. Templates Réutilisables

```
@brainTemplateSave
- Code générique réutilisable → sauvegarder
- Documenter usage et paramètres
```

### 7. Log Session (fin de travail significatif)

```
@brainSession
- Résumer travail effectué
- Lister décisions prises
- Noter apprentissages
```

## Règles

- **Jamais deviner** → consulter Brain ou doc officielle
- **Toujours citer source** : "Basé sur [Brain/Doc officielle]..."
- **Enrichir continuellement** : chaque nouvelle info = sauvegarde potentielle
- **Zéro hallucination** : si incertain → le dire clairement
- **Preuves factuelles** : pas d'affirmation sans source vérifiable
