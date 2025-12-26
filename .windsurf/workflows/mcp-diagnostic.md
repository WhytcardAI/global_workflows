---
name: mcp-diagnostic
description: Diagnostic complet des serveurs MCP (status, tools, erreurs, config)
---

# MCP Diagnostic

## Objectif

Vérifier l'état de tous les **serveurs MCP** configurés : connexion, outils disponibles, erreurs, configuration.

## Quand l'utiliser

- Un outil MCP ne répond pas.
- Après modification de `mcp_config.json`.
- Audit périodique des intégrations.
- Onboarding sur un nouveau poste.

## Pipeline

### 1) Lister les serveurs configurés

- [ ] Lire `~/.codeium/windsurf-next/mcp_config.json`.
- [ ] Identifier chaque serveur et sa commande.

### 2) Vérifier la connexion

Pour chaque serveur :

- [ ] Vérifier que la commande existe.
- [ ] Vérifier les variables d'environnement requises.
- [ ] Tester la connexion.

**Checklist par serveur :**

```
[ ] Commande accessible (node, npx, python, etc.)
[ ] Arguments valides
[ ] Variables d'env définies
[ ] Port/socket disponible
[ ] Pas de conflit de version
```

### 3) Lister les outils disponibles

Pour chaque serveur connecté :

- [ ] Appeler `tools/list`.
- [ ] Documenter les outils et leurs paramètres.

### 4) Tester un outil par serveur

- [ ] Appel minimal pour vérifier le fonctionnement.
- [ ] Capturer les erreurs éventuelles.

### 5) Erreurs courantes et solutions

| Erreur         | Cause probable         | Solution                              |
| -------------- | ---------------------- | ------------------------------------- |
| `ENOENT`       | Commande introuvable   | Vérifier PATH, utiliser chemin absolu |
| `ECONNREFUSED` | Serveur non démarré    | Vérifier le process, redémarrer       |
| `Timeout`      | Serveur lent ou bloqué | Augmenter timeout, vérifier logs      |
| `Invalid JSON` | Réponse malformée      | Vérifier version du serveur           |
| `Auth failed`  | Token expiré/invalide  | Régénérer les credentials             |

### 6) Configuration Windows

Sur Windows, préférer :

```json
{
  "command": "node",
  "args": ["C:/path/to/server.js"]
}
```

Plutôt que :

```json
{
  "command": "npx",
  "args": ["server-name"]
}
```

### 7) Rapport

```
## Serveurs MCP

### ✅ Fonctionnels
| Serveur | Outils | Latence |
|---------|--------|---------|
| whytcard-brain | 8 | 50ms |
| context7 | 2 | 200ms |
| tavily | 4 | 300ms |

### ❌ Non connectés
| Serveur | Erreur | Action |
|---------|--------|--------|
| mcp-playwright | ENOENT | Installer playwright |
| chrome-devtools | Timeout | Vérifier Chrome |

### Configuration actuelle
[Extrait de mcp_config.json]

### Recommandations
1. [Serveur] — [Action]
```

## Commandes utiles

```bash
# Vérifier si node est accessible
node --version

# Tester un serveur MCP manuellement
echo '{"jsonrpc":"2.0","method":"tools/list","id":1}' | node path/to/server.js

# Logs Windsurf
# Windows: %APPDATA%\Windsurf - Next\logs\
# macOS: ~/Library/Application Support/Windsurf - Next/logs/
```

## Règles

- Documenter chaque serveur MCP utilisé.
- Tester après chaque mise à jour de config.
- Préférer les chemins absolus sur Windows.
- Garder les credentials hors du repo (env vars).
