# Kerogo Tools Academy

L'académie IA du groupe Kerogo : les bons réflexes IA, 5 minutes par jour.

Les collaborateurs suivent un parcours de 6 niveaux de 4 missions courtes chacun (24 missions), à suivre à leur rythme. Avant tout, ils lisent et signent la charte d’utilisation de l’IA (étape 0). Ils font aussi le quiz du mois, s’inscrivent aux webinaires, consultent le catalogue Kerogo Tools, gagnent des XP pour leur site et téléchargent leurs documents (accusé de réception de la charte, certification du niveau 1) depuis leur profil. L'équipe Dev Ops pilote le tout depuis l'espace admin, et les managers suivent leurs équipes.

**Adresse :** https://dev-ops-kerogo.github.io/kerogo-tools-academy/

Projet porté par l'équipe Dev Ops du groupe Kerogo.

## Le parcours

| Niveau | Thème | Badge | État |
|---|---|---|---|
| 1 | Les bons réflexes | Découvreur | 4 missions et 4 capsules vidéo prêtes |
| 2 | Bien utiliser Slackbot | Utilisateur | 4 missions et 4 capsules vidéo prêtes |
| 3 | Les compétences Kerogo Tools | Efficace | 4 missions et 4 capsules vidéo prêtes |
| 4 | Automatiser | Autonome | 4 missions et 4 capsules vidéo prêtes |
| 5 | L'IA dans ton métier | Expert | 4 missions et 4 capsules vidéo prêtes |
| 6 | Créateurs | Créateur | 4 missions et 4 capsules vidéo prêtes |

- Les missions se font dans l'ordre : chacune s'ouvre quand la précédente est validée.
- Chaque niveau s'ouvre à la date réglée dans l'admin (onglet **Missions** > « Calendrier des niveaux »). Avant cette date, il reste visible avec « Ouvre le … ». Sans date, il s'ouvre dès que ses missions sont prêtes.
- Une mission validée rapporte 50 XP, un niveau terminé 100 XP et un badge. Le quiz du mois rapporte 10 XP par bonne réponse, et un webinaire suivi 30 XP.
- **Étape 0 · La charte** : la charte officielle (Google Doc) se lit dans l'Académie, en plusieurs fois si besoin. Chaque section compte quand elle a été lue le temps nécessaire, puis la personne signe. Elle ne bloque pas les missions, mais elle est nécessaire pour le badge Découvreur et la certification du niveau 1. Une nouvelle version de la charte demande une nouvelle signature.
- **Documents** : l'accusé de réception de la charte (un par version signée) et la certification du niveau 1 sont produits une seule fois, archivés dans le Drive partagé « Automatisations », et téléchargeables depuis **Mon profil**. L'admin (onglet **Attestations**) les liste pour un audit. La liste de l'équipe se télécharge en PDF depuis **Mon équipe**.
- **Catalogue** et **Webinaires** : le catalogue Kerogo Tools (liste Slack) et le programme des webinaires (agenda), avec l'inscription en un clic.

## Comment ça fonctionne

