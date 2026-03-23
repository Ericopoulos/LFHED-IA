# LFHED-IA — Assistant Informatique 🎓

Assistant informatique intelligent pour le Lycée Français Hors d'Europe à Distance.  
**Progressive Web App** — fonctionne sur Android, iPhone et ordinateur.

---

## 🚀 Déploiement rapide sur GitHub Pages

### 1. Créer le dépôt
```bash
git init
git add .
git commit -m "Initial commit — LFHED-IA v1.0"
git branch -M main
git remote add origin https://github.com/VOTRE-USERNAME/lfhed-ia.git
git push -u origin main
```

### 2. Activer GitHub Pages
- Allez dans **Settings** → **Pages**
- Source : **Deploy from a branch** → `main` → `/ (root)`
- Cliquez **Save**
- Votre app sera disponible sur : `https://VOTRE-USERNAME.github.io/lfhed-ia/`

### 3. (Optionnel) Nom de domaine personnalisé
Ajoutez un fichier `CNAME` avec votre domaine, ex : `ia.lfhed.edu`

---

## 📱 Installation sur les appareils

### iPhone / iPad (Safari)
1. Ouvrir l'URL dans **Safari**
2. Appuyer sur **Partager** (icône carrée avec flèche)
3. Sélectionner **Sur l'écran d'accueil**
4. L'icône LFHED-IA apparaît comme une vraie app

### Android (Chrome)
1. Ouvrir l'URL dans **Chrome**
2. Un bandeau "Installer l'application" apparaît automatiquement
3. Ou : menu ⋮ → **Ajouter à l'écran d'accueil**

---

## ⚙️ Configuration de la clé API (pour les réponses IA)

1. Créer un compte sur [console.anthropic.com](https://console.anthropic.com)
2. Générer une clé API (commence par `sk-ant-...`)
3. Dans l'application → **Paramètres** ⚙️ → **Clé API Anthropic**
4. Coller la clé → **Enregistrer**

> **Note sécurité** : La clé est stockée uniquement sur l'appareil de l'utilisateur (localStorage). Elle n'est jamais envoyée à un serveur tiers.

---

## 🌐 Fonctionnement hors-ligne

Sans connexion ou sans clé API, l'application utilise sa **base de connaissances locale** couvrant :

| Catégorie | Problèmes couverts |
|-----------|-------------------|
| 🌐 Réseau & WiFi | WiFi lent, impossible de se connecter, sites bloqués |
| 🖥️ Matériel | Imprimante hors ligne, ordinateur lent |
| 💻 Logiciels | Pronote, Outlook/messagerie |
| 📽️ Vidéo/Audio | Projecteur sans image, pas de son |

---

## 🗂️ Structure du projet

```
lfhed-ia/
├── index.html        ← Application principale (tout-en-un)
├── manifest.json     ← Configuration PWA
├── sw.js             ← Service Worker (cache hors-ligne)
├── icons/            ← Icônes de l'application
│   ├── icon-192.png
│   └── icon-512.png
└── README.md
```

---

## 🔧 Personnalisation

### Ajouter des problèmes à la base de connaissances

Dans `index.html`, modifiez l'objet `KB` :

```javascript
const KB = {
  fr: {
    network: [
      {
        id: 'mon-probleme',
        emoji: '🔧',
        title: 'Description du problème',
        cat: 'Réseau',
        steps: [
          { text: 'Étape 1...', note: 'Note optionnelle' },
          { text: 'Étape 2...', note: null },
        ],
        alts: ['Alternative 1', 'Alternative 2']
      }
    ]
  }
}
```

### Modifier les coordonnées du service informatique

Recherchez `escalate()` dans `index.html` et modifiez :
```javascript
function escalate() {
  showToast('📞 +XX XXX XXX XXXX — Votre message');
}
```

---

## 🌍 Langues supportées

- 🇫🇷 Français (défaut)
- 🇬🇷 Grec (Ελληνικά)

Pour ajouter une langue, ajoutez une entrée dans les objets `T` et `KB`.

---

## 📄 Licence

Usage interne — Lycée Français Hors d'Europe à Distance  
Développé avec Claude AI (Anthropic)
