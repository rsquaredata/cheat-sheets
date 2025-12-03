<details><summary><strong>todo/tuto</strong></summary>

| étape | intitulé | action |
|-------|----------|--------|
| 1 | **compte Zotero** | créer un compte gratuit sur [Zotero](https://www.zotero.org/) et installer l'app de bureau |
| 2 | **serveur WebDAV** | activer et configurer le service WebDAV sur le NAS Synology, puis entrer les paramètres (URL, identifiants) dans Zotero (Préférences > Synchronisation) |
| 3 | **extension VS Code** | installer le plugin [Better BibTeX](https://retorque.re/zotero-better-bibtex/) dans Zotero, puis installer une extension VS Code (ex.: **Citation Picker for Zotero**) |

## Exemple - Ajouter une première référence

#### 1. Ajout de la référence

- **Action** : Une fois Zotero ouvert, utiliser le connecteur Zotero du navigateur (ou l'outil de recherche par identifiant) pour trouver et ajouter cette référence.

- **Résultat dans Zotero** : Un nouvel élément est créé dans la bibliothèque :
  - **Titre** : Hidden Technical Debt in Machine Learning Systems
  - **Auteurs** : D. Sculley, Gary Holt, Daniel Golovin, Eugene Davydov, Todd Phillips, Dietmar Ebner, Vinay Chaudhary, Michael Young, Jean-François Crespo, Dan Treiman
  - **Date** : 2015
  - **Publication** : NIPS 2015
 
#### 2. Création de la fiche résumé (Note)

Dans Zotero, clic droit → Ajouter Note Enfant → **Titre de la note** : "Résumé - Dette technique ML"

```text
## Fiche Résumé : Dette technique dans les systèmes ML (Sculley et al., 2015)

### Thèse principale
Le coût réel d'un système de Machine Learning n'est pas dans le code de l'algorithme lui-même, mais dans l'**énorme
quantité d'ingénierie et d'infrastructure non-ML** nécessaire pour le faire fonctionner en production. Cette
infrastructure génère une « Dette Technique » invisible et coûteuse à maintenir.

### Points clés pour la recherche
1.  **Seulement 5% de ML :** Le code ML est une petite fraction (souvent < 5%) du code total du système. L'environnement
doit gérer les collecteurs de données, la vérification des données, la gestion des ressources, l'analyse des journaux,
etc.
2.  **Dette par changement :** Les changements de données (dérive/drift) sont plus coûteux que les changements de code.
La dette technique s'accumule lorsque l'on ne valide pas que les données de production respectent les hypothèses faites
durant l'entraînement.
3.  **Problèmes d'architecture (Exemples) :**
    * **Glue Code (Code Colle) :** Petits scripts nécessaires pour connecter les systèmes qui deviennent impossibles à
maintenir.
    * **Configuration :** Un système ML est souvent plus complexe à configurer qu'un système logiciel classique.
    * **Abstention :** L'incapacité à détecter le moment où un modèle devient obsolète.

### Conclusion / Évaluation
* **Pertinence :** rticle fondamental (Doit être cité pour l'introduction de ma partie sur les MLOps et l'infrastructure)
* **À creuser :** Les concepts de **dette de changement** (où le coût du changement n'est pas local) et de la difficulté
à détecter les boucles de rétroaction involontaires.
* **Statut :** Fini.
```

## Tagger les articles

Par exemple, ajouter les références fondamentales du MLOps et ajouter un tag "MLOps" :

| Thème | Titre de la référence | Auteurs / Année | Pourquoi l'ajouter |
|-------|-----------------------|-----------------|--------------------|
|Dette Technique/MLOps | Hidden Technical Debt in Machine Learning Systems| D. Sculley et al., 2015 | Le point de départ pour comprendre pourquoi l'ingénierie est cruciale. |
| Architecture logicielle | Software Engineering for Machine Learning: A Case Study | H. M. Kim et al., 2017 | Étude de cas sur les défis pratiques lors de la mise à l'échelle d'un système ML réel, axé sur les pratiques d'ingénierie. |
| Expérimentation | The ML Test Score: A Rubric for ML Production Readiness and Technical Debt Reduction | Eric Breck et al., 2017 | Liste pratique de tests essentiels (tests de données, de modèles, d'infrastructure) pour garantir qu'un système ML est prêt pour la production. |
| Cadre Général MLOps | Machine Learning Operations (MLOps): Overview, Challenges and Future Directions | G. H. W. Fan et al., 2020 | Une bonne synthèse des principes des MLOps (automatisation, CI/CD, monitoring) pour structurer votre collection. |
| Systèmes de Données | Data Management for Machine Learning: A Survey | Z. Ding et al., 2021 | Si on travaille beaucoup avec les données, papier essentiel pour comprendre l'état de l'art dans la gestion, le nettoyage et la validation des données pour le ML. |

### Exemple - Ajouter et gérer les tags MLOps

1. Sélectionner l'élément
    1. Sélectionner l'article à étiqueter (par exemple, "Hidden Technical Debt in Machine Learning Systems").
    2. Dans le panneau de droite de Zotero (le panneau d'information de l'élément), cliquer sur l'onglet Tags (souvent représenté par une icône d'étiquette 🏷️).
2.  Créer et appliquer le tag
    1. Cliquer sur le bouton Ajouter (ou faire un double-clic dans la zone des tags).
    2. Taper le nom de l'étiquette, par exemple : MLOps.
    3. Appuyez sur Entrée (ou Return).

Le tag `MLOps` est maintenant rattaché à cet article. On peut ajouter autant de tags que nécessaire (ex: `MLOps`,  `dette-technique`, `systèmes-distribués`).

3. Utiliser la barre d'étiquettes (panneau inférieur gauche)
  1. Dans le panneau inférieur gauche de Zotero, sous l'arborescence de vos collections, se trouve la section Étiquettes.
  2. Cliquer sur le tag MLOps → affiche tous les articles de toutes vles collections qui portent cette étiquette.
  3. On peut également utiliser la fonction de recherche dans ce panneau si la liste de tags devient très longue.

### Automatisation des tags avec Zotero
Pour vous faciliter la vie, Zotero propose une fonction très pratique appelée Tags automatiques :
1. Lors de l'ajout d'une référence depuis une source en ligne (comme IEEE Xplore ou ArXiv), Zotero importe souvent les tags (mots-clés) déjà associés à l'article par l'éditeur ou les auteurs.
2. Pour que ces tags automatiques ne polluent pas l'organisation personnelle de la collection, il est possible de choisir de masquer ces tags automatiques ou d'en faire le tri.
3. Conseil : se concentrer sur la création de tags personnels et cohérents (MLOps, Stats-Bayesiennes, NLP-Attention) pour vos fiches résumées, car ce sont les plus pertinents pour son propre flux de travail.

### Exemples de tags pertinents

#### Catégorie : Sujet (domaine / sous-domaine)

##### MLOps

| Tag suggéré | Contexte et exemples de papiers |
|-------------|---------------------------------|
| **MLOps** | architecture, déploiement, tests de production, CI/CD, dette technique |
| **stats-bayesiennes** | modèles hiérarchiques, MCMC, inférence bayésienne, Stan |
| **DeepLearning-CNN** | réseaux de neurones convolutifs, vision par ordinateur, classification d'images |
| **DeepLearning-Attention** | modèles Transformer, BERT, GPT, State-Space Models (SSM) |
| **NLP** | traitement du langage naturel, tokenization, systèmes de dialogue, RAG |
| **apprentissage-renforcement | Reinforcement Learning (RL), Q-learning, algorithmes de politique |
| **time-series** | séries temporelles, modèles ARIMA, réseaux récurrents pour la prévision |
| **explainableAI (XAI)** | interprétabilité, SHAP, LIME, éthique de l'IA |

##### Statistiques & Économétrie

| Tag suggéré | Contexte et explications |
|-------------|--------------------------|
| **stats-inférence** | tests d'hypothèses, intervalles de confiance, théorie asymptotique, bootstrapping |
| **économétrie** | modèles de régression linéaire/non linéaire, modèles à variables instrumentales, causalité |
| **modèles-discrets** | régression logistique, modèles Probit, Logit, modèles pour variables qualitatives |
| **sondages-échantillons** | méthodes d'échantillonnage, redressement, pondération, méthodes d'enquête |

##### Systèmes d'information et gestion des données

| Tag suggéré | Contexte et explications |
|-------------|--------------------------|
| **data-wrangling** | nettoyage, fusion, imputation de données manquantes, techniques de préparation de données |
| **SQL-Bases** | optimisation de requêtes, modélisation relationnelle, bases de données NoSQL (MongoDB, Cassandra) | 
| BI-Reporting,"Outils de visualisation (Tableau, Power BI), dashboards, principes de la Business Intelligence."
Data-Warehouse,"Concepts de Data Mart, ETL/ELT, modélisation en étoile/flocon."

### Catégorie : Type de document / source

| Tag suggéré | Contexte et exemples de Papiers |
|-------------|---------------------------------|
| **article-fondamental** | lLes papiers qui ont introduit un concept majeur (ex: *Attention is All You Need*) | 
| **revue-synthèse** | articles de Survey ou Review qui résument l'état de l'art d'un domaine |
| **manuel-référence** | chapitres de livres ou manuels utilisés pour apprendre une base théorique (ex: _The Elements of Statistical Learning_) |
| **implémentation-code** | ressources qui se concentrent sur un code ou une implémentation spécifique (ex: tutoriel PyTorch ou un GitHub repo) |

### Catégorie : Statut / évaluation personnelle

| Tag suggéré | Objectif |
|-------------|----------|
| **à-lire** | références ajoutées récemment et qui nécessitent une lecture approfondie |
| **à-citer-urgent** | documents dont je sais qu'ils doivent absolument figurer dans ma prochaine rédaction |
| **trop-théorique** | documents très mathématiques ou peu pratiques ; à ne consulter que pour la preuve |
| **excellent-exemple** | contient un exemple de cas d'utilisation très pertinent pour mon travail |

### Catégorie : SISE

#### Méthodes statistiques & Analyse

| Tag suggéré | Correspondance dans le MCCC | Utilité |
|-------------|-----------------------------|---------|
| **séries-temporelles** | TD Séries temporelles et données séquentielles | Pour les modèles de prévision, ARIMA, etc. |
| **ANOVA-Exp** | TD Analyse de variance et plans d'expériences | Pour la méthodologie de l'expérimentation et l'analyse de l'impact de facteurs. |
| **biostats-catégorielles** |TD Biostatistique, données catégorielles | Pour les modèles Logit/Probit, l'analyse de survie et les données de santé/sociales. |
| tests-statistiques | Applicable à toutes les UE | Pour les articles ou manuels se concentrant sur les tests d'hypothèses et l'inférence. |

#### Informatique appliquée & Systèmes de Données

| Tag suggéré | Correspondance dans le MCCC | Utilité |
|-------------|-----------------------------|---------|
| **data-warehouse** | TD Entrepôt de données avancé | modélisation dimensionnelle, ETL, architecture Data Mart |
| **Bases-NoSQL** | TD Bases de données NoSQL | pour les bases orientées document, graphe ou clé-valeur |
| **BigData-Tech** | TD Technologies Big Data | architectures distribuées comme Hadoop, Spark, ou systèmes de streaming |
| **data-viz** | TD Data visualisation | outils et principes pour la création de tableaux de bord (BI) et l'exploration |
| **MLOps-Cloud** | TD Data visualisation, Bl. Cloud, MLOps | tag pour l'intégration de ML en production, les déploiements cloud |

## TOC

## Articles
D. Sculley et al., "Hidden Technical Debt in Machine Learning Systems," in Advances in Neural Information Processing Systems 28 (NIPS 2015), 2015.

## Ouvrages
