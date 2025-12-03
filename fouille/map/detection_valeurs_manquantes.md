<!--
Title: "Procédures d'apprentissage - Préparation des données - Détection de valeurs manquantes (Imputation)"
Author: rsquaredata
Last updated: 2025-12-03
-->

# Détection des valeurs manquantes et imputation des données


La **détection de valeurs manquantes et leur imputation** constituent une étape préliminaire et obligatoire dans 
le processus global de la **préparation des données**, lui-même pilier de la **procédure d'apprentissage en fouille de 
données massives**.

Cette phase est critique car elle garantit que les jeux de données sont dans un état utilisable avant l'application de 
tout algorithme d'apprentissage.

## 1. La place de l'imputation dans la procédure d'apprentissage

Le processus complet de fouille de données se compose de quatre étapes principales, le **pré-traitement des données** 
étant la deuxième.

Dans ce cadre, la préparation des données implique plusieurs actions essentielles, parmi lesquelles :

- **Détecter d’éventuelles valeurs manquantes et faire de l’imputation**.
- Identifier les caractéristiques du jeu de données (telles que la taille, la dimension, le type de problème et le 
ratio des classes).

La gestion des valeurs manquantes est donc un *réflexe fondamental* attendu face aux jeux de données, notamment sur le 
plan pratique. Une fois que les données sont jugées "**prêtes**", la mise en œuvre du processus d’apprentissage peut 
commencer.

## 2. Pertinence dans le contexte des données massives

La présence de données manquantes est une réalité dans les jeux de données concrets :

- **Exemples concrets** : Dans un scénario d'étude clinique utilisant des données génomiques, certaines informations 
spécifiques aux patients (telles que le sexe, l'âge, le poids, la taille, ou le nombre de globules) **sont manquantes**. 
La proposition d'une procédure complète, qu'il s'agisse d'analyse génomique ou de détection de fraude fiscale, doit 
obligatoirement inclure le **pré-traitement des données**.

La procédure d'apprentissage doit donc **intégrer l'imputation** pour traiter ces lacunes avant de procéder au choix de 
l'algorithme, à la validation croisée, et à la comparaison des performances.

En pratique, l'imputation fait partie des "**méthodes de pré-apprentissage**" qui sont mises en œuvre sur les données 
avant de tester plusieurs algorithmes, pour garantir l'intégrité de l'ensemble de données utilisé pour l'entraînement

## 3. Méthodes d'imputation

Utilisation de la moyenne, de la médiane ou de modèles prédictifs.

