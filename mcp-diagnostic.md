---
description: Diagnostic complet des serveurs MCP (status, tools, erreurs, config)
---

# MCP Diagnostic

Vérifier l'état de tous les **serveurs MCP** configurés.

---

## 1) Lister les serveurs

- [ ] Lire `~/.codeium/windsurf-next/mcp_config.json`
- [ ] Identifier chaque serveur et sa commande

---

## 2) Vérifier la connexion

Pour chaque serveur :

- [ ] Commande accessible (node, npx, python)
- [ ] Arguments valides
- [ ] Variables d'env définies
- [ ] Port/socket disponible

---

## 3) Lister les outils

- [ ] Appeler `tools/list` pour chaque serveur
- [ ] Documenter les outils disponibles

---

## 4) Erreurs courantes

| Erreur         | Cause                | Solution                     |
| -------------- | -------------------- | ---------------------------- |
| `ENOENT`       | Commande introuvable | Vérifier PATH, chemin absolu |
| `ECONNREFUSED` | Serveur non démarré  | Redémarrer                   |
| `Timeout`      | Serveur lent/bloqué  | Vérifier logs                |
| `Invalid JSON` | Réponse malformée    | Vérifier version             |
| `Auth failed`  | Token expiré         | Régénérer credentials        |

---

## 5) Config Windows

Préférer :

```json
{
  "command": "node",
  "args": ["C:/path/to/server.js"]
}
```

Plutôt que `npx` qui peut avoir des problèmes de PATH.

---

## 6) Commandes utiles

```bash
node --version

# Logs Windsurf
# Windows: %APPDATA%\Windsurf - Next\logs\
# macOS: ~/Library/Application Support/Windsurf - Next/logs/
```

---

## Rapport

```
## Serveurs MCP

### ✅ Fonctionnels
| Serveur | Outils | Latence |
|---------|--------|---------|
| whytcard-brain | 8 | 50ms |
| context7 | 2 | 200ms |

### ❌ Non connectés
| Serveur | Erreur | Action |
|---------|--------|--------|
| mcp-playwright | ENOENT | Installer |

### Recommandations
1. [Serveur] — [Action]
```