| Élément | Rôle | Où il se trouve |
|---|---|---|
| Page de l'Académie | Ce que voient les collaborateurs (accueil, missions, classement, profil, admin) | Ce dépôt : `index.html`, publié par GitHub Pages |
| Connexion | Compte Google professionnel (Google Identity Services) | Projet Google Cloud « Kerogo Tools Academy » |
| Serveur | Vérifie l'identité, lit et enregistre la progression, publie dans Slack | Google Apps Script (`Code.gs`), déployé en application web |
| Données | Collaborateurs, progression, quiz, calendrier, retours, documents | Tableur Google « Kerogo Tools Academy — suivi » (les actions des collaborateurs arrivent dans l'onglet « Écritures », reportées toutes les 5 minutes) |
| Charte | Le texte officiel, lu par le serveur | Google Doc de la charte (réglable dans l'admin) |
| Documents archivés | Accusés de réception et certifications en PDF | Drive partagé « Automatisations » |
| Messages Slack | Publications dans #kerogo-tools, relances, invitations | Application Slack « Kerogo Tools Academy » |
| Rédaction automatique | Brouillons mensuels (article, focus compétence, quiz) | API Claude, appelée par le serveur |

La page ne contient **aucune donnée** ni aucun secret. Le navigateur envoie le jeton de connexion Google au serveur Apps Script, qui le vérifie (identifiant client et domaine de l'adresse) avant de répondre. L'identifiant client et l'adresse du serveur présents dans `index.html` sont publics par nature.

## Mettre à jour la page

1. Dans ce dépôt, cliquer sur **Add file** puis **Upload files**.
2. Déposer le nouveau `index.html` (le nom doit être exactement celui-ci).
3. Choisir **Commit directly to the `main` branch**, puis **Commit changes**.
4. Attendre une minute : GitHub Pages publie la nouvelle version.

La version affichée en bas des messages d'erreur (« Version de la page ») permet de vérifier que la bonne version est en ligne. Le fichier vide `.nojekyll` indique à GitHub de publier la page telle quelle.

## Mettre à jour le serveur

1. Ouvrir le projet Apps Script, remplacer `Code.gs` et `Index.html`.
2. **Déployer → Gérer les déploiements**, puis publier une nouvelle version du déploiement « API version hébergée » (accès : tout le monde), utilisé par cette page.
3. Si une nouvelle version demande de nouvelles autorisations Google, lancer une fonction depuis l'éditeur (par exemple `getCharte`) et les accepter **avant** de publier le déploiement.

Les réglages sensibles sont dans les **propriétés du script** (Paramètres du projet), jamais dans le code :

| Propriété | Contenu |
|---|---|
| `CLIENT_ID_GOOGLE` | Identifiant client OAuth du projet Google Cloud |
| `URL_APP` | Adresse de l'Académie utilisée par les boutons Slack |
| `SLACK_ACADEMIE_TOKEN` | Jeton du bot Slack |
| `ANTHROPIC_API_KEY` | Clé de l'API Claude pour la rédaction automatique |
| `SLACK_WEBINAR_TOKEN` | Jeton de l'app Slack des webinaires (inscriptions dans la liste « Inscriptions Kerogo Tools webinar ») |
| `ORGANIGRAMME_ID` | Identifiant de l'organigramme |

Les administrateurs, managers et domaines autorisés se gèrent dans l'espace admin, onglet **Accès**. Les dates d'ouverture des niveaux se règlent dans l'onglet **Missions**.

`index.html` (ce dépôt) et `Index.html` (Apps Script) sont la même page : la première se connecte avec Google, la seconde passe par Apps Script. Une modification de la page se reporte dans les deux fichiers, qui sont livrés ensemble.

## Ajouter une mission

Toutes les missions à partir du niveau 2 suivent le même modèle : les notions à comprendre, un exemple avant/après si besoin, un exercice corrigé, puis 2 ou 3 questions.

1. **Contenu** : une fiche dans `MISSIONS_DEF` (page), avec le titre, l'introduction, les notions, l'exercice, les questions et le seuil de réussite. Ajouter son identifiant dans `MISSIONS_JOUABLES`.
2. **Serveur** : dans `MISSIONS` (`Code.gs`), passer la mission à `dispo: true` et renseigner `seuil` et `total` (bonnes réponses à l'exercice + aux questions). Le seuil doit être le même des deux côtés.
3. **Vidéo** : exporter la capsule en MP4, la déposer sur Google Drive (partagée en lecture avec le groupe), puis coller son lien dans l'admin, onglet **Missions**.
4. **Tester** : dans l'onglet **Missions**, le bouton « Tester » ouvre n'importe quelle mission, même verrouillée. Le quiz du mois se teste aussi depuis son onglet, brouillons compris. En test, rien n'est enregistré.

## Ajouter une adresse d'hébergement

Si la page est publiée à une nouvelle adresse, l'ajouter dans Google Cloud : **API et services → Identifiants → client « Académie » → Origines JavaScript autorisées**. On saisit l'origine seule, sans chemin ni barre oblique finale (par exemple `https://dev-ops-kerogo.github.io`). La prise en compte demande quelques minutes.

## Accès des collaborateurs

- Seules les adresses des domaines autorisés (onglet Accès de l'admin) peuvent se connecter.
- Un domaine géré dans une autre organisation Google demande que l'écran de connexion OAuth du projet Google Cloud soit en mode « Externe ».
- Un collaborateur bloqué voit le message « Cette adresse n'a pas encore accès à l'Académie » avec le domaine concerné : il suffit d'ajouter ce domaine dans l'admin.

## Relances Slack

- Charte non signée 4 jours ouvrés après l'inscription, puis une fois par semaine.
- Alerte aux administrateurs : charte non signée 14 jours après la date de départ (lancement de l'Académie, nouvelle version de la charte ou arrivée dans l'organigramme). Chaque jour ouvré à 9 h s'il y a de nouveaux cas, et un récapitulatif le lundi. Le bouton « Relancer » de l'admin écrit à la personne et à son manager.
- Aucune visite depuis 4 jours ouvrés, puis une fois par semaine.
- Les week-ends et les jours fériés ne comptent pas : ceux de France, de Tunisie, de Madagascar ou de l'Île Maurice selon le site du collaborateur.
- En mode test (onglet **Réglages**), relances, invitations et publications partent uniquement vers l'adresse de test.

## Fichiers du dépôt

| Fichier | Rôle |
|---|---|
| `index.html` | La page de l'Académie |
| `README.md` | Ce document |
| `.nojekyll` | Indique à GitHub Pages de publier la page telle quelle |
| `favicon-chapeau-64.png` | L'icône d'onglet (le chapeau du logo), utilisée par la version Apps Script |

## En cas de problème

| Symptôme | Piste |
|---|---|
| La fenêtre Google affiche « origin_mismatch » | L'adresse de la page n'est pas dans les origines JavaScript autorisées |
| « Jeton non reconnu » | `CLIENT_ID_GOOGLE` absent ou différent de celui de la page |
| La page charge sans fin | Le déploiement « API version hébergée » n'est pas à jour ou n'est pas ouvert à « tout le monde » |
| Les boutons Slack ouvrent l'ancienne adresse | Mettre à jour la propriété `URL_APP` |
| Une mission reste « Verrouillée » | La mission précédente n'est pas validée, ou le niveau n'est pas encore ouvert (calendrier) |
| Un manager ne voit pas « Mon équipe » | Il n'a personne sous sa responsabilité dans l'organigramme, ou il n'est pas encore relié à sa fiche (voir l'onglet **Réglages**) |
| Un collaborateur n'apparaît pas dans l'équipe de son manager | Il n'est pas relié à sa fiche de l'organigramme : le bilan du rapprochement est dans l'onglet **Réglages** |

Une question : #kerogo-tools.
