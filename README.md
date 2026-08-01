# 🌍 DÉGG — Traducteur Temps Réel · JOJ Dakar 2026

> **DÉGG** signifie *"comprendre"* en Wolof.  
> Application PWA de traduction instantanée conçue pour les bénévoles et visiteurs des **Jeux Olympiques de la Jeunesse Dakar 2026**.

---

## ✨ Présentation

DÉGG est une application web progressive (PWA) mobile-first qui permet de briser la barrière de la langue lors des JOJ Dakar 2026. Elle fonctionne directement dans le navigateur, sans installation, et supporte **13 langues** dont le Wolof, langue nationale du Sénégal.

---

## 🗣️ Langues supportées

| Code | Langue | Drapeau |
|------|--------|---------|
| `WO` | Wolof | 🇸🇳 |
| `FR` | Français | 🇫🇷 |
| `EN` | Anglais | 🇬🇧 |
| `AR` | Arabe | 🇸🇦 |
| `ES` | Espagnol | 🇪🇸 |
| `PT` | Portugais | 🇵🇹 |
| `DE` | Allemand | 🇩🇪 |
| `RU` | Russe | 🇷🇺 |
| `ZH` | Chinois | 🇨🇳 |
| `JA` | Japonais | 🇯🇵 |
| `KO` | Coréen | 🇰🇷 |
| `SW` | Swahili | 🌍 |
| `HI` | Hindi | 🇮🇳 |

> **Note Wolof** : Le Wolof (`WO`) est mappé sur `fr-SN` pour la reconnaissance et la synthèse vocale (aucun support natif dans les navigateurs). DeepL ne supporte pas le Wolof nativement — les traductions vers `WO` peuvent échouer silencieusement.

---

## 🚀 Fonctionnalités

### 📝 Onglet Traduction
- Saisie de texte avec **auto-traduction** (debounce 800 ms)
- **Reconnaissance vocale** (Speech Recognition API)
- **Synthèse vocale** (Speech Synthesis API) pour écouter la traduction
- Historique des traductions de la session

### 💬 Onglet Phrases JOJ
Phrases utiles pré-traduites organisées par catégories :
- 📍 Orientation (stade, village olympique, transports…)
- 🏟️ Compétition (billets, scores, cérémonies…)
- 🍽️ Nourriture (allergies, régimes, restaurants…)
- 🚨 Urgences (ambulance, police, médecin…)
- 🤝 Politesse (salutations, remerciements…)
- 🏨 Hébergement (hôtel, réservation, check-out…)

Un clic sur une phrase l'envoie directement dans l'onglet Traduction.

### 🔄 Onglet Conversation
Mode dialogue bilingue A ↔ B :
- Chaque côté saisit dans sa propre langue
- La traduction s'effectue automatiquement vers l'autre langue
- Le résultat est lu à voix haute via `speechSynthesis`

### 🗺️ Onglet Mobilité
Explorateur des **3 zones JOJ** (Dakar · Diamniadio · Saly) :
- Navigation par site et par sport
- Informations de transport traduites à la demande
- Phrases de transport contextuelles (`TRANSPORT_PHRASES`)

### 🤖 Chatbot Assistant
Assistant conversationnel intégré pour répondre aux questions des visiteurs.

### 🧭 Guide Coach
Tutoriel interactif en 9 étapes, **bilingue FR/EN**, s'ouvre automatiquement à la première visite (via `sessionStorage`). La langue du guide est mémorisée dans `localStorage`.

### 📲 QR Code
Partage de l'URL courante de l'application via un QR code généré avec `qrcode.react`.

---

## 🏗️ Stack technique

| Technologie | Version | Rôle |
|-------------|---------|------|
| **Next.js** | 16.2.3 | Framework React (App Router) |
| **React** | 19.2.4 | UI |
| **TypeScript** | ^5 | Typage statique |
| **Tailwind CSS** | v4 | Styles (`@import "tailwindcss"`) |
| **DeepL API** | Free tier | Moteur de traduction |
| **Capacitor** | ^8.3.1 | Build Android / iOS natif |
| **qrcode.react** | ^4.2.0 | Génération de QR codes |

---

## 📁 Architecture du projet

