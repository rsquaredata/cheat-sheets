# SLIDE 1 - Contexte & Problématique

## AYA

Un service d'urgence est un système sous pression permanente.

Le personnel est limité. Les flux sont imprévisibles. Les décisions doivent être prises en quelques secondes.

Chaque choix impacte la congestion, les délais... et parfois le pronostic vital.

_(pause courte)_

## MAZILDA

Le problème n'est pas uniquement médical. Il est logistique.

Comment orchestrer patients, personnel et capacités... sous contraintes strictes et en temps réel ?

_Transition_

Nous avons donc choisi d'aborder les urgences comme un problème d'orchestration sous contraintes.

# SLIDE 2 - Modélisation formelle

## RINA

Nous avons formalisé le service comme un système dynamique discret.

À chaque instant t, l'état sₜ est défini par les patients, les ressources, les files et l'occupation temporelle.

Les actions possibles appartiennent à un ensemble fini : transférer un patient, mobiliser du staff, organiser une escorte.

Mais toutes les actions ne sont pas autorisées.

## AYA

_(pointer vers la fonction de contrainte)_

Nous définissons une fonction C(sₜ, aₜ) ∈ {0,1}.

1 : action valide. 0 : action interdite.

_(pause)_

Le système n'autorise jamais une action qui viole les règles métier.

_Punchline :_

Ici, l'IA ne décide pas seule. Elle agit dans un espace strictement contraint.

## MAZILDA

_Transition :_

À partir de cette base formelle, nous avons construit une architecture hybride.

# SLIDE 3 - Architecture hybride intelligente

"Notre architecture repose sur une séparation stricte entre logique et raisonnement.

_(pointer schéma)_

À gauche : le moteur de règles en Python, structuré avec Pydantic.

C'est la source de vérité. Il garantit les contraintes métier.

## RINA

À droite : le cerveau IA, basé sur Mistral.

Il analyse, propose, justifie... mais ne peut pas agir directement.

Toute action passe par le moteur de règles.

_Punchline :_

L'IA raisonne. Le système décide.

_Transition :_

Mais raisonner ne suffit pas. Il faut prioriser.

# SLIDE 4 - Règles de priorité

## AYA

Nous avons implémenté trois niveaux de priorité.
- Priorité vitale : le pronostic vital prime toujours.
- Priorité de flux : éviter les goulots d'étranglement.
- Priorité de boarding : limiter l'occupation prolongée des box.

Ces règles structurent les décisions de l'agent.

Nous avons testé 6 scénarios, dont plusieurs stress tests.

Dans chaque cas, le système respecte strictement ces priorités.

## MAZILDA

_Transition :_

Pour orchestrer ces décisions, nous avons implémenté un agent.

# SLIDE 5 - Architecture agentique


L'agent fonctionne en boucle.

Il observe l'état du système.

Il génère une analyse via le LLM.

Il propose une action.

Cette action est validée par les règles.

Puis le scheduler met à jour l'état.

## RINA 

Nous avons donc une boucle analyse → validation → action.

_Transition :_

Mais pour garantir la sécurité, nous avons ajouté des garde-fous.

# SLIDE 6 - Guardrails


Nous avons intégré trois niveaux de guardrails.
- Contraintes métier codées.
- Validation systématique des actions.
- Séparation stricte entre génération et exécution.

Même si le modèle propose une action incohérente...

_(pause)_

Elle est automatiquement refusée.

_Punchline :_

L'IA n'a jamais le pouvoir final.

## AYA

_Transition :_

Voyons maintenant les outils d'action.

# SLIDE 7 - Tools sécurisés

Nous avons défini trois outils d'action :
- TransferPatient.
- TransferEscort.
- TransferStaff.

Chaque outil déclenche une vérification complète des contraintes.

Si une règle est violée, l'action est bloquée.

## MAZILDA

_Transition :_

Ces actions s'inscrivent dans une dynamique temporelle.

# SLIDE 8 - Scheduler & dynamique

Le système évolue par cycles discrets.

Chaque cycle met à jour les disponibilités via busy_until.

Cela nous permet de modéliser l'occupation réelle du personnel.

Le système est donc un système dynamique contraint.

_Transition :_

Pour garantir robustesse et maintenabilité, nous avons structuré le code.

# SLIDE 9 - Clean Code & Typage fort

## RINA

Le projet repose sur une architecture modulaire.

Typage fort avec Pydantic.

Séparation claire des responsabilités.

Contraintes implémentées de manière déterministe.

Cela garantit robustesse, traçabilité et extensibilité du système.

## RINA

_Transition :_

Nous avons également ajouté une brique d'apprentissage.

# SLIDE 11 – Brique ML

Nous avons intégré un module de prédiction de tension du service.

Il permet d'anticiper les pics de congestion.

Ce module peut être couplé à l'agent pour adapter la stratégie.

Nous passons ainsi d'une orchestration réactive...

_(pause)_

À une orchestration anticipative.

## AYA

_Transition :_

Pour améliorer l'explicabilité, nous avons aussi intégré un mécanisme RAG.

# SLIDE 12 – RAG & explicabilité

Nous extrayons les trajectoires du système.

Nous injectons les règles IOA.

Le LLM génère une justification en langage naturel.

Chaque décision peut donc être expliquée.

_Punchline :_

L'agent n'est pas seulement efficace. Il est explicable.

_Transition :_

Enfin, nous avons adopté une approche d'IA responsable.

# SLIDE 13 - IA responsable

## MAZILDA

Sobriété : modèles optimisés et coût API monitoré.

Maîtrise : guardrails et contrôle humain.

Transparence : justification des priorités.

Notre objectif n'est pas d'automatiser la médecine.

_(pause)_

Mais d'assister la logistique en toute sécurité.

_Transition :_

Techniquement, voici notre stack.

# SLIDE 14 – Stack & Synthèse

## RINA

Python, Pydantic, moteur de règles déterministe.

Mistral pour le raisonnement.

Architecture modulaire et extensible.

Au croisement de trois dimensions :
- Modélisation formelle.
- IA agentique contrôlée.
- Système orienté produit.

_Punchline finale :_

Urgence Manager est un agent logistique sous contraintes, explicable et maîtrisé.

_Transition vers démo :_

Nous allons maintenant vous montrer le système en action.
