# Introduction et Contexte (00:00 - 02:00)

## Aya

### SLIDE 1 : Titre

Bonjour à tous. Nous vous présentons Urgence Manager, une solution de régulation logistique agentique. Notre projet répond à une crise majeure : l'engorgement des services d'urgences. L'objectif est d'utiliser l'IA pour orchestrer les flux patients de manière optimale, en libérant du temps de cerveau pour les équipes soignantes.

## Mazilda

### SLIDE 2 : Le défi

Le défi est triple : les flux sont imprévisibles, les ressources limitées et la pression décisionnelle constante. Au triage, le délai d'attente impacte directement le pronostic vital. Urgence Manager intervient ici comme un assistant intelligent capable de prioriser les actions logistiques en temps réel.

## Rina

### SLIDE 3 : Architecture hybride

L'innovation repose sur une architecture hybride. Nous séparons strictement la logique métier déterministe, qui est notre source de vérité, du raisonnement IA. Cela permet de bénéficier de la puissance de calcul des LLM tout en garantissant un cadre opérationnel rigide et sécurisé.

## Aya

Techniquement, nous utilisons Python 3.11 pour le moteur de règles et l'API de Mistral AI pour le cerveau décisionnel. L'ensemble de l'état hospitalier est modélisé par des objets Pydantic, assurant une validation stricte des données avant chaque cycle de décision.

# Architecture et Logique Agentique (02:00 - 04:00)

## Mazilda

### SLIDE 4 : Logique agentique

Le "Brain" de l'agent fonctionne par cycles de perception-action. Il récupère le dashboard complet des urgences, analyse les risques de saturation et génère une série d'actions structurées en JSON. Ce raisonnement suit des priorités médicales pré-établies.

## Rina

La priorité absolue est l'urgence Vital, ou Priorité 0. Tout patient classé ROUGE au triage est immédiatement orienté vers les soins critiques. L'IA ne peut pas ignorer ce protocole : elle doit mobiliser les ressources nécessaires pour libérer un box de déchocage en priorité.

## Aya

Vient ensuite la priorité de Flux. L'IA surveille la disponibilité des médecins. Dès qu'une salle de consultation se libère, elle organise le transfert du prochain patient prioritaire. L'objectif est d'éliminer les temps morts entre deux consultations.

## Mazilda
Enfin, la priorité de Boarding gère la sortie des urgences. Dès qu'une décision d'hospitalisation est prise, l'agent cherche à transférer le patient vers les services d'aval, comme la cardiologie ou l'orthopédie, en fonction des capacités de lits réelles.

# Outils et RAG (04:00 - 06:00)

## Rina

### SLIDE 5 : Outils opérationnels

Pour agir, l'IA dispose d'un catalogue d'outils atomiques. Ces outils ne sont pas directement du code IA, mais des fonctions Python "LLM-Safe". Par exemple, le Transfer Escort vérifie systématiquement si un aide-soignant est disponible avant de lancer l'action.

## Aya

Ces outils gèrent également les contraintes de temps. Un transport prend entre 5 et 45 minutes selon la complexité. L'agent doit donc anticiper l'occupation du personnel sur ces durées pour ne pas paralyser le service par une succession de transferts mal planifiés.

## Mazilda

### SLIDE 6 : Pipeline RAG

Pour l'explicabilité, nous avons intégré un pipeline RAG (Retrieval-Augmented Generation). L'IA peut répondre aux questions des soignants en croisant l'état actuel avec l'historique des trajectoires stocké en CSV. Cela justifie chaque décision par des faits.

## Rina

Cette brique RAG permet de comprendre pourquoi un patient PAT_001 attend depuis deux heures. L'IA analyse les priorités comparatives et les règles médicales IOA pour fournir une réponse factuelle, renforçant ainsi la confiance clinique envers le système.

# Machine Learning et Analytique (06:00 - 08:00)

## Aya

### SLIDE 7 : Machine Learning

En parallèle du LLM, nous exploitons le Machine Learning classique. Un modèle K-Means effectue un clustering de l'état de tension du service. On passe dynamiquement d'un état "Calme" à "Critique", ce qui modifie l'agressivité des politiques de transfert de l'IA.

## Mazilda

Nous utilisons aussi une Random Forest pour prédire la probabilité d'hospitalisation. Dès l'entrée au triage, si le modèle prédit une probabilité de 90%, l'Urgence Manager commence à pré-alerter les services d'aval pour anticiper la libération d'un lit.

## Rina

### SLIDE 8 : Dashboard

Le monitoring est assuré par un Dashboard Streamlit. Il offre une vue en temps réel via des diagrammes de Sankey, montrant les flux physiques des patients. On y suit aussi la latence des appels API et la consommation des jetons Mistral.

## Aya

Ce dashboard permet une traçabilité totale. Chaque mouvement, chaque décision du "Brain" est logguée. En cas de décision contestée, l'équipe SISE peut remonter la chaîne de raisonnement pour vérifier si une règle métier a été mal interprétée.

# Validation et Éthique (08:00 - 10:00)

## Mazilda

### SLIDE 9 : Stress tests

La fiabilité du système a été testée sur 9 scénarios critiques. Nous avons simulé des pannes de lits, des afflux massifs de patients et des effectifs réduits. Dans tous les cas, les "Guardrails" ont empêché l'IA de violer les protocoles de sécurité vitale.

## Rina

### SLIDE 10 : Éthique

Notre solution adopte une approche responsable. Notre IA est sobre : nous utilisons Mistral-Small pour les tâches simples afin de limiter l'empreinte carbone. De plus, l'IA ne remplace jamais le médecin ; elle se contente d'exécuter la logistique validée par les protocoles.

## Aya

###  SLIDE 11 : Conclusion

En conclusion, Urgence Manager transforme la gestion des urgences en une orchestration dynamique. L'impact est immédiat : réduction de la charge mentale, fluidification des parcours et surtout, une meilleure sécurité pour les patients les plus graves.

## Rina

### SLIDE 12 : Questions
Urgence Manager est ainsi un pas vers l'hôpital augmenté. Merci de votre attention.

Si n'avez pas de questions, nous passons maintenant à la présentation de l'app.

---
