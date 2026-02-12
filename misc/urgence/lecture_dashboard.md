## Détail du clustering

> Il s'agit du cluster auquel appartient l'état courant du service.
> Ici, on applique un K-Means à 4 clusters pour segmenter les états du service.
> Le bloc affiché correspond au profil moyen du cluster actuel.
> On observe un cluster “calme” avec 1 patient en moyenne et aucune gravité rouge ou jaune.
> C'est une analyse descriptive, pas décisionnelle.

> `ratio_rouge` permet de mesurer la pression relative, pas seulement le volume brut.
> Parce que 2 rouges, ça ne veut rien dire sans contexte :
> - 2 rouges sur 2 patients → situation critique (100%)
> - 2 rouges sur 50 patients → tension relative faible (4%)
> On distingue charge absolue vs intensité relative.

---

## Matrice de confusion

> Cette matrice montre que le modèle prédit systématiquement la classe majoritaire, ici “Sorti”.
> On observe 99 bonnes prédictions pour les sorties, mais aucun hospitalisé correctement identifié.
> C'est un effet classique du déséquilibre de classes sur un petit dataset simulé.
> Dans un cadre réel, on optimiserait le recall sur la classe critique via class weights ou ajustement du seuil.
> Ici, la brique ML est démonstrative, pas clinique.

---

## Classification de l'état des urgences

> Ce graphe montre l'évolution temporelle de l'état global du service d'urgence, tel que classifié par notre modèle ML.
> Axe horizontal → Le temps (cycles de simulation).
> Axe vertical → L'état du service : CALME / NORMAL / TENDU
> La ligne bleue représente la prédiction du modèle à chaque instant.

> Ce graphique montre la classification dynamique de l'état du service au fil du temps.
> On observe que dans le scénario minimal, le système reste majoritairement en état calme, avec quelques pics de tension.
> Cela correspond à une charge modérée, sans saturation structurelle.

> Ça oscille autant parce que le modèle réagit aux variations locales : arrivées simultanées, gravité élevée, occupation temporaire du staff.
> Sur données simulées, c'est illustratif. Sur données réelles, on ferait validation croisée et calibration.

---

## Patients par gravité

> C'est un histogramme empilé dans le temps.
> Axe horizontal → Temps de la simulation (en minutes)
> Axe vertical → Nombre total de patients présents dans le service

> Ce graphique montre l'évolution de la charge hospitalière au cours de la simulation.
> La hauteur totale correspond au nombre de patients présents, et les couleurs représentent la répartition par gravité.
> On observe une accumulation progressive, typique d'un scénario à ressources limitées.

> Ça augmente continuellement parce que dans le scénario minimal, la capacité de traitement est inférieure au flux d'entrée.
> C'est un scénario volontairement contraint pour observer le comportement sous tension.
> On n'a pas de plateau parce que la sortie dépend du staff disponible et du temps de prise en charge modélisé.

---

## Monitoring système

### Appels API – 60 (~60 000 tokens)

Cela signifie :
- 60 cycles d'analyse par le LLM
- Environ 60 000 tokens consommés au total

👉 Chaque cycle correspond à une boucle : observation → analyse → proposition d'action.

Ce chiffre permet d'évaluer :
- la charge LLM
- la scalabilité
- le coût

### Latence Moyenne – 370 ms

Cela correspond au temps moyen de réponse du modèle par appel.

Lecture du graphe :
- Axe horizontal : numéro du cycle
- Axe vertical : latence en millisecondes
- Ligne bleue : latence réelle à chaque appel
- Ligne rouge pointillée : moyenne (~370 ms)

On observe :
- Quelques pics autour de 500 ms
- Une variabilité modérée
- Pas de dérive croissante
👉 Conclusion : stabilité temporelle.

### Coût Estimé API – $0.1200

Cela correspond au coût total des appels pendant la simulation.  
Important car : Une architecture agentique doit être économiquement viable.

Ici : 60 décisions → 12 centimes -> Ce n'est pas prohibitif.

### Uptime – 99.5%

Cela représente la disponibilité du service API.

Dans un système réel, cela signifierait :
- Tolérance aux erreurs
- Robustesse réseau
- Gestion des exceptions

> Cette section montre que l'architecture agentique est techniquement viable.
> La latence moyenne est de 370 ms, ce qui permet une boucle décisionnelle fluide.
> Le coût API reste maîtrisé, et la stabilité temporelle ne montre pas de dérive.
> Nous avons donc intégré une dimension d'observabilité technique complète.

---

## KPI Métier

### Utilisation Personnel – 11.5 %

Premier point critique. La heatmap montre :
- AS_01 à 100 % la plupart du temps
- AS_02 souvent à 100 %

