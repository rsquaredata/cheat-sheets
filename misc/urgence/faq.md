# I. ARCHITECTURE & POSITIONNEMENT

---

## Est-ce simplement une application Streamlit ?

**Réponse courte :**

> L'application n'est qu'une interface.
> La contribution principale est architecturale : intégrer un agent heuristique non déterministe dans un système logistique déterministe sécurisé.

**Si on pousse :**

> La valeur scientifique réside dans la séparation formelle entre sélection flexible et validation structurelle.

---

## Le système est-il déterministe ?

**Réponse courte :**

> Le moteur et le scheduler sont déterministes.
> Le LLM ne l'est pas, mais toute action doit être validée.

**Si on pousse :**

> Le non-déterminisme est limité à la sélection heuristique, jamais à la validation.

---

## Que se passe-t-il si on retire le LLM ?

> Il reste un moteur logistique déterministe pleinement fonctionnel.

---

## Le LLM peut-il faire une erreur ?

> Oui. Mais il ne peut pas violer une contrainte métier.

---

## Pourquoi ne pas utiliser uniquement un solveur d'optimisation ?

**Réponse courte :**

> Un solveur optimise une fonction définie.
> Nous démontrons ici une intégration flexible et explicable sous contraintes.

**Si on pousse :**

> Le LLM apporte heuristique contextuelle et explicabilité, le moteur garantit la validité.

---

# II. FORMALISATION & THÉORIE

---

## Peut-on formaliser le système comme un MDP ?

> Oui, il est compatible avec un cadre MDP : états, actions, transitions.
> Mais nous n'avons pas défini de fonction de récompense ni appris de politique optimale.

---

## Garantissez-vous des invariants ?

> Oui, certains invariants structurels sont garantis :
> capacité non dépassée, priorités vitales respectées, disponibilité du personnel.

---

## Votre fonction C(s,a) est-elle complète ?

> Elle est complète relativement aux règles formalisées.
> Nous ne prétendons pas couvrir l'ensemble des contraintes hospitalières réelles.

---

## Y a-t-il un risque de deadlock ?

> Le système peut saturer, mais il ne peut pas entrer dans un état incohérent.
> Il continue d'évoluer par cycles.

---

# III. DONNÉES & VALIDITÉ SCIENTIFIQUE

---

## Vos données sont simulées. Quelle valeur scientifique ?

> La contribution porte sur l'architecture et la formalisation.
> Les données servent à tester la robustesse structurelle.

---

## Quelle est la principale faiblesse actuelle ?

> Les décisions reposent sur des heuristiques définies a priori, sans validation empirique sur données réelles.

---

## Prochaine amélioration scientifique ?

> Intégrer des données réelles, calibrer le moteur et comparer différentes politiques décisionnelles.

---

# IV. MACHINE LEARNING

---

## Pourquoi du clustering ?

> Pour structurer l'état global du service indépendamment des contraintes locales.

---

## Pourquoi K-Means ?

> Simplicité, interprétabilité et segmentation ordonnable.

---

## Pourquoi Random Forest ?

> Pour capturer des interactions non linéaires tout en conservant une interprétabilité.

---

## 93.8% de précision, est-ce fiable ?

> C'est cohérent sur données simulées structurées.
> Une validation externe serait indispensable en contexte réel.

---

## Les modèles ML influencent-ils directement les décisions ?

> Non.
> Ils enrichissent l'analyse, mais la validation reste déterministe.

---

# V. KPI & GRAPHIQUES

---

## Pourquoi afficher clustering et KPI ?

> Le clustering structure l'état global.
> Les KPI mesurent les performances opérationnelles.
> Diagnostic macro vs indicateurs micro.

---

## Que représente le Sankey ?

> Les trajectoires cumulées des patients dans la session.

---

## Pourquoi le scénario 1 AS ?

> Pour tester la robustesse en configuration dégradée.

---

## Pourquoi monitorer les appels API ?

> L'intégration d'un LLM implique latence et coût mesurables.
> La gouvernance technologique fait partie de la robustesse.

---

# VI. ÉTHIQUE & RESPONSABILITÉ

---

## Est-ce éthiquement acceptable ?

> Oui, car aucune décision autonome n'est laissée à l'agent.
> Le système assiste la logistique sous supervision humaine.

---

## En cas d'erreur ?

> Les décisions sont tracées.
> L'origine peut être identifiée et analysée.

---

# VII. AUTRES QUESTIONS

## Quelle est la contribution scientifique principale ?

> La démonstration qu'un agent IA peut être intégré dans un système logistique critique sans compromettre la sécurité déterministe, grâce à une séparation formelle entre sélection heuristique et validation structurelle.

---

## Expliquez les chiffres du dashboard.

