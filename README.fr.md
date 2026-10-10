<div align="center">
  <img src="./assets/logo.svg" width="760" alt="CINÉ-GENESIS — studio de films IA">
  <p>
    <img src="./assets/badges/status.svg" alt="Statut : actif">
    <img src="./assets/badges/version.svg" alt="Version : 1.0.0">
    <img src="./assets/badges/license.svg" alt="Licence : AGPL-3.0-or-later">
    <img src="./assets/badges/browser.svg" alt="Compatibilité : navigateurs modernes">
  </p>
  <p><a href="./README.md">Esperanto</a> · <strong>Français</strong> · <a href="./README.en.md">English</a> · <a href="./README.es.md">Español</a></p>
</div>

# CINÉ-GENESIS

Application web autonome pour créer des courts-métrages avec l'intelligence
artificielle, depuis l'idée initiale jusqu'aux images et plans vidéo.

## Fonctionnalités

| Fonction | Description |
|---|---|
| Création guidée | Sept étapes structurées, du genre au film final |
| Cohérence visuelle | Références partagées entre personnages, scènes et plans |
| Génération vidéo | Production plan par plan avec suivi automatique |
| Reprise du travail | Sauvegarde locale automatique après interruption |
| Plusieurs clés | Répartition des requêtes entre plusieurs clés API |
| Confidentialité | Clés limitées à l'onglet par défaut et exports nettoyés |
| Projet portable | Export complet au format JSON |

## API requise

CINÉ-GENESIS utilise exclusivement l'API [Agnès AI](https://agnes-ai.com/).
Une clé OpenAI, Anthropic, Google Gemini ou provenant d'un autre fournisseur ne
fonctionnera pas.

1. Ouvrez le site officiel d'Agnès AI.
2. Choisissez **Free API** ou **Access API**.
3. Créez ou copiez une clé depuis l'espace de développement.
4. Collez-la dans le premier écran de CINÉ-GENESIS.

Les offres, quotas, tarifs et modèles peuvent évoluer. Vérifiez les conditions
actuelles d'Agnès avant une génération importante.

## Démarrage rapide

Une seule commande clone et sert l'application localement :

```bash
git clone https://github.com/AnARCHIS12/Voanh-Agnes-film-maker.git && cd Voanh-Agnes-film-maker && python3 -m http.server 8080
```

Ouvrez ensuite `http://localhost:8080` dans un navigateur moderne.

## Parcours de création

1. **Connexion** — saisie des clés Agnès et choix du type d'offre.
2. **Format** — sélection du genre et du ratio d'image.
3. **Synopsis** — rédaction libre ou amélioration par l'IA.
4. **Personnages** — fiches, costumes et portraits de référence.
5. **Scènes** — décors, ambiance, dialogues et images principales.
6. **Plans** — découpage cinématographique et images de départ.
7. **Vidéos** — génération, suivi et lecture de la séquence complète.

Chaque résultat reste modifiable avant l'étape suivante.

## Configuration technique

| Usage | Valeur |
|---|---|
| API principale | `https://apihub.agnes-ai.com/v1` |
| Suivi vidéo | `https://apihub.agnes-ai.com/agnesapi` |
| Authentification | `Authorization: Bearer VOTRE_CLE` |
| Texte | `agnes-3.0-flash`, puis `agnes-2.5-flash` |
| Image | `agnes-image-2.5-flash` |
| Vidéo gratuite | `agnes-video-v2.0` |
| Vidéo Token Plan | `agnes-video-2.5-flash` |

Routes utilisées : `POST /v1/chat/completions`,
`POST /v1/images/generations`, `POST /v1/videos` et
`GET /agnesapi?video_id=...&model_name=...`.

## Architecture

L'application n'a pas de backend propre. L'interface, les styles et la logique
résident dans `index.html`. Les projets sont stockés dans `IndexedDB`, avec
`localStorage` comme solution de repli.

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

```text
.
├── assets/
│   ├── badges/          Badges SVG locaux
│   └── logo.svg         Identité visuelle du projet
├── index.html           Application complète
├── LICENSE              AGPL-3.0-or-later
├── README.md            Espéranto
├── README.fr.md         Français
├── README.en.md         Anglais
└── README.es.md         Espagnol
```

## Confidentialité et sécurité

Les projets restent dans le navigateur, mais les fonctions IA ne sont pas
locales. Les textes, dialogues, descriptions, références visuelles et photos
confirmées nécessaires sont transmis à Agnès AI. Les vidéos peuvent rester
accessibles par une URL distante.

- Les clés restent dans l'onglet par défaut.
- Leur mémorisation locale est facultative et révocable.
- Elles sont retirées des exports JSON.
- Les URL non chiffrées ou manifestement locales sont refusées.
- Une politique de sécurité du contenu limite les capacités de la page.

N'envoyez pas de données personnelles, d'œuvres confidentielles ou d'images de
tiers sans droit approprié et consentement éclairé.

## Compatibilité

| Environnement | Prise en charge | Remarques |
|---|---|---|
| Navigateurs Chromium | Complète | Recommandés pour la vidéo et la synthèse vocale |
| Firefox | Complète | Les voix disponibles dépendent du système |
| Safari | Version récente | Vérifier les restrictions de lecture automatique |
| Navigateurs mobiles | Interface responsive | Garder l'onglet ouvert pendant les générations longues |
| Hors connexion | Partielle | L'édition fonctionne, la génération nécessite Agnès |

## Contribution

1. Créez un fork puis une branche ciblée.
2. Conservez le projet sans CDN, télémétrie ni secret.
3. N'ajoutez aucun emoji dans l'interface, la documentation ou les commits.
4. Vérifiez la syntaxe JavaScript et le rendu responsive.
5. Utilisez un commit conventionnel, par exemple `fix: protect exported API keys`.
6. Ouvrez une proposition de fusion avec les contrôles effectués.

## Licence

Ce projet est un logiciel libre distribué sous
[GNU Affero General Public License v3.0 ou ultérieure](./LICENSE). Toute version
modifiée accessible par réseau doit également proposer son code source
correspondant selon les conditions de la licence.
