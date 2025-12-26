---
description: Audit qualité complet (code mort, duplication, sécurité, performance, tests, i18n, dépendances, pre-release)
---

# Full Audit

Audit complet d'un projet couvrant **tous les aspects qualité** en une seule passe.

---

## 1) Sécurité (CRITIQUE)

### Secrets exposés

- [ ] Rechercher API keys, passwords, tokens dans le code
- [ ] Vérifier `.env` non commité, `.gitignore` correct

```bash
git secrets --scan
gitleaks detect --source .
grep -rn "API_KEY\|SECRET\|PASSWORD\|TOKEN" --include="*.ts" --include="*.js"
```

### Vulnérabilités dépendances

- [ ] Scanner les CVE connues

```bash
npm audit
pnpm audit
```

### Injections

- [ ] SQL Injection : requêtes paramétrées
- [ ] XSS : échapper inputs utilisateur
- [ ] Command Injection : pas de `exec()` avec input user

```bash
grep -rn "eval\|exec\|innerHTML\|dangerouslySetInnerHTML" --include="*.ts" --include="*.tsx"
```

### Auth & Config

- [ ] JWT : expiration et signature vérifiées
- [ ] Mots de passe hashés (bcrypt/argon2)
- [ ] CORS : pas de `*` en prod
- [ ] Headers sécurité : CSP, X-Frame-Options, HSTS

---

## 2) Dépendances

- [ ] Lister dépendances obsolètes

```bash
npm outdated
```

- [ ] Identifier packages non utilisés

```bash
npx depcheck
```

- [ ] Vérifier licences compatibles

```bash
npx license-checker --summary
```

**Sévérités** : Critical → bloquer | High → cette semaine | Moderate → planifier

---

## 3) Code Quality

### Code mort

- [ ] Fonctions/variables non utilisées
- [ ] Imports non utilisés
- [ ] Fichiers orphelins

```bash
npx ts-prune
npx eslint . --ext .ts,.tsx --rule 'no-unused-vars: error'
```

### Duplication

- [ ] Blocs de code identiques

```bash
npx jscpd . --min-lines 5 --min-tokens 50
```

### Typage

- [ ] Pas de `any` explicite
- [ ] Pas de `@ts-ignore` sans justification

```bash
npx tsc --noEmit
```

### Conventions

- [ ] Variables/fonctions : camelCase
- [ ] Classes/Types : PascalCase
- [ ] Constantes : SCREAMING_SNAKE_CASE
- [ ] Fichiers : kebab-case

### Lint

```bash
npx eslint . --ext .ts,.tsx,.js,.jsx
```

---

## 4) Tests

- [ ] Tous les tests passent
- [ ] Couverture ≥ seuil (ex: 80%)

```bash
npx jest --coverage
npx vitest run --coverage
```

### Gaps prioritaires

1. Code critique (auth, paiement) avec 0% couverture
2. Code fréquemment modifié
3. Code avec bugs historiques

### Qualité des tests

- [ ] Pas de tests sans assertions
- [ ] Pas de tests fragiles (timing, ordre)
- [ ] Mocking raisonnable

---

## 5) Performance

### Bundle size

```bash
npx @next/bundle-analyzer
```

**Seuils** : Initial JS < 200KB gzip, Largest chunk < 100KB gzip

### Lazy loading

- [ ] Routes dynamiques
- [ ] Composants lourds en `lazy()` / `dynamic()`
- [ ] Images avec `loading="lazy"`

### N+1 Queries

- [ ] Pas de boucles avec requêtes DB/API

### Memory leaks

- [ ] Event listeners nettoyés dans cleanup
- [ ] Subscriptions annulées
- [ ] Timers/intervals clearés

### Core Web Vitals

```bash
npx lighthouse https://example.com --view
```

- LCP < 2.5s | FID < 100ms | CLS < 0.1

---

## 6) Internationalisation

### Strings hardcodées

```bash
grep -rn ">[A-Z][a-z]" --include="*.tsx"
grep -rn "placeholder=\"[A-Z]" --include="*.tsx"
```

### Traductions complètes

```bash
diff <(jq -r 'paths | join(".")' messages/en.json | sort) \
     <(jq -r 'paths | join(".")' messages/fr.json | sort)
```

### Formats localisés

- [ ] Dates : `Intl.DateTimeFormat`
- [ ] Nombres : `Intl.NumberFormat`
- [ ] Devises : format selon locale

---

## 7) Pre-Release Checklist

### Code

- [ ] Build réussit
- [ ] Lint passe
- [ ] Type-check passe
- [ ] Pas de console.log de debug
- [ ] Pas de code commenté

### Tests

- [ ] Tous passent
- [ ] Couverture ≥ seuil
- [ ] Pas de tests skippés

### Sécurité

- [ ] npm audit sans critique/haute
- [ ] Pas de secrets dans le code

### Documentation

- [ ] README à jour
- [ ] CHANGELOG mis à jour
- [ ] Variables d'env documentées

### Déploiement

- [ ] Branche à jour avec main
- [ ] PR reviewée
- [ ] CI/CD vert

---

## Rapport

```
## Audit Report
- Projet : [nom]
- Date : YYYY-MM-DD

## Résumé
| Catégorie | Status | Blockers | Warnings |
|-----------|--------|----------|----------|
| Sécurité | ✅/❌ | N | N |
| Dépendances | ✅/❌ | N | N |
| Code Quality | ✅/❌ | N | N |
| Tests | ✅/❌ | N | N |
| Performance | ✅/❌ | N | N |
| i18n | ✅/❌ | N | N |
| Pre-release | ✅/❌ | N | N |

## Actions prioritaires
1. [Critique] ...
2. [Haute] ...
3. [Moyenne] ...
```

---

## Règles

- **Blocker** = résoudre avant release
- **Warning** = documenter et accepter le risque
- Prioriser : sécurité > bugs > code mort > conventions
