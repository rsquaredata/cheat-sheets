<!--
Title: "Big Data Mining - Fiches - Big Data"
Author: rsquaredata
Last updated: 2025-12-09
-->

# Big data

## 1. Définition

Le **Big Data** désigne des ensembles de données dont **l'un au moins** ds attributs suivants dépasse les capacités classiques de stockage, de traitement ou d'analyse.  

## 2. Les 3V historiques

### 1) Volume

Taille massive des données (Go → To → Po).  
Exemples : logs web, capteurs IoT, génomique, transaction.  

### 2) Vélocité

Données générées ou arrivant très vite (streaming).  
Exemples : flux financiers, réseaux sociaux, surveillance en temps réel.  

### 3) Variété

Données hétérogènes :  
- structurées (tables)  
- semi-structurées (JSON, XML)  
- non structurées (texte, image, vidéo)

## 3. Les 5V complets

### 4) Véracité

Qualité incertaines, données bruitées, manquantes, incohérentes.  
Problème majeur en fouille.  

### 5) Valeur

But final : extraire un bénéfice (business, médical, optimisation).  
Sans valeur, pas de Big Data (au sens utile).  

## 4. Pourquoi un tel engouement pour les données ?

- Explosion du numérique → tout géénère de la donnée.  
- Nouvelles capacités de stockage (cloud, NoSQL).
- Puissance de calcul → GPU, clusters, calcul distribué.
- Nouvelles méthodes d'analyse : ML, deep learning.
- Les données permettent de :
  - optimiser les décisions,
  - personnaliser les services,
  - détecter anomalies/fraudes,
  - faire de la prédiction.

> Le Big Data a permis de passer d'une logique descriptive à une logique prédictive et prescriptive, grâce à l'exploitation massive des données.

## 5. Enjeux pour la fouille et le Machine Learning

### Conséquences techniques

- Les algorithmes doivent être **scalabless** (O(n) ou O(n log n)).
- Il faut gérer la **distribution des données** → Spark, MapReduce.
- Forte **hétérogénéité** → preprocessing lourd.
- **Dérive des données** (concept drift) si haute vélocité.
- Problème de **qualité** → nettoyage essentiel.

### Conséquences sur le choix des méthodes

- Éviter les modèles trop coûteux (SVM kernel sur 10M points → impossible).
- Préférer :
  - random forests
  - gradient boosting distribué
  - régressions linéaires/GLM
  - méthodes en ligne (SGD, mini-batch)
  - clustering scalable (MiniBatch KMeans)

> Les données massives se caractérisent par les 3V :  
> - Volume (données en très grande quantité),  
> - Vélocité (arrivée rapide et continue),  
> - Variété (hétérogénéité des formats).  
> 
> On ajoute souvent deux dimensions supplémentaires :  
> - Véracité (qualité incertaine),  
> - Valeur (capacité à extraire un bénéfice des données).  
> 
> L'engouement vient des progrès concomitants du stockage distribué, de la puissance de clacul et des méthodes de Machine Learning qui permettent d'exploiter ces données pour la prédiction, la détection d'anomalies ou l'automatisation de décisisons.

