# CCarré

CCarré est une plateforme web développée dans le cadre d'un projet universitaire à Aix-Marseille Université.

Le projet est composé de deux parties :

- **Frontend** : Next.js avec App Router
- **Backend** : API REST TypeScript/Express avec MongoDB

## Stack technique

### Frontend

- Next.js
- React
- TypeScript
- App Router
- pnpm

### Backend

- Node.js
- TypeScript
- Express
- MongoDB / Mongoose
- Socket.io
- Cloudinary
- Nodemailer / SMTP
- JWT

---

# Prérequis

Avant de commencer, installer :

- **Node.js 20+**
- **pnpm** pour le frontend
- **npm 10+** pour le backend
- **MongoDB** local ou MongoDB Atlas
- Un compte **Cloudinary**
- Un compte SMTP, **SendGrid recommandé** pour la production

Vérifier les versions :

```bash
node -v
npm -v
pnpm -v
```

Si pnpm n'est pas installé :

```bash
npm install -g pnpm
```

---

# Installation

Le frontend et le backend doivent être installés séparément.

## 1. Backend

Se placer dans le dossier du backend :

```bash
cd backend
```

Installer les dépendances :

```bash
npm install
```

### Configuration de l'environnement

Créer un fichier `.env` à la racine du backend.

Exemple :

```env
# Environnement
NODE_ENV=development
PORT=5000

# MongoDB
MONGODB_URI=mongodb://127.0.0.1:27017/ccarre

# JWT
JWT_SECRET=change_this_with_a_long_random_secret
JWT_EXPIRES_IN=7d

# SMTP / Nodemailer
SMTP_HOST=smtp.sendgrid.net
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=apikey
SMTP_PASS=SG_xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx

# Cloudinary
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

> Ne jamais commiter le fichier `.env`.

---

# Configuration des services

## MongoDB

Deux possibilités sont disponibles.

### MongoDB local

Installer MongoDB puis utiliser :

```env
MONGODB_URI=mongodb://127.0.0.1:27017/ccarre
```

### MongoDB Atlas

1. Créer un cluster MongoDB Atlas.
2. Créer un utilisateur de base de données.
3. Autoriser l'adresse IP utilisée pour la connexion.
4. Récupérer l'URI de connexion.
5. La renseigner dans `MONGODB_URI`.

Exemple :

```env
MONGODB_URI=mongodb+srv://<username>:<password>@<cluster-url>/<db>?retryWrites=true&w=majority
```

---

## Cloudinary

Cloudinary est utilisé pour le stockage et l'upload des images.

Depuis le dashboard Cloudinary, récupérer :

- `cloud_name`
- `api_key`
- `api_secret`

Puis les renseigner dans `.env` :

```env
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

---

## SendGrid / SMTP

Le projet utilise Nodemailer pour l'envoi des emails.

SendGrid est recommandé en production.

1. Créer un compte SendGrid.
2. Vérifier un expéditeur.
3. Générer une API Key avec les permissions nécessaires.
4. Configurer les variables SMTP :

```env
SMTP_HOST=smtp.sendgrid.net
SMTP_PORT=587
SMTP_SECURE=false
SMTP_USER=apikey
SMTP_PASS=<ta_sendgrid_api_key>
```

---

# 2. Frontend

Se placer dans le dossier du frontend :

```bash
cd frontend
```

Installer les dépendances :

```bash
pnpm install
```

### Configuration de l'API

Créer un fichier `.env.local` à la racine du frontend :

```env
NEXT_PUBLIC_API_URL=http://localhost:5000
```

Si le backend utilise une autre adresse ou un autre port, modifier cette valeur.

> Après toute modification de `.env.local`, redémarrer le serveur Next.js.

---

# Lancer le projet

Les deux serveurs doivent fonctionner simultanément.

## Backend

Depuis le dossier `backend` :

```bash
npm run dev
```

L'API sera disponible sur :

```text
http://localhost:5000
```

## Frontend

Depuis le dossier `frontend` :

```bash
pnpm dev
```

Le frontend sera disponible sur :

```text
http://localhost:3000
```

Ouvrir ensuite :

```text
http://localhost:3000
```

---

# Vérifier le backend

Le backend possède un endpoint de vérification :

```http
GET /api/health
```

Avec une installation locale :

```bash
curl http://localhost:5000/api/health
```

Si le serveur fonctionne correctement, l'API doit répondre à cette requête.

---

# Scripts disponibles

## Frontend

| Commande | Description |
|---|---|
| `pnpm dev` | Lance Next.js en mode développement |
| `pnpm build` | Génère le build de production |
| `pnpm start` | Lance le build de production |
| `pnpm lint` | Vérifie le code avec ESLint |

## Backend

| Commande | Description |
|---|---|
| `npm run dev` | Lance le serveur en mode développement |
| `npm run build` | Compile TypeScript vers `dist` |
| `npm start` | Lance le serveur compilé |
| `npm run lint` | Vérifie le code avec ESLint |

---

# Build de production

## Backend

```bash
npm run build
npm start
```

## Frontend

```bash
pnpm build
pnpm start
```

---

# Structure du projet

```text
ccarre/
├── frontend/
│   ├── app/
│   ├── components/
│   ├── ...
│   └── package.json
│
├── backend/
│   ├── src/
│   ├── dist/
│   ├── .env
│   └── package.json
│
└── README.md
```

---

# Problèmes fréquents

### Le frontend ne se lance pas

Vérifier :

- que Node.js est installé ;
- que les dépendances sont installées avec `pnpm install` ;
- que le port `3000` est disponible.

### Les données ne remontent pas

Vérifier :

- que le backend est lancé ;
- que MongoDB est accessible ;
- que `NEXT_PUBLIC_API_URL` pointe vers la bonne adresse ;
- que le serveur Next.js a été redémarré après une modification de `.env.local`.

### Le backend ne se connecte pas à MongoDB

Vérifier :

- `MONGODB_URI` ;
- que MongoDB est lancé si vous utilisez une instance locale ;
- que votre adresse IP est autorisée si vous utilisez MongoDB Atlas.

### Les emails ne sont pas envoyés

Vérifier :

- les identifiants SMTP ;
- la clé API SendGrid ;
- que l'expéditeur est vérifié dans SendGrid.

### Les images ne sont pas uploadées

Vérifier les trois variables Cloudinary :

```env
CLOUDINARY_CLOUD_NAME=
CLOUDINARY_API_KEY=
CLOUDINARY_API_SECRET=
```

---

# Sécurité

- Ne jamais commiter `.env` ou `.env.local`.
- Ne jamais publier les clés API, mots de passe ou secrets JWT.
- Utiliser un `JWT_SECRET` suffisamment long et aléatoire.
- En production, utiliser des credentials dédiés aux services.
- Si un secret est accidentellement exposé, le révoquer et en générer un nouveau immédiatement.
