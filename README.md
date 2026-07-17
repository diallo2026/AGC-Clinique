# AGC — Déploiement Firebase (Hosting + Firestore temps réel)

Votre application est maintenant connectée à **Firestore** (la base de
données de Firebase) : toute modification faite sur un appareil apparaît
automatiquement, en temps réel, sur toutes les autres machines connectées
à la même clinique. Ce guide vous fait passer de "code sur GitHub" à
"site en ligne avec synchronisation active".

---

## Étape 1 — Créer la base de données Firestore

1. Allez sur [console.firebase.google.com](https://console.firebase.google.com)
2. Ouvrez votre projet (ou créez-en un si ce n'est pas déjà fait :
   **Ajouter un projet** → donnez-lui un nom → Google Analytics n'est pas
   nécessaire, vous pouvez le désactiver)
3. Dans le menu de gauche, cliquez sur **Firestore Database**
4. Cliquez sur **Créer une base de données**
5. Choisissez une région proche de vous (ex. `europe-west` si disponible,
   sinon la plus proche de la Guinée)
6. Sélectionnez **Mode production** (les vraies règles de sécurité seront
   déployées à l'étape 4 — ne restez pas indéfiniment en "mode test", qui
   expire après 30 jours et ferme l'accès à tout le monde, y compris vous)

---

## Étape 2 — Récupérer la configuration de votre application web

1. Dans la Console Firebase, cliquez sur l'icône **⚙️ (roue dentée)** en
   haut à gauche → **Paramètres du projet**
2. Descendez jusqu'à **Vos applications**
3. Si aucune application web n'existe encore, cliquez sur l'icône **</>**
   (Web) → donnez un nom (ex. "AGC Clinique") → **Enregistrer l'application**
   (pas besoin de configurer Firebase Hosting depuis cet écran, on le fera
   en ligne de commande)
4. Vous verrez un bloc de code ressemblant à ceci :
   ```js
   const firebaseConfig = {
     apiKey: "AIzaSy...",
     authDomain: "votre-projet.firebaseapp.com",
     projectId: "votre-projet",
     storageBucket: "votre-projet.appspot.com",
     messagingSenderId: "123456789",
     appId: "1:123456789:web:abcdef"
   };
   ```
5. Gardez cette page ouverte, vous en avez besoin à l'étape suivante.

---

## Étape 3 — Coller la configuration dans le fichier

1. Ouvrez `public/index.html` dans un éditeur de texte (Bloc-notes,
   VS Code, ou l'éditeur de fichiers de GitHub directement dans votre
   navigateur)
2. Cherchez ce bloc (vers le début du fichier, juste après les balises
   `<script>` de Firebase) :
   ```js
   const firebaseConfig = {
     apiKey: "__FIREBASE_API_KEY__",
     authDomain: "__FIREBASE_AUTH_DOMAIN__",
     projectId: "__FIREBASE_PROJECT_ID__",
     storageBucket: "__FIREBASE_STORAGE_BUCKET__",
     messagingSenderId: "__FIREBASE_SENDER_ID__",
     appId: "__FIREBASE_APP_ID__"
   };
   ```
3. Remplacez chacune des six valeurs `"__FIREBASE_..._​__"` par la valeur
   correspondante copiée à l'étape 2 (gardez les guillemets).
4. Enregistrez le fichier.
5. Renvoyez ce changement sur GitHub :
   ```bash
   git add public/index.html
   git commit -m "Configuration Firebase"
   git push
   ```

---

## Étape 4 — Déployer les règles Firestore et le site

### Prérequis (une seule fois sur votre machine)
```bash
npm install -g firebase-tools
firebase login
```

### Relier ce dossier à votre projet
1. Ouvrez `.firebaserc` et remplacez
   `REMPLACEZ-PAR-VOTRE-ID-PROJET-FIREBASE` par l'ID de votre projet
   (visible dans **Paramètres du projet**, en haut de la page — ressemble
   à `agc-clinique-a1b2c`)

### Déployer
Depuis ce dossier, dans un terminal :
```bash
firebase deploy
```
Cette commande envoie **à la fois** :
- les règles de sécurité Firestore (`firestore.rules`)
- votre site (`public/index.html`) sur Firebase Hosting

À la fin, une **Hosting URL** s'affiche (ex.
`https://votre-projet.web.app`) — c'est l'adresse de votre clinique en
ligne, accessible depuis n'importe quel appareil.

---

## Étape 5 — Vérifier la synchronisation temps réel

1. Ouvrez la Hosting URL sur deux appareils différents (ou deux
   navigateurs)
2. Connectez-vous à la même clinique sur les deux
3. Ajoutez un patient sur l'un des deux → il doit apparaître
   **automatiquement, sans recharger la page**, sur l'autre appareil en
   quelques secondes
4. Dans la barre latérale, le point vert "☁️ Synchronisé" confirme que la
   connexion au cloud fonctionne

---

## ⚠️ Sécurité — à lire avant d'utiliser de vraies données patients

Les règles Firestore fournies (`firestore.rules`) sont **ouvertes** :
elles permettent à quiconque connaît la configuration technique de votre
projet Firebase de lire ou modifier n'importe quelle clinique dans la
base, pas seulement la vôtre. La protection actuelle repose entièrement
sur le fait que vos codes clinique/PIN restent secrets à l'intérieur de
l'application — ce n'est pas une vraie protection au niveau de la base
de données.

C'est correct pour tester, mais **avant de mettre de vraies données
patients en production**, il est recommandé d'ajouter une vraie couche
d'authentification (Firebase Authentication) pour que les règles
Firestore puissent vérifier qui a le droit d'accéder à quoi. Dites-le moi
si vous voulez qu'on la mette en place — c'est un ajout que je peux faire
par-dessus ce qui existe déjà.

---

## En cas de problème

- **"Missing or insufficient permissions"** dans la console du
  navigateur → les règles Firestore n'ont pas encore été déployées.
  Relancez `firebase deploy`.
- **La synchronisation ne se déclenche pas** → vérifiez que les six
  valeurs de `firebaseConfig` sont bien remplacées (pas de
  `__FIREBASE_..._​__` restant) et que Firestore est bien créé (étape 1).
- **Page blanche après déploiement** → ouvrez la console du navigateur
  (F12) pour voir le message d'erreur exact, généralement lié à
  `firebaseConfig` mal renseigné.