Peu importe la métrique, toujours répondre  en 3 temps :
1. Ce que c'est
2. Comment c'est calculé
3. Ce que ça signifie dans le scénario en question

→ Toujours rattacher au scénario.

Exemple :  
> Le temps d'attente moyen est calculé à partir des timestamps d'entrée et de sortie par gravité.
> Dans le scénario 1 AS, il augmente mécaniquement pour les patients verts, car la priorité vitale est respectée.

---

## Quelles sont les limites du projet ?

> Les principales limites sont :
> - données simulées,
> - absence de validation clinique,
> - heuristiques définies a priori,
> - absence d'optimisation globale formelle.
> En revanche, la robustesse structurelle est démontrée.

---

## Pourquoi utiliser le plus gros modèle Mistral ?

> Nous avons choisi un modèle plus expressif pour maximiser la qualité de la justification et la cohérence des propositions heuristiques.
> Cependant, l'architecture est indépendante du modèle : un modèle plus léger pourrait être substitué sans compromettre la validation.

> Le choix du modèle n'affecte pas la sécurité, uniquement la qualité linguistique et contextuelle.

---

## Pourquoi ne pas afficher clairement le staff disponible ?

> Le staff disponible est intégré dans l'état système et utilisé par le moteur de validation.
> L'interface privilégie la lisibilité globale.
> Une amélioration UX consisterait à exposer explicitement ces informations.

---

## Définissez heuristique.

> Une heuristique est une règle décisionnelle approximative utilisée pour sélectionner une action sans résoudre un problème d'optimisation formelle complète.

Ou plus simple :  
> Une heuristique guide la décision rapidement, sans garantir l'optimalité globale.

Dans notre système :  
> Dans notre système, une heuristique est une règle décisionnelle utilisée par l'agent pour sélectionner une action candidate à partir de l'état courant et des priorités métier, sans résoudre un problème d'optimisation globale.

---

## Les chiffres ont-ils une valeur réelle ?

> Ils ont une valeur interne au modèle simulé.
> Ils ne prétendent pas refléter un hôpital réel, mais permettent d'évaluer la cohérence du système sous contraintes.

> Les métriques sont calculées sur les trajectoires simulées et servent à évaluer la cohérence interne du système sous contraintes.

---

## Pourquoi ne pas avoir fait du RL ?

> L'objectif n'était pas l'apprentissage optimal, mais la démonstration d'un cadre sécurisé d'intégration d'agent.

---

## Votre système est-il vraiment intelligent ?

> Il est structurellement sûr.
> L'intelligence est encadrée, pas autonome.

---

## Votre système améliore-t-il objectivement les performances par rapport à un baseline sans LLM ?

> Nous n'avons pas conduit d'évaluation comparative formelle entre une politique sans LLM et une politique avec LLM.
> L'objectif du projet était architectural : démontrer la possibilité d'intégrer un agent heuristique sous contraintes, pas d'optimiser quantitativement les performances.
> Qualitativement, le LLM produit des arbitrages plus contextualisés et des justifications plus riches. Mais nous n'avons pas mesuré un gain statistiquement validé.

> Nous n'avons pas mené d'évaluation quantitative formelle.
> Cependant, l'architecture est modulaire : il serait trivial d'implémenter une politique purement déterministe basée uniquement sur les priorités métier, sans agent heuristique.
> Cela constituerait une baseline naturelle.
> L'intérêt de notre architecture est précisément qu'elle permet cette comparaison, sans refactorisation du système.

Plus académique :
> La séparation sélection/validation rend l'ablation study immédiate :
> il suffit de remplacer la couche heuristique par une règle déterministe fixe.

---

## Si la baseline déterministe respecte déjà les priorités, quel est l'intérêt du LLM ?

> Le moteur garantit la légalité des actions.
> Le LLM améliore la qualité du choix parmi les actions légales.

Plus académique :
> Une politique déterministe stricte applique une hiérarchie figée.
> Le LLM permet d'introduire une sélection heuristique adaptative lorsque l'espace d'actions valides contient plusieurs alternatives équivalentes au regard des contraintes.
> Il apporte de la flexibilité décisionnelle et de l'explicabilité en langage naturel.

---

## Pourquoi votre projet mérite-t-il une très bonne note ?

> Parce que nous ne nous sommes pas limités à développer une application fonctionnelle.
> Nous avons formalisé un système dynamique contraint, conçu une architecture modulaire, intégré un agent heuristique non déterministe sous validation déterministe stricte, et assuré traçabilité, explicabilité et monitoring.
> Le projet démontre une maîtrise technique, une structuration théorique cohérente et une réflexion critique sur ses limites.
Pause
> Il s'agit d'un démonstrateur architectural robuste, pas d'un simple prototype visuel.




