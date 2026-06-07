---
layout: default
title: 1. Étape 1 : 
parent: Etapes de fabrication
nav_order: 1
---

# Étape 1 : Spécifications et Initialisation du Projet

Cette section décrit la première étape de notre processus de fabrication : la phase de réflexion, l'étude de faisabilité et la définition du cahier des charges pour le système de suivi des filaments 3D.

## 💡 Brainstorming et Genèse de l'Idée

L'objectif initial était de résoudre un problème récurrent dans l'atelier d'impression 3D : la panne de matière première en cours d'impression. Nous avons réfléchi à une solution permettant de mesurer et d'afficher en temps réel le pourcentage de filament restant directement sur une Interface Homme-Machine (IHM).

### Points clés de la réflexion :
- **Précision :** Comment mesurer le filament (poids de la bobine, capteur optique, ou calcul de la longueur extrudée) ?
- **Architecture :** Comment faire remonter l'information de l'imprimante vers l'IHM de manière industrielle ?
- **Choix technique :** Utilisation d'un automate programmable industriel (API) comme nœud central pour garantir la robustesse de la solution.

## 📋 Cahier des Charges Fonctionnel

Pour valider cette première étape, nous avons défini les spécifications techniques suivantes :

| Fonction | Description | Solution retenue |
| :--- | :--- | :--- |
| **Mesure** | Capter la quantité de filament restante. | Capteurs dédiés sur l'imprimante 3D |
| **Traitement** | Centraliser et traiter les données capteurs. | Automate Programmable Industriel (API) |
| **Communication** | Lier l'imprimante à l'automate. | Protocole réseau supporté par TIA Portal V20 |
| **Affichage** | Visualiser le pourcentage restant de manière claire. | Interface Homme-Machine (IHM) dynamique |

## 🛠️ Outils et Environnement Identifiés

À la fin de cette première phase, nous avons listé les outils logiciels nécessaires pour la suite :
- **TIA Portal V20** : Pour la configuration de l'automate et la création de l'IHM.
- **Imprimantes 3D de test** : Pour l'acquisition des données de filaments.
- **Automate de laboratoire** : Pour valider la communication.

---

Une fois cette étape de spécification terminée, passez à la conception technique : [Étape 2 : Conception et Codage](/conception).
