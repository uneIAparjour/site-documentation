# Agent.md — uneIAparjour.fr

## Identité du projet

**Nom** : Une IA par jour  
**URL** : https://www.uneiaparjour.fr  
**Auteur** : Bertrand Formet  
**Contact** : contact@uneiaparjour.fr · @bertrandformet sur les réseaux sociaux  
**Licence** : Creative Commons CC BY 4.0 (articles, catégorisation, sélection, base de données)  
**Hashtag** : #uneIAparjour  
**Lancement** : février 2023 (d'abord sur X/Twitter, puis site dédié depuis novembre 2023)  

## Mission

Proposer chaque jour un outil d'IA générative gratuit ou freemium, testé et documenté avec descriptif et captures d'écran. Le site est une ressource de veille indépendante destinée à quiconque souhaite découvrir et expérimenter des applications d'intelligence artificielle générative.

## Principes éditoriaux

### Accessibilité et simplicité
- Outils gratuits ou freemium avec crédits suffisants pour des tests à moyen terme, sans période d'essai restrictive.
- Inscription facilitée : pas de numéro de téléphone ou d'adresse professionnelle requis, une simple adresse email suffit.
- Outils disponibles en France sans VPN.
- Pas de clé d'API requise.

### Éthique et respect
- Outils respectueux de l'humain dans leurs générations (pas de violence, nudité, deepfakes).
- Pas d'outils « humanizers » (tromperie sur l'origine des contenus).
- Pas de « détecteurs de contenus IA » (fiabilité non garantie).
- Publicité absente ou discrète et non invasive.

### Transparence et authenticité
- Captures d'écran brutes, non retouchées (erreurs des outils visibles).
- Instructions récurrentes pour permettre la comparaison entre outils.
- Aucune collaboration commerciale, aucun avantage personnel.
- Tous les articles sont écrits sans assistance d'une IA générative.

---

## Architecture du site

### Plateforme technique
- **CMS** : WordPress (hébergement OVH)
- **Thème** : Kenta Artistic Blog (WP Moose)
- **Plugins notables** : Jetpack (partage social), Google Fonts (Montserrat)
- **Typographie** : Montserrat (via Google Fonts)
- **Feed RSS** : disponible (contient les ~1000 articles les plus récents)

### Structure des URLs
- Page d'accueil : `https://www.uneiaparjour.fr/`
- Articles paginés : `https://www.uneiaparjour.fr/page/{n}/` (11+ pages)
- Articles individuels : `https://www.uneiaparjour.fr/{slug-outil}/`
- Catégories : `https://www.uneiaparjour.fr/category/{slug-categorie}/`

### Menu principal (6 entrées)
| Page | URL | Description |
|------|-----|-------------|
| **Accueil** | `/` | Fil des articles (1 outil IA par jour), paginé |
| **Sélection** | `/selection/` | 60 outils choisis, classés en 10 catégories × 3 niveaux, PDF téléchargeable |
| **Aide au choix** | `/aide-au-choix/` | Arbre de décision interactif (réalisé avec Lovable) |
| **Lettre d'infos** | `/lettre-dinfos/` | Présentation de la newsletter hebdomadaire + archives récentes |
| **Lectures partagées** | `/lectures-partagees/` | Compilation des ressources de veille de la newsletter |
| **À propos du site** | `/a-propos-du-site/` | Principes éditoriaux, contact |

### Page accessible via le footer
| Page | URL | Description |
|------|-----|-------------|
| **Base du site** | `/base/` | Téléchargement de la base de données (.ods) + liens GitHub et Hugging Face |
| **Contact** | `/contact/` | Formulaire de contact |
| **Mentions et confidentialité** | `/mentions-et-confidentialite/` | Mentions légales |
| **Plan du site** | `/sitemap_index.xml` | Sitemap XML |

---

## Contenus et données

