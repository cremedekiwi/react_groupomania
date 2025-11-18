# Migration vers la configuration Axios centralisée

Pour que votre frontend fonctionne correctement en production, vous devez migrer tous vos appels axios pour utiliser la configuration centralisée.

## Option 1 : Migration Manuelle (Recommandé pour comprendre)

### Avant (exemple dans Home.js)
```javascript
import axios from 'axios'

axios.get('http://localhost:3001/posts', {
  headers: { accessToken: localStorage.getItem('accessToken') },
})
```

### Après
```javascript
import axios from '../api/axios'

// L'URL de base et les headers sont automatiquement ajoutés
axios.get('/posts')
```

## Option 2 : Script de Migration Automatique

Exécutez ce script pour migrer automatiquement tous vos fichiers :

```bash
cd /Users/jarumuga/Documents/react_groupomania/client/src/pages

# Pour chaque fichier, remplacer les imports
find . -name "*.js" -type f -exec sed -i '' "s|import axios from 'axios'|import axios from '../api/axios'|g" {} +

# Remplacer les URLs complètes par des chemins relatifs
find . -name "*.js" -type f -exec sed -i '' "s|'http://localhost:3001/|'/|g" {} +
find . -name "*.js" -type f -exec sed -i '' "s|\"http://localhost:3001/|\"/|g" {} +

# Supprimer les headers accessToken manuels (maintenant automatiques)
# ATTENTION : Cette commande est complexe, vérifiez manuellement après
```

## Fichiers à migrer

Vous devez mettre à jour ces fichiers :
- src/pages/Home.js
- src/pages/Login.js
- src/pages/Post.js
- src/pages/Profile.js
- src/pages/Registration.js

## Vérification

Après la migration :
1. Démarrez le serveur : `cd server && npm run dev`
2. Démarrez le client : `cd client && npm start`
3. Testez toutes les fonctionnalités (login, posts, comments, likes)

## Avantages de cette migration

- URL de l'API configurable via variable d'environnement
- Headers d'authentification automatiques
- Fonctionne en développement et en production sans modification
- Code plus propre et maintenable
