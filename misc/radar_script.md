SCRIPT ORAL - PRÉSENTATION RADAR (7-9 min)

## Slide 0 - Introduction (≈ 40 s)

### Thibaud
Bonjour, nous allons vous présenter RADAR, un projet de data science et de NLP dont l'objectif est d'analyser le marché de l'emploi data à partir des offres publiées en ligne, avec une approche à la fois sémantique, territoriale et applicative.

L'idée centrale du projet est de partir de données textuelles non structurées — les offres d'emploi — pour construire un observatoire opérationnel, capable d'aider à la fois à la compréhension du marché et à la prise de décision, côté candidats comme recruteurs.

Pour comprendre pourquoi ce projet est nécessaire, on commence par la problématique.

## Slide 1 - Problématique & Objectifs (≈ 1 min)

### Rina
Aujourd'hui, le marché de l'emploi est extrêmement fragmenté : les offres sont dispersées sur de nombreuses plateformes, avec des formats hétérogènes, et surtout beaucoup de texte non structuré.

Cela rend difficile une vision globale du marché, notamment à l'échelle régionale, et complique l'identification des compétences réellement demandées.

### Aya
Les objectifs de RADAR sont donc triples :

- Centraliser des offres issues de sources multiples,
- Structurer automatiquement l'information via des méthodes NLP,
- Et visualiser ces résultats pour produire une aide à la décision exploitable.

On ne cherche pas à prédire un recrutement individuel, mais à décrire et structurer un marché.

## Slide 2 - Architecture Générale (≈ 1 min)

### Habib
Dès le départ, RADAR a été pensé comme une architecture modulaire end-to-end, et non comme un simple notebook exploratoire.

Le pipeline est découpé en plusieurs briques indépendantes : 

- acquisition des données,
- stockage,
- traitement NLP,
- et restitution via une application interactive.

Cette modularité permet à la fois la reproductibilité, l'évolutivité, et la possibilité de remplacer ou d'améliorer un module sans remettre en cause l'ensemble du système.

Ce choix est volontairement plus proche d'une logique d'industrialisation que d'un projet académique isolé.

## Slide 3 - Sources & Prétraitement (≈ 1 min)

### Thibaud
Pour garantir la représentativité, nous avons diversifié les canaux : France Travail pour le socle national, des agrégateurs comme Adzuna ou Jooble via son API, et même l'usage d'Agents IA pour scraper des sources complexes comme Emploi Territorial.

### Rina
Les données utilisées provenant de plusieurs sources d'offres d'emploi, publiques et privées, avec des formats et des niveaux de qualité très variables, un travail important a donc été réalisé sur le prétraitement : déduplication, nettoyage du texte, normalisation des champs, et enrichissement.

### Aya
À l'issue de ce pipeline, on aboutit à une base consolidées de 2 764 offres uniques, représentant plus de 33 000 compétences extraites, avec des indicateurs de qualité comme la répartition des types de contrats.

## Slide 4 - Modèles NLP & Extraction sémantique (≈ 1 min)

### Habib
Pour l'analyse sémantique, nous avons fait le choix de méthodes interprétables et adaptées à un contexte métier.

Notre architecture NLP suit une logique de raffinage successif. D'abord, la vectorisation par TF-IDF permet de donner un poids statistique aux compétences rares.

Ensuite, notre moteur de Reconnaissance d'Entités (NER) s'appuie sur une taxonomie métier de 161 items pour extraire précisément les Hard et Soft skills.

Enfin, le Clustering LDA nous permet de découvrir de manière non supervisée les grandes familles de métiers qui structurent réellement le marché.

L'objectif n'est pas d'avoir le modèle le plus complexe, mais le plus compréhensible et actionnable.

## Slide 5 - Analyse territoriale & Visualisations (≈ 1 min)

### Thibaud
Bloc 1 - Cartographie  
La cartographie permet de visualiser la densité et la nature des offres par région ou département.

Bloc 2 - Compétences dominantes  
On peut identifier les compétences les plus demandées localement, ce qui met en évidence des spécialisations régionales.

### Rina
Bloc 3 - Structure du marché  
Au-delà de la géographie, nous analysons la structure contractuelle. Avec 75 % de CDI identifiés, RADAR permet de qualifier la pérennité des emplois par zone, offrant ainsi une vision de la "santé" du marché au-delà du simple volume d'annonces.

Bloc 4 - Aide à la décision  
Ces éléments facilitent la comparaison territoriale et aident à orienter des choix de mobilité ou de recrutement.

## Slide 6 - Fonctionnalités clés (≈ 1 min)

### Aya
RADAR n'est pas uniquement un projet d'analyse, mais une application fonctionnelle.

Elle propose un tableau de bord interactif, un moteur de recherche par compétences, des visualisations dynamiques, et des fonctionnalités d'export.

Un module d'assistance à la candidature, basé sur des techniques d'IA, ouvre également la voie à des usages plus personnalisés. L'objectif est de rendre l'analyse utilisable par des non-experts.

### Habib
L'application intègre également une dimension communautaire et pédagogique. Via la page 'Contribuer', les utilisateurs peuvent enrichir l'observatoire tout en progressant dans un système de gamification (XP et badges).

C'est un outil complet, déployable sous Docker, qui allie analyse de pointe et expérience utilisateur engageante.

## Slide 7 - Conclusion & Perspectives (≈ 1 min)

### Thibaud
Pour conclure, RADAR propose une solution de bout-en-bout permettant de transformer des offres d'emploi textuelles en information structurée et exploitable.

Les perspectives sont à la fois techniques (enrichissement des données, modèles NLP plus avancés, automatisation complète) et applicatives, avec des fonctionnalités de matching CV, d'alertes personnalisées et de déploiement cloud.

### Rina
<small>(parler des limites si on a parlé trop vite sinon enchaîner sur la clôture)  
On peut identifier trois limites principales dans le projet:
- La fraîcheur : Le scraping est une photographie à l'instant T ; la perspective d'automatisation via Airflow vise à transformer cela en flux continu.
- Le dictionnaire : La taxonomie est performante mais statique ; l'utilisation de modèles de type BERT en perspective permettra de s'affranchir des dictionnaires pour une détection purement contextuelle.
- Les doublons : Le dédoublonnage repose sur un hash des descriptions ; il est robuste mais peut être affiné par des mesures de similarité cosinus pour détecter les offres quasi-identiques postées sur des sites différents.</small>

RADAR constitue ainsi une base solide pour un observatoire évolutif du marché de l'emploi data.

Merci pour votre attention, nous allons maintenant passer à la démonstration.

