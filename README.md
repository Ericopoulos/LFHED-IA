# LFHED-IA v3 — Authentification locale @lfh.gr

Système d'inscription/connexion **100% hors-ligne**, sans serveur ni Firebase.

---

## ⚙️ Configuration rapide (`index.html`)

```javascript
const ALLOWED_DOMAIN = "lfh.gr";         // Domaine autorisé
const ADMIN_USERS    = ["informatique"]; // Identifiant(s) admin
const ADMIN_PIN      = "2025";          // ← CHANGEZ CE PIN
const SESSION_MS     = 8 * 60 * 60 * 1000; // Session : 8h
```

## 🛡️ Compte admin pré-créé

| Identifiant | Mot de passe initial | PIN Admin |
|-------------|----------------------|-----------|
| `informatique` | `Admin2025!` | `2025` |

> ⚠️ Changez le mot de passe et le PIN au premier accès.

## 👤 Fonctionnement

- Seules les adresses **@lfh.gr** sont acceptées à l'inscription
- Comptes stockés dans le localStorage de **chaque appareil**
- Sessions expirent après 8h d'inactivité
- Panneau Admin protégé par code PIN séparé

## ⚠️ Limitation

Les comptes sont locaux à chaque appareil.
Un collègue doit s'inscrire sur chaque appareil qu'il utilise.

## 🚀 Déploiement GitHub Pages

```bash
git add . && git commit -m "v3" && git push
```
Settings → Pages → main / root → Save

