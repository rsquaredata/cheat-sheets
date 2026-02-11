## Slide 1 : Titre

- Titre : Urgence Manager
- Sous-titre : Régulation logistique agentique des urgences hospitalières : Vers une orchestration de soins augmentée.
- Note : Ajouter logo Lyon 2.

## Slide 2 : Le défi : Fracture en première ligne

- Points clés :
    - Flux imprévisibles et goulots d'étranglement.
    - Délais d'attente affectant le pronostic vital.
    - Pression décisionnelle sur le personnel (IOA).
- Visuel suggéré : Image d'un service d'urgence saturé.

## Slide 3 : Architecture Hybride Intelligente

- Points clés :
    - Moteur de règles (Python/Pydantic) : Source de vérité.
    - Cerveau IA (Mistral AI) : Pilotage et raisonnement.
    - Séparation stricte entre Analyse et Action.
- Visuel suggéré : Schéma avec deux blocs distincts (Logic vs AI Brain).

## Slide 4 : Logique agentique : Hiérarchie du Brain
Priorités :
Priorité 0 (Vital) : Orientation immédiate des cas ROUGE.
Priorité 1 (Flux) : Libération systématique des salles de consultation.
Priorité 2 (Boarding) : Transfert vers unités d'hospitalisation (Ortho, Cardio).

## Slide 5 : Maîtrise opérationnelle : Les outils
- Outils "LLM-Safe" :
    - Transfer Escort (Aide-Soignant requis).
    - Transfer Basic (Zones d'attente).
    - Transfer Staff (Optimisation de la surveillance).
- Note : Chaque outil vérifie les règles métier avant exécution.

## Slide 6 : RAG : De la Donnée à l'Explication

- Processus :
    - Retrieval : Extraction des trajectoires (CSV).
    - Augmentation : Injection des règles IOA.
    - Explicabilité : Justification des priorités en langage naturel.

## Slide 7 : Machine Learning : Anticiper pour mieux réguler

- Modèles :
    - K-Means : Classification de la tension (Calme vs Critique).
    - Random Forest : Probabilité d'hospitalisation (Précision : 93.8%).
- Visuel suggéré : Capture du graphique de tension ou de précision.

## Slide 8 : La tour de contrôle (Dashboard)

- Fonctionnalités :
    - Flux Sankey en temps réel.
    - Monitoring système (Latence 306ms).
    - Traçabilité atomique et coûts API (0.05$).

## Slide 9 : Validation : Stress tests

- 9 scénarios validés.
- Respect des protocoles vitaux même en saturation.
- Optimisation de 100% de la ressource disponible.
- Simulation manuelle possible

## Slide 10 : IA responsable : sobriété & maîtrise

- Principes :
    - Guardrails : Blocage des actions non sécurisées.
    - Sobriété : Utilisation de modèles optimisés.
    - Transparence : Contrôle humain maintenu.

## Slide 11 : Conclusion & Impact

- Bilan :
    - Fluidité des flux (patients, ressources)
    - Sécurité des protocoles garantie.
    - Réduction de la charge mentale.

## Slide 12 : Questions & Merci

- Équipe : Aya, Lamia, Maissa, Mazilda, Rina
- Stack : Mistral AI, Python, Streamlit.







