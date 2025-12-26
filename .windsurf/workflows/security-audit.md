---
name: security-audit
description: Audit sécurité (secrets, vulnérabilités, injections, auth, CORS)
---

# Security Audit

## Objectif

Détecter les **vulnérabilités de sécurité** : secrets exposés, dépendances vulnérables, injections, mauvaise config auth/CORS.

## Quand l'utiliser

- Avant déploiement en production.
- Après ajout de dépendances.
- Audit périodique (mensuel minimum).
- Après incident de sécurité.

## Pipeline

### 1) Secrets exposés (CRITIQUE)

- [ ] Rechercher dans le code : API keys, passwords, tokens.
- [ ] Vérifier `.env` n'est pas commité.
- [ ] Vérifier `.gitignore` inclut les fichiers sensibles.

**Commandes :**

```bash
# Recherche patterns secrets
git secrets --scan
gitleaks detect --source .

# Patterns manuels
grep -rn "API_KEY\|SECRET\|PASSWORD\|TOKEN" --include="*.ts" --include="*.js" --include="*.env*"
```

### 2) Dépendances vulnérables

- [ ] Scanner les CVE connues.
- [ ] Mettre à jour les dépendances critiques.

**Commandes :**

```bash
# Node.js
npm audit
pnpm audit

# Python
pip-audit
safety check
```

### 3) Injections

- [ ] **SQL Injection** : requêtes paramétrées obligatoires.
- [ ] **XSS** : échapper les inputs utilisateur.
- [ ] **Command Injection** : pas de `exec()` avec input user.
- [ ] **Path Traversal** : valider les chemins de fichiers.

**Patterns à rechercher :**

```bash
grep -rn "eval\|exec\|innerHTML\|dangerouslySetInnerHTML" --include="*.ts" --include="*.tsx" --include="*.js"
```

### 4) Authentification / Autorisation

- [ ] Tokens JWT : vérifier expiration et signature.
- [ ] Sessions : régénérer après login.
- [ ] Mots de passe : hashés (bcrypt/argon2), jamais en clair.
- [ ] Rate limiting sur endpoints sensibles.
- [ ] RBAC/permissions vérifiées côté serveur.

### 5) Configuration CORS

- [ ] Pas de `Access-Control-Allow-Origin: *` en prod.
- [ ] Whitelist explicite des origines autorisées.
- [ ] Credentials : `Access-Control-Allow-Credentials` seulement si nécessaire.

### 6) Headers de sécurité

- [ ] `Content-Security-Policy` configuré.
- [ ] `X-Frame-Options: DENY` ou `SAMEORIGIN`.
- [ ] `X-Content-Type-Options: nosniff`.
- [ ] `Strict-Transport-Security` (HSTS).

### 7) Rapport

```
## Résumé Sécurité
- Vulnérabilités critiques : N
- Vulnérabilités hautes : N
- Vulnérabilités moyennes : N
- Vulnérabilités basses : N

## Détails

### CRITIQUE
- [CVE-XXXX] [dépendance] — [description] — [action]

### Secrets exposés
- [fichier:ligne] — [type] — SUPPRIMER IMMÉDIATEMENT

### Injections potentielles
- [fichier:ligne] — [type] — [recommandation]

### Config manquante
- [header/config] — [recommandation]

## Plan de remédiation
1. [Immédiat] ...
2. [Cette semaine] ...
3. [Ce mois] ...
```

## Règles

- **CRITIQUE** = bloquer le déploiement.
- Secrets exposés = rotation immédiate + suppression historique git.
- Documenter les exceptions avec justification et date de revue.
