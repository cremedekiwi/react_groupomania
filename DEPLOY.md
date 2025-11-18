# Guide de Déploiement sur Render

Ce guide vous explique comment déployer votre application Groupomania sur Render.

## Étape 1 : Préparer votre dépôt GitHub

## Étape 2 : Déployer le Backend (API)

### 2.1 Créer un nouveau Web Service sur Render

1. Connectez-vous à [render.com](https://render.com)
2. Cliquez sur **"New +"** → **"Web Service"**
3. Connectez votre dépôt GitHub
4. Sélectionnez votre dépôt `react_groupomania`

### 2.2 Configurer le Web Service

Remplissez les informations suivantes :

- **Name** : `groupomania-api` (ou un nom de votre choix)
- **Region** : Choisissez la région la plus proche (Europe (Frankfurt) recommandé)
- **Branch** : `main` (ou `master`)
- **Root Directory** : `server`
- **Environment** : `Node`
- **Build Command** : `npm install`
- **Start Command** : `npm start`
- **Instance Type** : `Free`

### 2.3 Ajouter les variables d'environnement

Dans la section **"Environment Variables"**, ajoutez :

| Key | Value |
|-----|-------|
| `NODE_ENV` | `production` |
| `SECRET` | `66861DA123944F5FFCDB7623B98E1` (ou générez un nouveau secret) |
| `PORT` | `3001` |

**NE PAS ENCORE DÉPLOYER** - Nous devons d'abord créer la base de données.

## Étape 3 : Créer la Base de Données PostgreSQL

### 3.1 Créer une nouvelle base de données

1. Dans le dashboard Render, cliquez sur **"New +"** → **"PostgreSQL"**
2. Remplissez les informations :
   - **Name** : `groupomania-db`
   - **Database** : `groupomania`
   - **User** : `groupomania_user` (automatique)
   - **Region** : Même région que votre backend
   - **PostgreSQL Version** : Dernière version
   - **Instance Type** : `Free`

3. Cliquez sur **"Create Database"**

### 3.2 Lier la base de données au backend

1. Retournez dans votre Web Service `groupomania-api`
2. Allez dans **"Environment"**
3. Ajoutez une nouvelle variable d'environnement :

| Key | Value |
|-----|-------|
| `DATABASE_URL` | (Copiez l'Internal Database URL depuis votre base de données PostgreSQL) |

Pour trouver l'URL :
- Allez dans votre base de données PostgreSQL `groupomania-db`
- Copiez la valeur de **"Internal Database URL"**
- Collez-la dans la variable `DATABASE_URL` de votre Web Service

4. Cliquez sur **"Save Changes"**

### 3.3 Déployer le backend

Render va maintenant automatiquement déployer votre backend. Attendez quelques minutes.

Une fois le déploiement terminé, vous verrez un lien comme :
```
https://groupomania-api.onrender.com
```

**NOTEZ CETTE URL** - Vous en aurez besoin pour le frontend !

## Étape 4 : Déployer le Frontend (Client React)

### 4.1 Créer un nouveau Static Site sur Render

1. Cliquez sur **"New +"** → **"Static Site"**
2. Connectez le même dépôt GitHub
3. Sélectionnez votre dépôt `react_groupomania`

### 4.2 Configurer le Static Site

Remplissez les informations suivantes :

- **Name** : `groupomania-client` (ou un nom de votre choix)
- **Region** : Même région que votre backend
- **Branch** : `main` (ou `master`)
- **Root Directory** : `client`
- **Build Command** : `npm install && npm run build`
- **Publish Directory** : `build`

### 4.3 Ajouter les variables d'environnement

Dans la section **"Environment Variables"**, ajoutez :

| Key | Value |
|-----|-------|
| `REACT_APP_API_URL` | `https://groupomania-api.onrender.com` (remplacez par VOTRE URL backend) |

### 4.4 Déployer le frontend

Cliquez sur **"Create Static Site"**

Render va automatiquement :
1. Installer les dépendances
2. Compiler votre application React
3. Déployer les fichiers statiques

Une fois terminé, vous aurez une URL comme :
```
https://groupomania-client.onrender.com
```