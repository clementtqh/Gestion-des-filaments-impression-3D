---
layout: default
title: "2. Étape 2 : Conception Mécanique, Programmation et Communication"
nav_order: 6
---

# Étape 2 : Conception Mécanique, Programmation et Communication

Cette section détaille le cœur technique de notre projet : la modélisation 3D des supports de bobines et de capteurs, la programmation de l'ESP32 pour l'acquisition des données, ainsi que le déploiement de l'architecture réseau industrielle sur TIA Portal V20.

---

## 📐 Partie 1 : Conception et Modélisation 3D (Onshape) & Impression

Pour notre infrastructure physique, nous devions concevoir un système capable de maintenir la bobine tout en garantissant une mesure stable du filament restant. Nous avons fait le choix de concevoir un ensemble monobloc reliant deux structures :
1. **Un support de bobine robuste** pour poser et guider la bobine en rotation.
2. **Un support de capteur sur mesure** positionné face au profil du filament.

La modélisation a été entièrement réalisée en CAO sur le logiciel **Onshape**.

### 🛠️ Résolution des problèmes mécaniques et itérations :
* **Vetusté et casses (Versions 1 & 2) :** Lors des premières impressions en PLA, les bras du support de la bobine pliaient sous le poids d'une bobine neuve (environ 1 kg) ou cassaient au niveau des liaisons à angle droit à cause des vibrations de l'imprimante. Le support du capteur bougeait également, ce qui faussait les mesures de distance.
* **Optimisation mécanique (Version Finale) :** Pour y remédier, nous avons appliqué des principes de RDM (Résistance des Matériaux) sur Onshape. Nous avons ajouté des congés de raccordement pour éviter les concentrations de contraintes, augmenté l'épaisseur des parois (passant de 2 mm à 4 mm) et intégré des nervures de renfort triangulaires.
* **Paramètres d'impression retenus :** Pour garantir la solidité finale, nous avons configuré le slicer avec un remplissage (*infill*) à 30% en nid d'abeille (*honeycomb*) et 4 couches de périmètre.

---

## 💻 Partie 2 : Acquisition des Données (ESP32 & Capteur Ultrason / Temps de Vol)
En parallèle de la mécanique, nous avons développé le module d'acquisition embarqué à l'aide d'un microcontrôleur **ESP32** programmé sous **Arduino IDE**, associé à un capteur de distance **ToF (Time-of-Flight)**.

### 🎯 Logique de fonctionnement et calcul de la distance
Pour ce projet, nous avons utilisé un capteur ToF (technologie laser de temps de vol) qui offre une précision millimétrique, insensible à la couleur ou à la texture du filament. Le capteur est fixé au-dessus de la bobine, pointé vers le rouleau de filament.

La logique de déduction de la matière est basée sur la proximité :
* **Bobine pleine :** Le rayon de la bobine est au maximum. Le filament est donc très proche du capteur ToF. La distance mesurée est **minimale**.
* **Bobine vide :** Au fur et à mesure de l'impression, le filament est consommé et le rayon diminue. Le filament s'éloigne du capteur fixe. La distance mesurée augmente jusqu'à atteindre son **maximum** (le niveau du tambour central vide).

### 🛠️ Stabilité du code et traitement du signal (Arduino)
L'environnement d'une imprimante 3D génère de nombreuses vibrations dues aux mouvements des axes X, Y et Z, ce qui peut fausser les mesures instantanées du laser. Pour garantir la fiabilité de la donnée :
1. **Filtrage numérique :** Nous avons écrit un algorithme dans l'Arduino IDE pour avoir la mesure afin d'obtenir la position de la bobine .
2. **Envoi des données :** Une fois la distance stable validée, l'ESP32 utilise son module Wi-Fi intégré pour transmettre cette valeur à l'automate.


## 🌐 Partie 3 : Architecture Réseau et Intégration Industrielle (TIA Portal V20)

Une fois la donnée acquise par l'ESP32, l'enjeu était de la faire remonter à travers le réseau Wi-Fi de l'atelier jusqu'à notre Automate Programmable Industriel (API) principal afin de l'afficher sur l'IHM.
### 1. Configuration Réseau et Liaison S7 Connection
Notre démarche a d'abord été purement réseau. Dans l'environnement de configuration des appareils de **TIA Portal V20**, nous avons configuré une liaison virtuelle de type **S7 Connection**. Pour cela, nous avons dû mapper précisément :
* L'adresse IP fixe de notre automate principal.
