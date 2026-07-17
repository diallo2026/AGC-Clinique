# AGC — Application de gestion de clinique (100 % locale)

## Fonctionnement général

`clinique.html` fonctionne entièrement dans le navigateur, sans serveur ni
connexion externe. Toutes les données sont enregistrées via le stockage local
de l'appareil utilisé (pas de synchronisation entre appareils).

## Nouveautés de cette version

### 1. Paramètres de la clinique
Onglet **Paramètres** : nom, logo, téléphone, email, adresse, et deux codes
d'accès dédiés :
- **Code Médecin** : déverrouille l'onglet Consultation.
- **Code Chef Labo / Fondateur** : déverrouille la gestion du catalogue
  d'examens en Laboratoire.

Si ces codes sont laissés vides, le code d'accès principal de la clinique
(saisi à la création) sert de code par défaut pour ces deux accès.

Ces informations (sauf le logo) sont aussi demandables dès la création de la
clinique (bouton "🏢 Créer ma clinique").

### 2. Couleur de la barre latérale
Si un logo est ajouté (à la création ou dans Paramètres), la couleur de la
barre latérale s'adapte automatiquement à la couleur dominante du logo. Sans
logo, elle reste bleue par défaut, avec le reste de l'interface en blanc.

### 3. Fiche patient enrichie
Chaque patient a désormais : prénom, nom, date de naissance, **sexe**,
téléphone, **adresse**, **personne responsable** et son **contact**.

### 4. Parcours patient en 3 étapes

**a. Accueil** — Enregistre un nouveau patient (ou sélectionne un patient
existant) et le place automatiquement dans la file d'attente du jour.

**b. Consultation** (accès médecin, protégé par le Code Médecin) — Affiche la
liste des patients du jour. Le médecin peut :
- **Appeler** un patient en attente,
- le marquer **Consulté** (avec un résumé de la consultation) ou **Non
  consulté**,
- imprimer un **rapport journalier en PDF A4** (bouton 🖨️, utilise
  l'impression du navigateur — choisir "Enregistrer en PDF" comme
  imprimante).

**c. Laboratoire** — Le technicien tape uniquement l'**ID du patient**
(format `JJMMAA-N`, généré automatiquement à l'Accueil) pour retrouver ses
informations. Le catalogue d'examens contient une valeur de référence
distincte pour Homme adulte / Femme adulte / Homme enfant / Femme enfant,
affichée automatiquement selon le patient sélectionné. **Ajouter ou modifier
le catalogue est réservé au Chef Labo et au Fondateur** (code dédié) ; tout
le personnel de labo peut en revanche enregistrer des résultats.

**d. Pharmacie** — Le pharmacien tape l'ID du patient pour le retrouver,
compose l'ordonnance (médicaments + quantités depuis le stock), enregistre
l'ordonnance et la facture correspondante en un clic, et peut imprimer un
**rapport journalier en PDF A4** de toutes les ventes du jour.

## Identifiants patients

Chaque nouveau patient reçoit un identifiant basé sur sa date d'enregistrement
(`JJMMAA-N`, ex. `170726-1`), généré à l'Accueil ou dans l'onglet Patients.
Cet identifiant est ensuite utilisé pour le retrouver rapidement en
Laboratoire, en Pharmacie ou lors de la prise de rendez-vous.

## Licence d'essai et Espace Concepteur

Chaque clinique créée démarre un **essai de 60 jours**. Passé ce délai, elle
est **automatiquement bloquée** à la prochaine connexion : l'utilisateur voit
alors un écran l'informant que sa licence doit être réglée, avec vos
coordonnées (WhatsApp 623030403, souleymane0403@gmail.com).

Depuis **🔧 Espace Concepteur** (bas de la page d'accueil, code maître
`AGC-CONCEPTEUR-2026` à personnaliser), vous pouvez :
- voir le nombre de cliniques créées **sur cet appareil**,
- voir le statut de la clinique active (active / essai expiré / bloquée) et
  son nombre de jours restants,
- la **bloquer** ou la **débloquer** manuellement (débloquer relance un
  essai de 60 jours).

**Limite importante à connaître :** l'application n'a pas de serveur central.
L'Espace Concepteur ne voit que les cliniques créées sur l'appareil/navigateur
où vous l'ouvrez — pas celles de vos clients installées sur leurs propres
appareils. Pour une vraie supervision à distance de toutes vos cliniques
clientes (blocage à distance inclus), il faudrait une base en ligne partagée
(ex. Supabase) ; dites-le-moi si vous voulez qu'on la mette en place.

## Code d'accès clinique et lien de connexion directe

Chaque clinique reçoit désormais, en plus de son code PIN :
- un **code clinique** unique (8 caractères, ex. `K7P2M9QX`),
- un **lien de connexion directe** (`clinique.html?access=K7P2M9QX`) qui,
  ouvert sur le même appareil où cette clinique a été créée, y reconnecte
  automatiquement sans ressaisir le PIN.

Ces deux informations s'affichent en résumé juste après la création de la
clinique. Depuis **l'Espace Concepteur**, vous pouvez à tout moment retrouver
le PIN et le lien de n'importe quelle clinique créée sur cet appareil
(boutons "PIN" / "Lien" dans le tableau), pour les renvoyer au client
concerné, ainsi que la bloquer ou la débloquer individuellement.

**Limite technique à connaître :** le lien ne fonctionne que sur l'appareil
où la clinique existe déjà en local (il ne transporte pas les données elles-
mêmes, seulement une référence vers celles stockées localement). Envoyer ce
lien à quelqu'un sur un autre ordinateur ou téléphone ne lui donnera pas
accès à la clinique — pour cela, il faudrait une base en ligne partagée.

## Connexion en deux étapes obligatoires

"Se connecter" demande maintenant, dans l'ordre :
1. **Le code clinique** (identifie précisément quelle clinique de cet appareil
   ouvrir) — si vous arrivez via le lien de connexion directe, cette étape
   est automatiquement validée avec le code du lien.
2. **Le code d'accès du responsable** (le PIN) — toujours demandé
   manuellement, y compris en passant par le lien : le lien identifie la
   clinique mais ne remplace jamais le code du responsable.

Si la clinique est bloquée (licence expirée), le message de blocage
s'affiche dès l'étape 1, avant même de demander le code du responsable.

## Sauvegarde des données

Comme il n'y a pas de serveur, vider le cache du navigateur efface les
données. Il n'existe pas encore de fonction d'export/import — dites-le-moi si
vous en avez besoin.

## Personnalisation avant diffusion

Dans `clinique.html`, remplacez le texte `[Votre nom / studio]` et les liens
de contact en bas de la page d'accueil, ainsi que `CODE_CONCEPTEUR`
(recherchez `AGC-CONCEPTEUR-2026`) par votre propre code maître.
