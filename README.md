# LFHED-IA v4 — Authentification Supabase

Système d'authentification centralisé via **Supabase** (gratuit jusqu'à 50 000 utilisateurs).
Comptes synchronisés entre tous les appareils, import CSV, gestion admin en ligne.

---

## 🗄️ Étape 1 — Créer le projet Supabase

1. Allez sur **https://supabase.com** → "Start your project"
2. Créez un projet : nom `lfhed-ia`, région `eu-west-3` (Paris)
3. Notez le **mot de passe** de la base de données

---

## 🔑 Étape 2 — Récupérer les clés API

Dans **Project Settings → API** :

| Clé | Où la mettre dans index.html |
|-----|------------------------------|
| `Project URL` | `SUPABASE_URL` |
| `anon / public` | `SUPABASE_ANON` |
| `service_role` ⚠️ | `SUPABASE_SERVICE` |

> ⚠️ La clé `service_role` est secrète — ne la partagez jamais publiquement.

---

## 🗃️ Étape 3 — Créer la table `profiles`

Dans **SQL Editor**, exécutez ce script :

```sql
-- Table des profils utilisateurs
CREATE TABLE profiles (
  id          UUID PRIMARY KEY REFERENCES auth.users(id) ON DELETE CASCADE,
  prenom      TEXT NOT NULL,
  nom         TEXT NOT NULL,
  email       TEXT NOT NULL UNIQUE,
  role        TEXT DEFAULT 'enseignant',
  suspended   BOOLEAN DEFAULT FALSE,
  must_change_pwd BOOLEAN DEFAULT TRUE,
  created_at  TIMESTAMPTZ DEFAULT NOW()
);

-- Activer RLS (Row Level Security)
ALTER TABLE profiles ENABLE ROW LEVEL SECURITY;

-- Politique : chaque utilisateur peut lire/modifier son propre profil
CREATE POLICY "users_own_profile" ON profiles
  FOR ALL USING (auth.uid() = id);

-- Politique : les admins peuvent tout faire (via service_role — pas de restriction)
-- La clé service_role bypass automatiquement RLS
```

---

## ⚙️ Étape 4 — Configurer l'authentification

Dans **Authentication → Settings** :

- **Site URL** : `https://VOTRE-USERNAME.github.io/lfhed-ia/`
- **Redirect URLs** : ajoutez la même URL
- **Email** : activez "Enable email confirmations" → **désactivez** (les comptes sont créés par l'admin, pas besoin de confirmation)

Dans **Authentication → Email Templates**, personnalisez si souhaité :
- **Invite** : email envoyé lors de l'ajout individuel
- **Reset Password** : email de réinitialisation

---

## 🔧 Étape 5 — Configurer index.html

```javascript
const SUPABASE_URL    = 'https://XXXX.supabase.co';
const SUPABASE_ANON   = 'eyJh...';   // clé anon
const SUPABASE_SERVICE= 'eyJh...';   // clé service_role
const ADMIN_EMAILS    = ['informatique@lfh.gr'];
const DEFAULT_PWD     = 'Lfhed2025!'; // mot de passe par défaut import CSV
```

---

## 📥 Format CSV pour l'import

```csv
prenom,nom,email,role
Marie,Dupont,marie.dupont@lfh.gr,enseignant
Jean,Martin,jean.martin@lfh.gr,direction
Sofia,Papadopoulos,sofia.papadopoulos@lfh.gr,admin-scolaire
```

**Rôles disponibles** : `enseignant`, `direction`, `admin-scolaire`, `autre`

> Tous les utilisateurs importés reçoivent le mot de passe `DEFAULT_PWD` et sont forcés à le changer à la première connexion.

---

## 🚀 Étape 6 — Déployer sur GitHub Pages

```bash
git add . && git commit -m "LFHED-IA v4 Supabase" && git push
```

**Settings → Pages → main / root → Save**

---

## 🔄 Fonctionnement

```
Connexion utilisateur
  → Supabase Auth vérifie email + mot de passe
  → Charge le profil depuis la table "profiles"
  → Si must_change_pwd = true → écran de changement forcé
  → Accès à l'app

Ajout individuel (admin)
  → Crée le compte via Supabase Admin API
  → Insère le profil dans "profiles"
  → Envoie un email d'invitation automatique

Import CSV (admin)
  → Parse le fichier
  → Crée chaque compte en séquence
  → Insère les profils en masse
  → Rapport final : créés / ignorés / erreurs
```

---

## 📁 Fichiers

```
lfhed-ia/
├── index.html      ← Application v4 complète
├── manifest.json   ← PWA config
├── sw.js           ← Service Worker
├── README.md
└── icons/
    ├── icon-192.png
    └── icon-512.png
```

---

*LFHED-IA v4 — Supabase Auth — Lycée Français Hors d'Europe*
