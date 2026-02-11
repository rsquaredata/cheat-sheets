# Démo App : Urgence Manager

Durée : 5 minutes | Scénario : Stress-Test "Personnel Minimal" (1 AS)

## Navigation & Setup Simulation (00:00 - 01:30)

### Aya

Nous débutons la démonstration sur l'onglet Simulation. Le menu latéral permet de naviguer entre les modules décisionnels et analytiques. Pour ce test, nous avons configuré un scénario '_Personnel Minimal_' : un seul Aide-Soignant est disponible. À droite, les métriques d'état confirment la charge : le temps d'attente moyen commence à croître dès le lancement du moteur de règles.

## Mazilda

Observons le _Diagramme de Sankey_. C'est le cœur visuel du flux. On voit les patients entrer par l'Admission, passer au Triage, puis stagner en Salle d'attente. L'épaisseur des liens vers les Boxes de consultation est limitée par notre unique AS. Ce graphique permet de détecter instantanément que le blocage est logistique (manque de bras) et non médical (disponibilité des médecins).

### Rina

En bas, l'_Historique des Événements_ logue chaque micro-décision. On y voit l'agent IA mettre en attente des cas U4 pour libérer l'AS sur une urgence vitale. Contrairement à une gestion humaine qui pourrait être submergée, l'IA traite chaque événement de manière atomique, garantissant qu'aucune demande de transfert n'est oubliée dans la file d'attente.

## Intelligence Agentique & RAG (01:30 - 03:00)

### Aya

Passons à l'onglet _AI Brain Logic_. C'est ici que l'agent Mistral expose son raisonnement. On y voit des blocs JSON structurés. Pour chaque action, comme TRANSFER_ESCORT, l'IA justifie sa décision par l'analyse des priorités. Si le système tente une action hors protocole, nos '_Guardrails_' bloquent l'exécution, assurant une sécurité absolue.

### Mazilda

L'onglet _RAG Analysis_ apporte l'explicabilité. En posant une question via le chat, comme 'Pourquoi ce patient attend-il ?', l'IA interroge les logs CSV et les règles métier. Elle répond avec des faits : 'Patient U3 en attente car l'unique ressource AS est mobilisée sur un cas U1'. C'est une traçabilité totale indispensable pour l'audit médical.

### Rina

Dans ce module, on peut aussi visualiser la pertinence des réponses. Le système ne se contente pas de générer du texte, il vérifie la conformité avec la base de connaissances hospitalière. Cela transforme une 'boîte noire' en un assistant transparent, capable de justifier ses priorités logistiques auprès des cadres de santé.

# Machine Learning & Dashboard KPIs (03:00 - 04:30)

### Aya

L'onglet _Machine Learning_ montre notre couche prédictive. Notre modèle Random Forest affiche une précision de 93.8% pour prédire l'hospitalisation des patients dès le triage. Cela permet d'anticiper les besoins en lits d'aval. Le clustering K-Means, lui, catégorise la tension du service : nous sommes actuellement en zone 'Critique' (point rouge).

### Mazilda

Regardons maintenant l'onglet Dashboard & KPIs. Le monitoring système affiche une latence moyenne de 306ms, ce qui est excellent pour de la régulation en temps réel. Le coût est dérisoire : environ 0.05$ pour l'ensemble de la simulation. Côté métier, le score de satisfaction est maintenu à 96.6/100 grâce à la gestion optimale des priorités.

### Rina

La _Heatmap d'utilisation du personnel_ est frappante : avec 1 seul AS, sa case est jaune vif, indiquant 100% d'occupation. Le graphique '_Temps d'attente par Gravité_' prouve l'efficacité du système : les cas les plus graves (U1/U2) restent proches de zéro minute d'attente, tandis que les cas légers (U5) absorbent le retard logistique.

# Conclusion (04:30 - 05:00)

### Aya

Pour conclure, la page About présente notre stack : Python 3.11, Mistral AI et Scikit-Learn. 

### Mazilda

Urgence Manager prouve qu'avec un agent IA encadré par des règles métier strictes, on peut optimiser un service même en sous-effectif critique.


### Rina

Notre solution est déployable, explicable et sécurisée. Merci de votre attention, nous sommes prêts pour vos questions.







