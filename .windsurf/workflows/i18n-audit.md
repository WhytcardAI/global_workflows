---
name: i18n-audit
description: Audit internationalisation (strings hardcodées, clés manquantes, pluriels, formats)
---

# i18n Audit

## Objectif

Vérifier que **toutes les chaînes utilisateur** sont internationalisées : pas de hardcoded strings, clés complètes dans toutes les langues, formats localisés.

## Quand l'utiliser

- Avant ajout d'une nouvelle langue.
- Après ajout de fonctionnalités avec UI.
- Audit périodique.
- Avant release internationale.

## Pipeline

### 1) Détecter les strings hardcodées

- [ ] Scanner le code pour les strings littérales dans le JSX/TSX.
- [ ] Exclure : noms techniques, logs, constantes non-UI.

**Patterns à rechercher :**

```bash
# Strings dans JSX (hors className, id, etc.)
grep -rn ">[A-Z][a-z]" --include="*.tsx" --include="*.jsx"

# Attributs texte
grep -rn "placeholder=\"[A-Z]" --include="*.tsx"
grep -rn "title=\"[A-Z]" --include="*.tsx"
grep -rn "alt=\"[A-Z]" --include="*.tsx"
```

### 2) Vérifier la complétude des traductions

- [ ] Lister toutes les clés du fichier source (ex: `en.json`).
- [ ] Comparer avec chaque langue cible.
- [ ] Identifier les clés manquantes.

**Script de vérification :**

```bash
# Comparer les clés entre fichiers JSON
diff <(jq -r 'paths | join(".")' messages/en.json | sort) \
     <(jq -r 'paths | join(".")' messages/fr.json | sort)
```

### 3) Pluralisation

- [ ] Vérifier que les pluriels utilisent le bon format.
- [ ] Tester avec 0, 1, 2, 5, 21 (règles varient selon langue).

**Format ICU :**

```json
{
  "items": "{count, plural, =0 {No items} one {# item} other {# items}}"
}
```

### 4) Formats localisés

- [ ] **Dates** : utiliser `Intl.DateTimeFormat` ou lib i18n.
- [ ] **Nombres** : `Intl.NumberFormat` (séparateurs, décimales).
- [ ] **Devises** : format selon locale.
- [ ] **Unités** : système métrique vs impérial.

**Patterns corrects :**

```typescript
// ✅ Correct
new Intl.DateTimeFormat(locale).format(date);
new Intl.NumberFormat(locale, { style: "currency", currency: "EUR" }).format(
  amount,
);

// ❌ Incorrect
date.toLocaleDateString() // Utilise la locale système, pas celle de l'app
`${amount} €`; // Format hardcodé
```

### 5) Direction du texte (RTL)

Si langues RTL supportées (arabe, hébreu) :

- [ ] CSS avec `dir="rtl"` ou logical properties.
- [ ] Icônes directionnelles inversées.
- [ ] Layout mirroré.

### 6) Accessibilité i18n

- [ ] `lang` attribute sur `<html>`.
- [ ] `aria-label` traduits.
- [ ] Screen reader : texte alternatif traduit.

### 7) Rapport

```
## Résumé i18n
- Langues supportées : [en, fr, de, ...]
- Clés totales : N
- Complétude par langue :
  - en : 100%
  - fr : 95% (N manquantes)
  - de : 90% (N manquantes)

## Strings hardcodées détectées
| Fichier | Ligne | String | Action |
|---------|-------|--------|--------|
| Button.tsx | 42 | "Submit" | Extraire vers i18n |

## Clés manquantes par langue
### fr
- HomePage.newFeature.title
- Settings.privacy.description

### de
- HomePage.newFeature.title
- HomePage.newFeature.description

## Formats non localisés
| Fichier | Ligne | Type | Problème |
|---------|-------|------|----------|
| Invoice.tsx | 15 | Date | toLocaleDateString() sans locale |

## Actions
1. [Critique] Extraire N strings hardcodées
2. [Haute] Compléter N clés manquantes
3. [Moyenne] Corriger N formats
```

## Règles

- **Zéro string UI hardcodée** dans le code.
- Clé i18n = namespace.section.key (ex: `HomePage.hero.title`).
- Toute nouvelle feature = clés i18n dans toutes les langues.
- Utiliser des placeholders pour les valeurs dynamiques : `{name}`, `{count}`.
