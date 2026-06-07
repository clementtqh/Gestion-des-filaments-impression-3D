---
layout: default
title: "1. Étape 1 : Spécifications et Initialisation du Projet"
nav_order: 5
---

# Étape 1 : Spécifications et Initialisation du Projet

Cette section décrit la première étape de notre processus de fabrication : la phase de réflexion, l'étude de faisabilité et la définition du cahier des charges pour le système de suivi des filaments 3D.

---

## Brainstorming et Genèse de l'Idée

L'objectif initial était de résoudre un problème récurrent dans l'atelier d'impression 3D : la panne de matière première en cours d'impression, qui génère des interruptions de flux et ralentit la production de l'usine école de près de 15%. Notre équipe a conçu une solution permettant de mesurer et d’afficher en temps réel le pourcentage de filament restant directement sur une Interface Homme-Machine (IHM) industrielle.

### Points clés de la réflexion :
- **Précision :** Choix d'un capteur optique de distance (Time-of-Flight) plutôt qu'un système de pesée pour s'affranchir des frottements et des tensions parasites sur le fil pendant l'impression.
- **Architecture :** Transit des informations du capteur de terrain vers un microcontrôleur d'acquisition, avant d'attaquer la couche de traitement de l'usine.
- **Choix technique :** Utilisation d'un automate programmable industriel (API) Siemens central pour centraliser le calcul et garantir la robustesse de la solution en environnement de production.

---

## Cahier des Charges Fonctionnel

Pour valider cette première étape, nous avons défini les spécifications techniques suivantes :

| Fonction | Description | Solution retenue |
| :--- | :--- | :--- |
| **Mesure** | Capter le rayon restant sur la bobine. | Capteur de distance ToF infrarouge. |
| **Traitement** | Centraliser et traiter les données capteurs. | Automate Siemens S7-1500 (bloc `FC Calcul_Filaments`). |
| **Communication** | Assurer le flux continu des variables physiques. | Liaison locale I2C couplée au réseau industriel de l'automate. |
| **Affichage** | Visualiser l'état du stock et les alertes de maintenance. | Interface Homme-Machine (IHM) dynamique sur écran tactile Siemens. |

---

## Outils et Environnement Identifiés

À la fin de cette première phase, nous avons listé les outils matériels et logiciels nécessaires pour le déploiement du projet :
- **Matériel Électronique** : Capteur optique ToF et microcontrôleur Arduino pour l'acquisition de données.
- **TIA Portal V20** : Environnement logiciel de programmation pour la configuration matérielle de l'automate et le développement de la supervision graphique.
- **Machines du Makerspace** : Infrastructures d'Amiens utilisées pour fabriquer et adapter les pièces physiques du support au sein de l'armoire.
- **Automate Siemens S7-1500** : Unité centrale de traitement de laboratoire servant de support pour valider le traitement du signal et les tables de variables.

---
