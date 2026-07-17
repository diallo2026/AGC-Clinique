# AGC — Application de gestion de clinique

Application 100 % locale (aucun serveur, aucune base de données externe).
Toutes les données sont stockées dans le navigateur de l'appareil qui
utilise l'application (`localStorage`). Le fichier à déployer est
`public/index.html`.

---

## 1. Déployer sur GitHub

1. Créez un nouveau dépôt sur [github.com/new](https://github.com/new)
   (par exemple `agc-clinique`), sans README ni licence (pour éviter les
   conflits avec les fichiers déjà présents ici).
2. Dans un terminal, à l'intérieur de ce dossier décompressé :
   ```bash
   git init
   git add .
   git commit -m "Version initiale AGC"
   git branch -M main
   git remote add origin https://github.com/VOTRE-COMPTE/agc-clinique.git
   git push -u origin main
   ```
   (Remplacez `VOTRE-COMPTE` et `agc-clinique` par votre nom d'utilisateur
   et le nom réel du dépôt créé à l'étape 1.)

À ce stade, le code est sur GitHub mais pas encore accessible comme site
web — c'est ce que fait Firebase Hosting à l'étape suivante.

---

## 2. Déployer sur Firebase Hosting

### Prérequis (une seule fois)
1. Installez Node.js si ce n'est pas déjà fait : [nodejs.org](https://nodejs.org)
2. Installez l'outil Firebase :
   ```bash
   npm install -g firebase-tools
   ```
3. Connectez-vous :
   ```bash
   firebase login
   ```

### Créer le projet Firebase
1. Allez sur [console.firebase.google.com](https://console.firebase.google.com)
2. **Ajouter un projet** → donnez-lui un nom (ex. `agc-clinique`) → suivez les
   étapes (vous pouvez désactiver Google Analytics, non nécessaire ici)
3. Une fois créé, notez l'**ID du projet** affiché dans les paramètres du
   projet (icône ⚙️ → Paramètres du projet) — il ressemble à
   `agc-clinique-a1b2c`

### Relier ce dossier à votre projet Firebase
1. Ouvrez le fichier `.firebaserc` de ce dossier et remplacez
   `REMPLACEZ-PAR-VOTRE-ID-PROJET-FIREBASE` par l'ID noté ci-dessus.
2. Dans un terminal, à l'intérieur de ce dossier :
   ```bash
   firebase deploy
   ```
3. À la fin, Firebase affiche une **Hosting URL** (ex.
   `https://agc-clinique-a1b2c.web.app`) — c'est le lien à partager avec vos
   clients.

### Mettre à jour le site après une modification
Chaque fois que vous modifiez `public/index.html` :
```bash
git add .
git commit -m "Mise à jour"
git push
firebase deploy
```

---

## Important à savoir sur l'hébergement

L'application reste **100 % locale au navigateur** même hébergée sur
Firebase : Firebase Hosting sert uniquement le fichier HTML, il ne stocke
aucune donnée de clinique. Chaque personne qui ouvre le lien Firebase a ses
propres données, stockées sur son propre appareil — il n'y a toujours **pas
de synchronisation entre appareils**. L'intérêt de l'hébergement Firebase
est uniquement de donner une **adresse web fixe et facile à partager**
(au lieu d'envoyer le fichier HTML directement), pas d'ajouter de base de
données partagée.

Si vous voulez un jour une vraie synchronisation entre appareils (plusieurs
utilisateurs voyant les mêmes données en temps réel), il faudra ajouter une
base de données en ligne (ex. Supabase ou Firebase Firestore) — ce qui est
un changement plus important que le simple hébergement statique fait ici.

---

## Documentation fonctionnelle de l'application

Voir `DOCUMENTATION.md` dans ce dossier pour le détail des fonctionnalités
(parcours patient, laboratoire, pharmacie, licence, espace concepteur, etc.).
