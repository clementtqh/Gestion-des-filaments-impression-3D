---
layout: default
nav_order: 3
title: Objectifs du projet
---

# Introduction

Dans le cadre de l'**Usine École 4.0 d'UniLaSalle Amiens**, ce projet s'inscrit dans une démarche de modernisation, d'optimisation logistique et de digitalisation des processus de fabrication additive (impression 3D). Notre groupe s'est penché sur une problématique concrète de gestion du matériel afin d'apporter une solution industrielle clé en main.

---

## Contexte du Projet

L'usine école dispose d'un parc d'imprimantes 3D intégrées dans des armoires de stockage fermées. Bien que ce système protège les machines, il engendre deux problématiques majeures au quotidien :

* **Manque d'ergonomie matérielle :** Le rangement actuel des bobines de filament manque de structures adaptées. Chaque changement de matière se transforme en une manipulation complexe et désorganisée.
* **Absence de visibilité technique :** Il est impossible de connaître en temps réel la quantité de filament restante sur une bobine fermée dans l'armoire. Cela entraîne des arrêts de production imprévus si le fil vient à manquer en cours d'impression.

---

## Cadre du Projet & Objectifs

Notre projet pluridisciplinaire se déploie à l'intersection de quatre grands domaines de notre formation :

### 1. Mécanique
* **Conception CAO :** Modélisation sur Onshape du nouveau système de support optimisé.
* **Fabrication :** Réalisation des structures physiques adaptées au sein du Makerspace d'Amiens.

### 2. Automatisme & Informatique Industrielle
* **Programmation embarquée :** Configuration du microcontrôleur (ESP32 ou Arduino) pour traiter intelligemment les mesures reçues du terrain.
* **Gestion des capteurs :** Automatisation du traitement des données physiques des filaments (densité, poids de la bobine vide) pour calculer en temps réel le pourcentage restant.

### 3. Réseaux Locaux Industriels (RLI)
* **Communication de données :** Établissement d'une liaison réseau stable pour acheminer les signaux bruts des capteurs vers l'automate.
* **Intégration Supervision (IHM) :** Connexion des variables de l'automate à l'écran Siemens de l'armoire pour afficher l'interface dynamique et déclencher les messages d'alerte de maintenance.

### 4. Gestion de Production
* **Méthodologie 5S :** Organisation du poste de travail pour rationaliser l'ergonomie, éliminer les gaspillages de temps et ordonner l'armoire de stockage.
* **Fiabilisation des flux :** Suppression des ruptures de production sur les imprimantes 3D grâce à l'anticipation exacte des changements de bobines de filament (capacité de 14 bobines simultanées).

---

# Existant & Éléments à disposition

Pour mener à bien ce projet, nous nous appuyons sur les ressources matérielles et contraintes suivantes issues de notre fiche projet :
* **Budget alloué :** 150 € maximum.
* **Matériel électronique :** Microcontrôleur (ESP32/Arduino Wi-Fi) et capteurs de mesure de poids.
* **Infrastructures :** Accès aux machines du *Makerspace* d'Amiens pour la fabrication des pièces.
* **Données d'entrée :** Caractéristiques physiques des filaments (diamètre 1.75mm, densité) et fichiers 3D actuels des armoires.

---

# Cahier des Charges & Validation

| Critère de validation | Indicateur de réussite | Statut actuel |
| :--- | :--- | :--- |
| **Capacité & Ergonomie** | Supporter 14 bobines et simplifier les manipulations physiques | **100% Validé** (Supports installés) |
| **Mesure embarquée** | Traiter les caractéristiques des bobines via l'ESP32/Arduino | **100% Validé** (Capteurs opérationnels) |
| **Interface de l'usine** | Affichage dynamique en % et alertes de maintenance sur l'IHM | **100% Validé** (Interface prête) |
| **Réseaux Industriels** | Communication fluide et stable des données vers l'automate | **En cours (85%)** (Ajustement du signal brut) |