```
src/
├── app/
│   ├── page.tsx            # Page principale — SPA avec 5 onglets
│   ├── layout.tsx          # Enregistrement PWA (sw.js + manifest.json)
│   ├── globals.css         # Design system Tailwind v4 + tokens CSS --degg-*
│   └── api/translate/
│       └── route.ts        # Route Handler → DeepL API (court-circuite si from === to)
├── components/
│   ├── GuideCoach.tsx      # Onboarding 9 étapes (bilingue FR/EN)
│   ├── MobiliteJOJ.tsx     # Explorateur 3 zones JOJ
│   ├── ChatbotAssistant.tsx# Chatbot assistant
│   ├── MapJOJ.tsx          # Carte interactive JOJ
│   ├── LandingScreen.tsx   # Écran d'accueil (par session)
│   └── QRModal.tsx         # Modal QR code
└── lib/
    ├── languages.ts        # LANGUAGES[], JOJ_PHRASES, LanguageCode
    ├── translate.ts        # translateBatch() — appels API groupés
    ├── chatbot.ts          # Logique chatbot + types ChatMessage
    └── ui-strings.ts       # Chaînes d'UI localisées (FR_UI, getUIStrings)

public/
├── manifest.json           # Manifest PWA
└── sw.js                   # Service Worker (offline)
```

---

## ⚙️ Installation & Démarrage

### Prérequis
- Node.js ≥ 18
- Un compte [DeepL API Free](https://www.deepl.com/pro-api) (gratuit jusqu'à 500 000 caractères/mois)

### 1. Cloner le projet

```bash
git clone https://github.com/Christylove229/DEGG.git
cd DEGG
```

### 2. Installer les dépendances

```bash
npm install
```

### 3. Configurer l'environnement

Créer (ou compléter) le fichier `.env.local` à la racine :

```env
DEEPL_API_KEY=votre-cle-api-deepl-free
```

### 4. Lancer en développement

```bash
npm run dev
```

L'application est accessible sur :
- **PC** → [http://localhost:3000](http://localhost:3000)
- **Mobile** (même réseau) → `http://192.168.0.XXX:3000`

---

## 🛠️ Commandes disponibles

```bash
npm run dev    # Serveur de développement (hot reload)
npm run build  # Build de production
npm run start  # Serveur de production
npm run lint   # Lint ESLint (Next.js core-web-vitals + TypeScript)
```

> ⚠️ Aucun framework de test n'est configuré.

---

## 📱 Build mobile (Capacitor)

DÉGG peut être packagée en application Android/iOS native via Capacitor.

```bash
# Build Next.js
npm run build

# Synchroniser vers Android
npx cap sync android

# Ouvrir dans Android Studio
npx cap open android
```

L'app ID est `com.degg.joj2026`. La couleur de fond est `#0a2a16`.

> En développement, Capacitor pointe sur `http://10.0.2.2:3000` (émulateur Android → localhost).  
> En production, modifier `server.url` dans `capacitor.config.ts` avec l'URL Vercel déployée.

---

## 🎨 Design System

Les tokens de design sont des variables CSS préfixées `--degg-*` (aliasées en `--joj-*`) :

| Token | Rôle |
|-------|------|
| `degg.bg` | Fond principal (vert sombre) |
| `degg.ink` | Texte |
| `degg.green` / `degg.green2` | Verts JOJ |
| `degg.orange` / `degg.orange2` | Oranges JOJ |
| `degg.yellow` | Jaune accent |

Classes CSS personnalisées : `joj-btn`, `joj-btn-primary`, `joj-btn-ghost`, `joj-fab`, `joj-coach`, `joj-overlay`.

Animations : `fade-up`, `shimmer`.

---

## 🌐 Déploiement (Vercel)

```bash
# Pousser sur main → déploiement automatique
git push origin main
```

Ou manuellement via [vercel.com/new](https://vercel.com/new).

**Variable d'environnement à configurer sur Vercel :**

```
DEEPL_API_KEY = votre-cle-api-deepl-free
```

---

## 📄 Licence

Projet développé dans le cadre des **Jeux Olympiques de la Jeunesse Dakar 2026**.

---

<div align="center">
  <strong>🇸🇳 DÉGG — Comprendre le monde, ensemble.</strong>
</div>
