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

# VII. QUESTION FINALE (TRÈS PROBABLE)

## Quelle est la contribution scientifique principale ?

> La démonstration qu'un agent IA peut être intégré dans un système logistique critique sans compromettre la sécurité déterministe, grâce à une séparation formelle entre sélection heuristique et validation structurelle.

---
