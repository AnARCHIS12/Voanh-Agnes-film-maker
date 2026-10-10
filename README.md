```markdown
# CINÉ-GENESIS — Studio de films IA

## À quoi sert ce script

CINÉ-GENESIS est une application web autonome (un seul fichier `index.html`, aucune
installation serveur, aucune base de données externe) qui permet de créer un
court-métrage complet généré par intelligence artificielle, étape par étape :

1. **Clés API** — vous enregistrez vos clés d'API IA (le script gère plusieurs clés
   et répartit automatiquement les requêtes pour respecter les limites de débit).
2. **Genre** — vous choisissez le style de film (fiction, documentaire, anime,
   cyberpunk, bande-annonce, etc.).
3. **Synopsis** — l'IA rédige l'histoire.
4. **Personnages** — l'IA crée les fiches et génère une image de référence pour
   chaque personnage (le visage et le costume restent cohérents dans tout le film).
5. **Scènes** — décors, ambiance, dialogues, image d'ambiance par scène.
6. **Plans** — découpage plan par plan (type de plan, action, caméra, réplique),
   avec image par plan.
7. **Vidéos** — chaque plan devient une vidéo courte, assemblées en séquence
   jouable avec sous-titres.

L'application gère aussi : la sauvegarde automatique du projet, la reprise après
interruption, la file d'attente avec limites de requêtes, la modification d'une
image par simple demande en français, l'import de vos propres photos, et un
journal technique pour diagnostiquer les erreurs d'API.

## Le principe « tiroir dans le tiroir » (données imbriquées)

Le script organise les données comme des tiroirs imbriqués, dans la base
IndexedDB du navigateur (avec repli automatique sur localStorage si IndexedDB
est indisponible) :

```
PROJET (le meuble)
└── Session  (1er tiroir : le film entier, son genre, son ratio, son synopsis)
    └── Personnages  (sous-tiroirs : fiche + image de référence de chaque personnage)
    └── Scènes      (sous-tiroirs : décor, ambiance, dialogues, image de la scène)
        └── Plans   (sous-tiroirs dans la scène : action, caméra, image, vidéo)
```

Chaque élément vit à l'intérieur de son parent :

- Une **vidéo** appartient à un **plan**.
- Un **plan** appartient à une **scène**.
- Une **scène** et les **personnages** appartiennent au **film**.

L'intérêt : quand l'IA génère l'image d'un plan, elle récupère automatiquement,
en remontant les tiroirs, les images de référence des personnages présents dans
ce plan et l'image de la scène. C'est ce qui garantit que le même personnage a
le même visage d'un plan à l'autre. Sauvegarder, modifier ou supprimer un
élément ne touche jamais le reste du meuble : tout est enregistré et rechargé
tel quel à la réouverture de la page.

## Installation assistée par l'IA

Ce type d'application n'exige pas de configuration manuelle fastidieuse. Au
premier lancement, on laisse l'IA construire elle-même la structure du projet :

1. Vous ouvrez le fichier dans un navigateur et entrez vos clés API.
2. Vous répondez à quelques questions du formulaire (genre, format, thème).
3. L'IA remplit elle-même toutes les cases : elle rédige le synopsis, crée les
   personnages avec leur fiche physique, écrit les scènes, découpe les plans.
4. Tout ce que l'IA a construit est écrit automatiquement dans les tiroirs de
   la base IndexedDB (objet `sessions` : le film entier avec ses personnages,
   scènes et plans imbriqués).
5. Une fois enregistré, le projet est immédiatement utilisable : à chaque
   réouverture de la page, le dernier projet se recharge tout seul. Vous
   pouvez modifier n'importe quel champ à la main, l'IA ne le refera pas.

Autrement dit : l'installation ne se configure pas, elle se construit. La
première session d'utilisation est l'installation ; les suivantes n'ont plus
besoin d'IA pour fonctionner, seulement pour générer des images et des vidéos.

## Cinq exemples d'utilisation de l'IA à l'installation de ce type de logiciel

1. **Onboarding automatique** : l'IA pose des questions en langage naturel
   (« quel genre de film voulez-vous ? ») et transforme les réponses en
   configuration technique complète du projet.
2. **Génération de contenu initial** : au lieu de formulaires vides, l'IA
   pré-remplit synopsis, personnages, scènes et plans ; l'utilisateur n'a
   plus qu'à corriger.
3. **Analyse de documents fournis** : on importe une photo ou un texte, l'IA
   le décrit et en fait une fiche de personnage ou un décor directement
   enregistrés dans la base.
4. **Création des gabarits de requêtes** : l'IA construit elle-même les
   prompts de génération d'images à partir des données du projet, l'utilisateur
   n'écrit jamais de prompt technique.
5. **Diagnostic d'installation** : en cas d'erreur d'API (clé invalide, quota
   atteint), l'IA explique le problème dans le journal et propose l'action
   correctrice (désactiver la clé, réessayer, etc.).

## Cinq exemples de reconversion de l'application pour d'autres usages

1. **Studio de musique IA** : remplacer « scènes » par « couplets » et « plans »
   par « mesures », avec génération de pochettes d'album et de clips.
2. **Générateur de romans illustrés** : personnage = protagoniste, scène =
   chapitre, plan = illustration ; la même logique de tiroirs garde la
   cohérence des personnages d'un chapitre à l'autre.
3. **Agence de storytelling de marque** : le pipeline produit des publicités
   ou bande-annonces produit, l'utilisateur décrivant la marque à l'installation.
4. **Outil pédagogique** : créer des séquences éducatives (leçon → section →
   illustration animée), l'IA construisant le plan de cours au premier lancement.
5. **Storyboard de jeu vidéo** : scènes = niveaux, plans = cinématiques,
   avec cohérence visuelle des personnages du concept à la cinématique.

## Déploiement

Aucun backend requis : mettez simplement le fichier `index.html` sur
n'importe quel hébergement statique (GitHub Pages, Hostinger, etc.) et
ouvrez-le dans un navigateur moderne. Toutes les données restent locales,
dans le navigateur de l'utilisateur.
```
