# SLIDE 1 - Introduction

## AYA

_(regard jury, 1 seconde de silence avant de commencer)_

Imaginez un service d'urgences à 19h un vendredi soir.

Les patients arrivent.  
Les lits sont presque pleins.  
Le personnel est déjà sous tension.  

Dans ce contexte, chaque décision compte.

Aujourd'hui, nous ne vous présentons pas un chatbot médical.  
Nous vous présentons une architecture conçue pour intégrer une IA... sans jamais lui abandonner le contrôle.

_(micro pause - regard)_

# SLIDE 2 - Contexte

## MAZILDA

Un service d'urgences est un système sous pression permanente/

Personnel limité.  
Flux patients imprévisibles.  
Goulots d'étranglement en consultation ou en soins critiques.  

Le triple défi est clair :

- Orchestrer un flux dynamique sous contraintes.
- Prioriser sans violer les règles métier.
- Décider sous incertitude.

Notre question a donc été :  
comment formaliser cette orchestration pour qu'elle soit sûre et explicable ?

# SLIDE 3 - Modélisation

## RINA 

Nous avons commencé par formaliser le système.

À chaque instant $t$, l'état $s_t$ comprend :
- les patients,
- les ressources humaines,
- les files et capacités,
- et les contraintes temporelles via `busy_until`.

Les actions possibles sont finies.

Mais surtout, chaque action passe par une fonction de contrainte  $C(s_t, a_t)$.

Si $C=0$, l'action est refusée.

Le système n'autorise jamais une action qui viole une règle métier.

La validité est structurelle.

_(pause courte)_

# SLIDE 4 - Architecture hybride

## AYA

À partir de cette modélisation, nous avons construit une architecture hybride.

D'un côté, un moteur de règles déterministe en Python, typé avec Pydantic.

De l'autre, un module de raisonnement basé sur Mistral.

Le LLM observe, analyse, propose, explique.

Mais il n'exécute jamais directement.

Toute action passe par le moteur.

Analyse ne signifie pas action.

# SLIDE 5 - Priorités métier

## MAZILDA

Nous avons codé explicitement les priorités métier :

- Priorité vitale d'abord.
- Puis fluidité du flux.
- Puis gestion du boarding.

Un patient critique ne sera jamais retardé pour optimiser la congestion.

Ces règles ne sont pas implicites.  
Elles sont formalisées et testées dans six scénarios expérimentaux.

# SLIDE 6 - Cycle agentique

## RINA

Le fonctionnement suit un cycle discret :

1. Observation de l'état.
2. Raisonnement du LLM.
3. Proposition d'action.
4. Validation via $C(s,a)$.

Puis mise à jour.

Si le modèle propose une action invalide, elle est automatiquement rejetée.

Le LLM explore.
Le système dispose.

_(regard jury)_

# SLIDE 7 - Guardrails

## AYA

Nous avons intégré des guardrails structurels :

- Contraintes codées explicitement.
- Validation systématique.
- Séparation stricte analyse / exécution.
- Supervision humaine possible.

Le modèle peut se tromper.

Le système, lui, ne transgresse pas ses règles.

# SLIDE 8 - Outils LLM-safe

## MAZILDA

Les actions sont limitées à des outils typés :

- TransferPatient.
- TransferEscort.
- TransferStaff.

Chaque outil vérifie disponibilité, capacité et contraintes temporelles.

Le LLM agit dans un espace défini.

Il ne peut pas inventer une action arbitraire.

# SLIDE 9 - Scheduler

## RINA

Le scheduler assure l'évolution dynamique.

Le temps est discret, en ticks successifs.

IOA → consultation → soins critiques / aval.

La variable `busy_until` empêche toute double affectation.

Et point important : Même sans LLM, le système fonctionne.

Le LLM améliore la sélection.  
Il ne garantit pas la validité.

# SLIDE 10 - Machine Learning

## AYA

Nous avons ajouté une brique ML pour prédire la tension du service.

À partir de l'occupation, du nombre de patients critiques et de la disponibilité du staff, nous estimons le niveau de saturation futur.

Cela permet de passer d'une orchestration réactive à une orchestration anticipative.

Le ML informe la décision.  
Il ne l'exécute pas.

# SLIDE 11 - RAG & explicabilité

## MAZILDA

Pour garantir la transparence, nous avons intégré un mécanisme RAG.

Nous injectons l'historique des états et les règles pertinentes dans le contexte du LLM.

Le modèle génère alors une justification liée explicitement aux priorités métier.

Aucune décision sans règle associée.

L'agent n'est pas seulement performant.

Il est explicable.

# SLIDE 12 - Architecture logicielle

## RINA

L'architecture repose sur :

- Un typage fort avec Pydantic.
- Une séparation claire des couches.
- Une logique métier déterministe.
- Des logs auditables.

Aucune règle n'est cachée dans le LLM.

Ce choix garantit fiabilité et testabilité.

# SLIDE 13 - IA responsable

## AYA

Notre approche repose sur trois principes :

1. Maîtrise.
2. Sobriété.
3. Transparence.

Les coûts API sont monitorés.  
Les décisions sont traçables.  
La supervision humaine reste possible.  

Nous assistons la logistique.  
Nous n'automatisons pas la médecine.

# SLIDE 14 - Perspectives

## MAZILDA

Les perspectives incluent :

Une architecture multi-agent spécialisée.  
Un renforcement du module ML.  
Un RAG enrichi avec protocoles médicaux.  

Mais la philosophie reste la même :

L'intelligence augmente le système.  
Elle ne le remplace pas.

# CONCLUSION

## RINA

_(regard, pause de 1 seconde avant de commencer)_

Urgence Manager démontre qu'un agent IA peut être intégré dans un système critique sans abandonner la sécurité déterministe.

La contribution principale n'est pas le LLM.

C'est la séparation formelle entre sélection heuristique et validation structurelle.

Nous ne faisons pas confiance au modèle.

Nous faisons confiance à l'architecture.

Et c'est cette architecture qui rend le système robuste, explicable et maîtrisé.

Merci.
