# CINÉ-GENESIS

Studio de création de courts-métrages assisté par intelligence artificielle, dans
une application web autonome.

CINÉ-GENESIS accompagne la création d'un film depuis l'idée initiale jusqu'aux
plans vidéo : synopsis, personnages, scènes, storyboard, images de référence et
séquences animées. L'interface tient dans un seul fichier `index.html` et ne
nécessite aucun serveur applicatif.

## Fonctionnalités

- parcours guidé de l'idée au film ;
- génération du synopsis, des personnages, des scènes et des plans ;
- création et modification d'images avec références visuelles ;
- génération vidéo plan par plan ;
- prise en charge de plusieurs clés API et répartition des requêtes ;
- sauvegarde automatique et reprise des projets dans le navigateur ;
- import facultatif de photos avec confirmation avant leur analyse ;
- lecture du film en vidéo ou en animatique ;
- export du projet au format JSON ;
- journal technique intégré pour diagnostiquer les erreurs.

## API requise

Cette application utilise exclusivement l'API **Agnès AI**. Une clé OpenAI,
Anthropic, Google Gemini ou provenant d'un autre fournisseur ne fonctionnera pas.

Pour obtenir une clé :

1. ouvrez le [site officiel d'Agnès AI](https://agnes-ai.com/) ;
2. choisissez l'accès **Free API** ou **Access API** proposé par Agnès ;
3. connectez-vous ou créez un compte ;
4. créez ou copiez une clé API depuis l'espace développeur Agnès ;
5. collez cette clé dans le premier écran de CINÉ-GENESIS.

Les offres, quotas, tarifs et noms de modèles peuvent évoluer. Consultez le site
d'Agnès avant utilisation. Ne publiez jamais votre clé dans le dépôt, une capture
d'écran, un rapport de bug ou un fichier exporté.

### Configuration technique actuelle

Le fichier `index.html` est déjà configuré pour utiliser :

- API principale : `https://apihub.agnes-ai.com/v1` ;
- suivi des tâches vidéo : `https://apihub.agnes-ai.com/agnesapi` ;
- authentification : en-tête `Authorization: Bearer VOTRE_CLE` ;
- texte : `agnes-3.0-flash`, avec repli sur `agnes-2.5-flash` ;
- image : `agnes-image-2.5-flash` ;
- vidéo avec clé gratuite : `agnes-video-v2.0` ;
- vidéo avec Token Plan : `agnes-video-2.5-flash`.

Routes appelées par l'application :

- `POST /v1/chat/completions` pour l'écriture et l'analyse ;
- `POST /v1/images/generations` pour les images ;
- `POST /v1/videos` pour lancer une génération vidéo ;
- `GET /agnesapi?video_id=...&model_name=...` pour suivre son avancement.

Il n'y a rien à renseigner dans le code pour un usage normal : la clé est saisie
directement dans l'interface. Les constantes d'API et de modèles se trouvent au
début du script dans `index.html` si une évolution du service impose leur mise à
jour.

## Démarrage rapide

### Option 1 : utilisation directe

1. téléchargez le dépôt ;
2. ouvrez `index.html` dans un navigateur moderne ;
3. collez une ou plusieurs clés API Agnès ;
4. lisez l'avis de confidentialité et confirmez-le ;
5. choisissez le type de clés : gratuites ou Token Plan ;
6. suivez les étapes affichées à l'écran.

### Option 2 : serveur local

Servir le dossier en HTTP local évite les restrictions que certains navigateurs
appliquent aux fichiers ouverts directement. Par exemple, depuis le dossier du
projet :

```bash
python3 -m http.server 8080
```

Ouvrez ensuite `http://localhost:8080`.

## Parcours de création

1. **Connexion** : saisie des clés Agnès et choix du type d'abonnement.
2. **Format** : sélection du genre et du ratio d'image.
3. **Synopsis** : rédaction libre ou amélioration par l'IA.
4. **Personnages** : fiches, costumes et portraits de référence.
5. **Scènes** : décors, ambiance, dialogues et images principales.
6. **Plans** : découpage cinématographique et images de départ.
7. **Vidéos** : génération, suivi de la file d'attente et lecture du film.

Chaque résultat reste modifiable avant de passer à l'étape suivante.

## Stockage et architecture

CINÉ-GENESIS ne possède ni backend applicatif ni base de données distante propre.
Les projets sont stockés dans `IndexedDB`, avec un repli sur `localStorage` lorsque
`IndexedDB` n'est pas disponible.

```text
Projet
├── paramètres, genre et synopsis
├── personnages
│   └── fiche et image de référence
└── scènes
    ├── décor, ambiance, dialogues et image
    └── plans
        └── action, caméra, réplique, image et vidéo
```

Cette structure imbriquée permet de réutiliser automatiquement les références
des personnages et des scènes afin d'améliorer la cohérence visuelle d'un plan à
l'autre.

## Confidentialité

Le fonctionnement n'est pas entièrement local. Dès qu'une fonction IA est
utilisée, les données nécessaires sont transmises à Agnès AI, notamment :

- idées, synopsis, dialogues et descriptions ;
- prompts techniques construits par l'application ;
- images de référence et images servant à la génération vidéo ;
- photos importées lorsque leur analyse est confirmée.

Les vidéos peuvent rester disponibles au moyen d'une URL distante fournie par
Agnès. Consultez les conditions d'utilisation et la politique de confidentialité
du fournisseur avant d'envoyer des données personnelles, des images de tiers ou
des œuvres confidentielles.

Les clés API restent en mémoire dans l'onglet par défaut. Leur mémorisation dans
le navigateur est facultative et peut être annulée avec le bouton **Effacer les
clés**. Les clés sont séparées des projets et retirées des exports JSON.

## Sécurité

- n'utilisez ce projet que sur un appareil et un hébergement de confiance ;
- évitez les postes partagés si vous choisissez de mémoriser une clé ;
- révoquez immédiatement une clé exposée ou suspecte ;
- vérifiez les coûts et quotas Agnès avant une génération importante ;
- conservez une copie des projets exportés si le navigateur doit être réinitialisé.

L'application applique une politique de sécurité du contenu, refuse les URL de
médias non chiffrées et bloque les destinations locales ou privées les plus
courantes. Comme tout client web sans backend, elle ne peut toutefois pas rendre
une clé totalement secrète pendant son utilisation dans le navigateur.

## Déploiement

Le dépôt peut être publié sur un hébergement statique de confiance, par exemple
GitHub Pages. Aucun secret ne doit être inscrit dans `index.html` : chaque
utilisateur fournit sa propre clé dans son navigateur.

Avant une mise en production, vérifiez que l'API Agnès autorise les requêtes CORS
depuis le domaine choisi et que les conditions du fournisseur conviennent à votre
usage.

## Dépannage

### La clé est refusée

Vérifiez qu'il s'agit bien d'une clé Agnès, qu'elle est encore active et que le
type sélectionné dans l'application correspond à votre offre.

### La génération reste en attente

La file vidéo peut être saturée ou le quota atteint. Gardez l'onglet ouvert,
consultez le journal technique et utilisez **Tester maintenant** ou **Reprendre la
production** lorsque l'interface le propose.

### Rien n'est sauvegardé

Autorisez le stockage du site dans le navigateur. Les modes privés, les politiques
d'entreprise et certains nettoyeurs peuvent supprimer `IndexedDB` et
`localStorage`.

### Une photo ne peut pas être analysée

Confirmez explicitement son envoi à Agnès et vérifiez qu'elle utilise un format
d'image pris en charge par le navigateur.

## Structure du dépôt

```text
.
├── index.html   Application complète : interface, styles et logique
└── README.md    Documentation du projet
```

## Compatibilité

Un navigateur récent est requis. L'application utilise notamment `IndexedDB`,
`fetch`, les URL d'objet, la synthèse vocale du navigateur et les éléments vidéo
HTML5.

## Licence

Aucun fichier de licence n'est actuellement fourni. En l'absence de licence
explicite, la consultation du code ne vaut pas autorisation de le copier, le
modifier ou le redistribuer.
