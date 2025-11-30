<!--
Title: "Concepts fondamentaux de ML - Types d'apprentissage - Par renforcement"
Author: rsquaredata
Last updated: 2025-11-30
-->

# Apprentissage par renforcement

L'apprentissage par renforcement (*Reinforcement Learning* - RL) est l'une des **trois grandes branches** du Machine Learning (ML), aux côtés de l'apprentissage supervisé et de l'apprentissage non supervisé.

## 1. Distinction et principes de base

L'apprentissage par renforcement se distingue fondamentalement des deux autres types d'apprentissage, car, dans ce cadre, il n'y a "**pas de données réelles en soi**" (*no real data per se*).

Le principe central du RL repose sur un système dynamique :
- Le système **évolue dans un environnement**.
- Il doit être capable d'entreprendre les **bonnes actions** en fonction de cet environnement et de l'état dans lequel il se trouve.
- Ceci est réalisé via un **système de récompenses/punitions** (*reward/punishment system*).

**Exemple illustratif — Pilotage automatique d'un drone** :
- L'objectif est d'éviter les obstacles pour atteindre sa destination.
- L'environnement peut être considéré comme l'ensemble des objets proches du drone.
- L'état du drone peut être sa condition structurelle ou sa position relative aux éléments environnants.
- Les actions correspondent aux mouvements du drone (gauche, droite, haut, bas).
- Dans le cas où le système frappe ou s'approche d'un obstacle, **la valeur de la récompense obtenue par le système diminue**.

## 2. Applications typiques du RL

Les domaines d'application du RL sont ceux où des décisions séquentielles en temps réel sont nécessaires :
- Les **décisions en temps réel** (*Real-time decisions*).
- L'**IA de jeu** (*Game AI*).
- La **navigation robotique** (*Robot Navigation*).
- L'**acquisition de compétences** (*Skill Acquisition*).
- Les **tâches d'apprentissage** (*Learning Tasks*).