### Base des articles (contenu principal)
- **~1 120 outils** référencés (au 14/03/2026)
- **Période** : 16/02/2023 → aujourd'hui (publication quotidienne)
- **34 catégories** WordPress utilisées pour le classement
- **Format d'un article** : titre, URL de l'outil, description textuelle, captures d'écran, catégories, date de publication
- **Structure de la base de données** : fichier .ods avec colonnes titre / description / URL sur le site / catégories (jusqu'à 6) / date de publication  

### Distribution de la base de données (multi-plateforme)

| Canal | URL | Format | Rôle |
|-------|-----|--------|------|
| **Site** (page `/base/`) | `https://www.uneiaparjour.fr/base` | ODS | Téléchargement direct pour les lecteurs |
| **GitHub** | `https://www.github.com/uneIAparjour/base` | ODS + CSV | Versioning, automatisation, réutilisation technique |
| **Hugging Face** | `https://www.huggingface.co/datasets/uneiaparjour/base` | ODS + CSV | Exploration interactive (Dataset Viewer), API, communauté IA |

### Workflow de mise à jour de la base
1. Mise à jour du fichier `base-uneiaparjour.ods` localement
2. Téléversé sur **GitHub** (Add file → Upload files → le fichier écrase l'ancien)
3. Un **workflow GitHub Actions** génère automatiquement le CSV à partir de l'ODS
4. Le workflow **synchronise ensuite automatiquement vers Hugging Face** (ODS + CSV + README)
5. Mise à jour manuelle du fichier en téléchargement sur **WordPress** (`/base/`)

### Structure du dépôt GitHub `uneIAparjour/base`
```
📁 .github/workflows/     ← Workflows : génération CSV + sync Hugging Face
📁 versions/               ← Archive des 6 versions historiques pré-GitHub
📄 .gitignore
📄 LICENSE                 ← CC BY 4.0
📄 README.md               ← Documentation du dépôt
📄 agent.md                ← Ce fichier
📄 base-uneiaparjour.csv   ← Généré automatiquement par le workflow
📄 base-uneiaparjour.ods   ← Fichier source (mis à jour manuellement)
📄 huggingface-README.md   ← Dataset card poussée vers Hugging Face
```

### Catégories WordPress (32 actives + archives)
actualités et fact-checking · application · automatisation · bande dessinée · chatbot · données · documents · éducation · histoires enfants · images · images 3D · navigateur · jeu vidéo · mindmap · musique · infographie · langues · LLM · open source · présentation · quiz et flashcards · qr code · recherche · sans compte · site web · texte · tutoriel · usage illimité · vidéo · voix · youtube · archives

### Sélection d'outils
- **60 outils** sélectionnés (version du 01/02/2026)
- **10 catégories** : éducation, chatbots, analyse de documents, présentation, quiz et flashcards, musique, voix, recherche, image et vidéo, génération d'applications et agents
- **3 niveaux d'appropriation** par catégorie (6 outils × 10 catégories)
- Disponible en version intégrée et PDF

### Aide au choix
- Application interactive (arbre de décision)
- Réalisée avec Lovable
- Objectif : aider l'utilisateur à se positionner face aux choix lors de l'utilisation d'un outil IA

---

## Newsletter (Lettre d'infos)

### Distribution
- **Plateforme** : Substack (`https://uneiaparjour.substack.com/`)
- **Fréquence** : hebdomadaire (samedi)
- **Numéro actuel** : #28+ (au 14/03/2026)
- **Première lettre** : 6 septembre 2025

### Structure récurrente de chaque lettre
1. **Les outils de la semaine** — récapitulatif des outils publiés dans la semaine
2. **Le focus** — coup de cœur ou zoom sur un outil/thème particulier
3. **Lectures partagées** — sélection de ressources de veille (articles, études, podcasts)

### Évolution du nom de la rubrique de veille
- Lettre #1 : « Vu et lu cette semaine »
- Lettres #2-4 : « Lu cette semaine »
- À partir de la lettre #5 : « Lectures partagées »

---

## Page Lectures partagées (détail)

### Données
- **211 ressources** compilées (lettres #1 à #28, au 14/03/2026)
- Classement thématique en **10 catégories** (antéchronologique au sein de chaque catégorie)

### Catégories thématiques
1. Études et recherche
2. Éthique et société
3. IA et travail
4. Sécurité et désinformation
5. Création, art et médias
6. Technique et infrastructure
7. Philosophie et réflexions
8. Éducation
9. Droit
10. Géopolitique et international

### Format d'une entrée
```
Titre (en gras)
Description (texte de la lettre)
À retrouver dans la lettre #N du JJ/MM/AAAA
Lien direct : Source
```

### Outil d'export (composant annexe, uniquement pour les lectures partagées)
- **Hébergement** : GitHub Pages (`https://uneiaparjour.github.io/export-lectures-partagees/`)
- **Intégration dans le site** : iframe dans la page WordPress `/lectures-partagees/`
- **Dépôt source** : `https://github.com/uneIAparjour/export-lectures-partagees`
- **7 formats d'export** : HTML (bookmarks), OPML (Feedly), CSV, RDF (Zotero), XML, Markdown, BibTeX, JSON
- **Fonctionnalités** : filtrage par catégorie, recherche textuelle, sélection individuelle
- **Données** : fichier `data.json` statique (mis à jour manuellement)
- **Style** : fond sombre (#2D2D2D), accent orange (#E67E22), police Montserrat

---

## Réseau de diffusion

### Présence en ligne
- **Site** : `https://www.uneiaparjour.fr` (WordPress)
- **Newsletter** : `https://www.uneiaparjour.substack.com`
- **Réseaux sociaux** : X/Twitter, LinkedIn, Bluesky (via @bertrandformet + #uneIAparjour) et page Facebook (`https://www.facebook.com/uneIAparjour/`)
- **GitHub** : `https://www.github.com/uneIAparjour` — 5 dépôts :
  - `base` : base de données du site (ODS + CSV, sync Hugging Face)
  - `export-lectures-partagees` : outil d'export des lectures partagées (GitHub Pages)
  - `site-documentation` : documentation générale du site (agent.md, etc.)
  - `choix-outil-ia` : application « Aide au choix » d'un outil IA (réalisée avec Lovable)
  - `rrs-to-ods` : script de conversion RSS vers ODS pour l'alimentation de la base
- **Hugging Face** : `https://huggingface.co/datasets/uneiaparjour/base` — base de données (synchronisée depuis GitHub)

---

## Règles impératives pour un agent IA

### Intégrité des données
- **Ne jamais inventer ou modifier les URLs** des outils ou des ressources : extraire fidèlement depuis les sources (site, Substack)
- La source de vérité pour les **dates de publication** est le site WordPress, pas le feed RSS (qui peut refléter des dates de republication)
- Lors de la synchronisation, **croiser** les données entre la base .ods, le feed RSS et le contenu du site

### Conventions de nommage
- Titre d'un outil = nom tel qu'affiché sur le site (respect de la casse originale)
- Les catégories WordPress doivent être reproduites exactement (slug et libellé)
- Les 10 catégories des lectures partagées sont fixes et distinctes des catégories WordPress

### Mise à jour du contenu
- **Articles** : publication quotidienne, vérifier le site ou le RSS pour les nouveaux ajouts
- **Newsletter** : hebdomadaire, chaque nouveau numéro peut contenir 4-8 nouvelles ressources pour les lectures partagées
- **Sélection** : mise à jour périodique (mention « Version du JJ/MM/AAAA »), ~60 outils
- **Base .ods** : reflet complet des articles du site. Mise à jour sur GitHub → CSV et Hugging Face se mettent à jour automatiquement. WordPress (`/base/`) reste à mettre à jour manuellement.
- **Lectures partagées / data.json** : mis à jour manuellement dans le dépôt `export-lectures-partagees`

### Intégration technique

#### Dépôt `uneIAparjour/base` (base de données du site)
- Contient l'ODS source, le CSV auto-généré, et l'agent.md
- Workflows GitHub Actions : génération CSV + synchronisation Hugging Face
- Mise à jour par téléversement d'un nouvel ODS → le reste est automatique
- Le dossier `versions/` archive les 6 versions pré-GitHub ; le versioning est désormais géré par Git

#### Dépôt `uneIAparjour/export-lectures-partagees` (outil d'export des lectures partagées)
- Ne concerne que la page Lectures partagées, pas le reste du site
- Le fichier `data.json` est statique et mis à jour manuellement (ne jamais modifier les URLs existantes)
- L'iframe dans WordPress communique via `postMessage` pour le scroll et le redimensionnement
- Police Montserrat héritée du site parent, cohérence visuelle orange/noir impérative

### Ton et style éditorial
- Factuel, sobre, pas de superlatifs commerciaux
- Descriptions d'outils : fonctionnalités concrètes, modèles sous-jacents quand connus, limitations visibles
- Pas de promotion, pas de jugement de valeur subjectif non étayé
- Respect du format existant (prose courte, captures brutes)

---

## Contacts et mentions légales

- **Auteur** : Bertrand Formet
- **Email** : contact@uneiaparjour.fr
- **Réseaux** : @bertrandformet
- **Hébergement** : OVH, FR
- **Licence contenu** : CC BY 4.0
- **Indépendance** : aucune collaboration commerciale, aucun sponsoring