Donc visuellement → saturation fréquente. Mais l'indicateur global affiche 11.5 %.

👉 Cela signifie que :
- soit le calcul est fait sur toute la durée totale (incluant périodes longues inactives),
- soit il y a un problème d'agrégation,
- soit on moyenne sur un horizon plus large que la heatmap.

> L'indicateur global est calculé sur l'ensemble de la simulation, tandis que la heatmap montre l'occupation par créneaux horaires spécifiques.
> Les pics sont localisés mais la moyenne reste faible.
> C'est un point que nous pourrions affiner pour aligner l'indicateur global avec la granularité horaire.

### Score Satisfaction – 93.7 / 100

Avec :
- 63 min d'attente moyenne
- 70+ min pour les rouges
- Congestion progressive

93.7 paraît élevé. Donc ce score n'est clairement pas clinique. 👉 C'est un score synthétique pondéré.

> Ce score est un indicateur composite interne basé sur les délais et priorités.
> Il ne prétend pas refléter une mesure patient réelle.

### Temps d'Attente Moyen – 63 min

Dans un scénario minimal, c'est cohérent.  
Mais 63 min avec seulement 11 % d'utilisation staff → contradiction apparente.  
Donc le problème clé ici est la cohérence entre KPI.  

### Taux d'Hospitalisation

- Rouge ~42 %
- Jaune ~15 %
- Vert ~20 %

Ça, c'est cohérent structurellement.

Ça montre que plus la gravité augmente,plus la probabilité d'hospitalisation augmente.

Bonne cohérence métier ici.

### HEATMAP STAFF

On voit :
- AS_01 saturé très souvent (100 %)
- AS_02 aussi très souvent
- INF_SALLE_01 et INF_SALLE_02 jamais utilisés

Donc :
- Soit les infirmiers ne sont pas intégrés dans les règles décisionnelles.
- Soit leur rôle n'est pas déclenché dans ce scénario.
- Soit il y a un déséquilibre dans la modélisation des tâches.
👉 Si on demande pourquoi les infirmiers sont à 0 % :
> Dans le scénario minimal simulé, certaines ressources ne sont pas sollicitées car les règles métier déclenchent principalement les aides-soignants.
> C'est un choix de paramétrage du scénario.

> L'objectif ici n'est pas la validité clinique des KPI, mais la démonstration que l'architecture permet leur calcul et leur traçabilité.

---

## Visualisations avancées

### Sankey – Flux de Patients dans le Service

Chaque rectangle = une étape du parcours :
- Accueil / Triage
- Salle Attente 1
- Salle Attente 2
- Soins Critiques
- Salle Attente 3
- tran_wr_consult
- tran_wr_hos

Chaque bande = un flux de patients ➡️ Plus la bande est épaisse → plus il y a de patients.

Ce que ça révèle dans le scénario (1 AS) :
- Grosse masse au départ → Accueil / Triage
- Beaucoup passent par Salle Attente 1
- Peu vont en Soins Critiques
- Une partie significative finit en tran_wr_consult
- Certains en tran_wr_hos

Lecture stratégique

Ce que le jury peut voir :

Il existe des boucles longues

Des patients font :
triage → salle → consultation → hospitalisation

👉 Ça peut signaler :
- Des réorientations
- Des congestions
- Une inefficience structurelle

> Le Sankey permet d'identifier visuellement les goulets d'étranglement et les parcours inefficients.
> On voit ici que la majorité des flux transitent par les salles d'attente, ce qui reflète la contrainte du scénario minimal avec un seul AS.

### Évolution Temporelle – Patients par Gravité

Lecture complète :
- Au début :, Pic ROUGE, Pic JAUNE, Pic VERT
- Puis : Stabilisation, ROUGE retombe, JAUNE fluctue, VERT reste modéré

> On observe que les pics ROUGE sont résorbés rapidement, ce qui montre que la règle de priorité vitale fonctionne correctement malgré la contrainte en personnel.

### Parcours Types – Top 5

Ça montre les 5 parcours les plus fréquents dans la simulation.

Ce que ça révèle :
- Beaucoup de parcours courts
- Quelques parcours longs avec hospitalisation
- Certains passent par soins critiques

> On voit que la majorité des parcours restent courts, ce qui reflète un service qui ne sature pas totalement, mais certains parcours complexes apparaissent lorsque les contraintes de capacité ralentissent le traitement.

Pourquoi c'est utile ces visualisations ?
> Elles permettent une lecture multi-échelle : flux global (Sankey), dynamique temporelle (évolution), et micro-structure des parcours. Cela permet d'identifier à la fois les goulets d'étranglement et la dynamique de saturation.





