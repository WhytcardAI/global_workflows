# Analyse Complète de Workspace - Workflow Général

## 1. Découverte et Inventaire

### 1.1 Scanner la Structure

- Lister tous les fichiers et dossiers (ignorer node_modules, .git, dist, build)
- Identifier les langages utilisés via extensions (.ts, .js, .py, .java, etc.)
- Détecter les fichiers de configuration (package.json, tsconfig.json, .eslintrc, etc.)
- Cartographier l'architecture (monorepo, microservices, monolithique)

### 1.2 Analyser les Dépendances

- Parser les fichiers de dépendances (package.json, requirements.txt, pom.xml, etc.)
- Vérifier les versions obsolètes
- Identifier les dépendances non utilisées
- Détecter les vulnérabilités connues (via npm audit, pip-audit, etc.)

## 2. Analyse du Code Source

### 2.1 Code Mort

- Fonctions/méthodes jamais appelées
- Variables déclarées mais non utilisées
- Imports non utilisés
- Classes/modules orphelins
- Fichiers non référencés dans le projet
- Routes/endpoints non utilisés
- Composants UI non rendus

### 2.2 Duplication de Code

- Blocs de code identiques ou quasi-identiques
- Fonctions avec logique similaire
- Patterns répétés pouvant être abstraits
- Constantes/configurations dupliquées
- Tests avec setup identique

### 2.3 Bonnes Pratiques

#### Général

- Nommage cohérent (camelCase, PascalCase, snake_case selon conventions)
- Longueur des fonctions (< 50 lignes recommandé)
- Complexité cyclomatique (< 10 recommandé)
- Profondeur d'imbrication (< 4 niveaux)
- Nombre de paramètres (< 5 recommandé)
- Principe DRY (Don't Repeat Yourself)
- Principe SOLID
- Séparation des préoccupations

#### TypeScript/JavaScript

- Utilisation de `const`/`let` vs `var`
- Typage strict (pas de `any`)
- Gestion des erreurs (try/catch, .catch())
- Async/await vs callbacks
- Immutabilité des données

#### Python

- PEP 8 compliance
- Type hints
- Docstrings
- Context managers

## 3. Qualité et Tests

### 3.1 Couverture de Tests

- Pourcentage de couverture global
- Fichiers/fonctions non testés
- Tests sans assertions
- Tests fragiles (dépendant de l'ordre)

### 3.2 Qualité des Tests

- Tests unitaires vs intégration vs e2e
- Mocking approprié
- Isolation des tests
- Nommage descriptif

## 4. Sécurité

### 4.1 Vulnérabilités

- Secrets exposés (API keys, passwords)
- Injection SQL/XSS/CSRF
- Dépendances vulnérables
- Permissions trop permissives
- CORS mal configuré

### 4.2 Authentification/Autorisation

- Validation des tokens
- Gestion des sessions
- Contrôle d'accès

## 5. Performance

### 5.1 Optimisations

- Requêtes N+1
- Memoization manquante
- Lazy loading non implémenté
- Bundle size excessif
- Images non optimisées

### 5.2 Ressources

- Memory leaks potentiels
- Event listeners non nettoyés
- Connexions non fermées

## 6. Documentation Officielle - Recherche MCP Internet

### 6.1 Pour Chaque Technologie Détectée

```
Rechercher via MCP:
- Documentation officielle de [framework/library]
- Best practices [framework/library] [version]
- Migration guide [framework/library] [old_version] to [new_version]
- Security advisories [framework/library]
- Changelog [framework/library] latest
```

### 6.2 Vérifications Spécifiques

- APIs dépréciées utilisées
- Patterns obsolètes
- Nouvelles fonctionnalités recommandées
- Breaking changes à anticiper

## 7. Actions Correctives

### 7.1 Priorité Critique

- [ ] Vulnérabilités de sécurité
- [ ] Secrets exposés
- [ ] Bugs bloquants

### 7.2 Priorité Haute

- [ ] Code mort à supprimer
- [ ] Duplications majeures à refactoriser
- [ ] Dépendances vulnérables à mettre à jour

### 7.3 Priorité Moyenne

- [ ] Non-respect des conventions
- [ ] Tests manquants
- [ ] Documentation à ajouter

### 7.4 Priorité Basse

- [ ] Optimisations de performance
- [ ] Refactoring esthétique
- [ ] Mise à jour des dépendances mineures

## 8. Commandes d'Analyse

### Outils à Exécuter

```bash
# JavaScript/TypeScript
npx eslint . --ext .js,.ts,.tsx
npx tsc --noEmit
npx depcheck
npm audit

# Python
pylint **/*.py
mypy .
pip-audit
bandit -r .

# Général
# Recherche de secrets
git secrets --scan
gitleaks detect

# Duplication
npx jscpd .

# Complexité
npx complexity-report .
```

## 9. Rapport Final

### Template de Rapport

```
## Résumé Exécutif
- Score de qualité global: X/100
- Fichiers analysés: N
- Problèmes critiques: N
- Problèmes majeurs: N
- Problèmes mineurs: N

## Détails par Catégorie
[Tableaux détaillés]

## Plan d'Action Recommandé
[Liste priorisée]

## Ressources Documentation
[Liens vers docs officielles pertinentes]
```
