---
layout: default
title: "2. Étape 2 : Conception Mécanique, Programmation et Communication"
nav_order: 6
---

# Étape 2 : Conception Mécanique, Programmation et Communication

Cette section détaille le cœur technique de notre projet : la modélisation 3D des supports de bobines et de capteurs, la programmation de l'ESP32 pour l'acquisition des données, ainsi que le déploiement de l'architecture réseau industrielle sur TIA Portal V20.

---

## Partie 1 : Conception et Modélisation 3D (Onshape) & Impression

Pour notre infrastructure physique, nous devions concevoir un système capable de maintenir la bobine tout en garantissant une mesure stable du filament restant. Nous avons fait le choix de concevoir un ensemble monobloc reliant deux structures :
1. **Un support de bobine robuste** pour poser et guider la bobine en rotation.
2. **Un support de capteur sur mesure** positionné face au profil du filament.

La modélisation a été entièrement réalisée en CAO sur le logiciel **Onshape**.

### Résolution des problèmes mécaniques et itérations :
* **Vetusté et casses (Versions 1 & 2) :** Lors des premières impressions en PLA, les bras du support de la bobine pliaient sous le poids d'une bobine neuve (environ 1 kg) ou cassaient au niveau des liaisons à angle droit à cause des vibrations de l'imprimante. Le support du capteur bougeait également, ce qui faussait les mesures de distance.
* **Optimisation mécanique (Version Finale) :** Pour y remédier, nous avons appliqué des principes de RDM (Résistance des Matériaux) sur Onshape. Nous avons ajouté des congés de raccordement pour éviter les concentrations de contraintes, augmenté l'épaisseur des parois (passant de 2 mm à 4 mm) et intégré des nervures de renfort triangulaires.
* **Paramètres d'impression retenus :** Pour garantir la solidité finale, nous avons configuré le slicer avec un remplissage (*infill*) à 30% en nid d'abeille (*honeycomb*) et 4 couches de périmètre.

---

## Partie 2 : Acquisition des Données (ESP32 & Capteur Ultrason / Temps de Vol)
En parallèle de la mécanique, nous avons développé le module d'acquisition embarqué à l'aide d'un microcontrôleur **ESP32** programmé sous **Arduino IDE**, associé à un capteur de distance **ToF (Time-of-Flight)**.

### Logique de fonctionnement et calcul de la distance
Pour ce projet, nous avons utilisé un capteur ToF (technologie laser de temps de vol) qui offre une précision millimétrique, insensible à la couleur ou à la texture du filament. Le capteur est fixé au-dessus de la bobine, pointé vers le rouleau de filament.

La logique de déduction de la matière est basée sur la proximité :
* **Bobine pleine :** Le rayon de la bobine est au maximum. Le filament est donc très proche du capteur ToF. La distance mesurée est **minimale**.
* **Bobine vide :** Au fur et à mesure de l'impression, le filament est consommé et le rayon diminue. Le filament s'éloigne du capteur fixe. La distance mesurée augmente jusqu'à atteindre son **maximum** (le niveau du tambour central vide).

### Stabilité du code et traitement du signal (Arduino)
L'environnement d'une imprimante 3D génère de nombreuses vibrations dues aux mouvements des axes X, Y et Z, ce qui peut fausser les mesures instantanées du laser. Pour garantir la fiabilité de la donnée :
1. **Filtrage numérique :** Nous avons écrit un algorithme dans l'Arduino IDE pour avoir la mesure afin d'obtenir la position de la bobine .
2. **Envoi des données :** Une fois la distance stable validée, l'ESP32 utilise son module Wi-Fi intégré pour transmettre cette valeur à l'automate.


## Partie 3 : Architecture Réseau et Intégration Industrielle (TIA Portal V20)

Une fois la donnée acquise par l'ESP32, l'enjeu était de la faire remonter à travers le réseau Wi-Fi de l'atelier jusqu'à notre Automate Programmable Industriel (API) principal afin de l'afficher sur l'IHM.
### 1. Configuration Réseau et Liaison S7 Connection
Notre démarche a d'abord été purement réseau. Dans l'environnement de configuration des appareils de **TIA Portal V20**, nous avons configuré une liaison virtuelle de type **S7 Connection**. Pour cela, nous avons dû mapper précisément :
* L'adresse IP fixe de notre automate principal.
* * L'adresse IP de l'équipement partenaire distant (qui centralise les données des capteurs).
* Le paramétrage des slots et racks logiques pour établir le pont de communication.

Cette configuration a automatiquement généré un identifiant de liaison unique : **l'ID réseau 100 en hexadécimal (W#16#100)**. Cet ID sert de route officielle et sécurisée pour acheminer nos paquets de données à travers les points d'accès Wi-Fi de l'atelier.

### 2. Programmation et Routage des Données (Bloc GET)
Nous avons ensuite structuré la logique de programmation dans l'automate en intégrant le bloc système standard **GET** (SFB14) dans le diagramme de bloc (OB1) :
* **Routage :** Nous avons appliqué l'ID de liaison `W#16#100` sur l'entrée `ID` du bloc.
* **Pointeur ANY :** Sur le paramètre `ADDR_1`, nous avons configuré manuellement le pointeur ANY sur la syntaxe exacte : `P#DB1.DBX0.0 INT 1`. Cela donne l'ordre précis au bloc d'aller lire une longueur de 1 entier (`INT 1`) à l'adresse de départ du bit 0.0 du Bloc de Données numéro 1 (`DB1.DBX0.0`) de l'automate distant.
* **Réception :** Le bloc rapatrie cette donnée brute et la stocke localement via la sortie `RD_1` dans une variable temporaire que nous avons nommée `#valeur_brute_wifi`.

### 3. Traitement mathématique et Mise à l'Échelle (NORM_X et SCALE_X)
Une fois la variable reçue, il s'agissait de transformer une simple valeur numérique brute (souvent comprise entre 0 et 27648 en standard Siemens, ou une valeur de distance brute en mm) en un pourcentage lisible par l'utilisateur (0% à 100%).

Nous avons modifié le câblage logique existant : nous avons déconnecté l'ancienne entrée physique locale de l'automate pour injecter à la place notre nouvelle variable `#valeur_brute_wifi`.
* **Bloc NORM_X (Normalisation) :** Il prend la valeur brute reçue par Wi-Fi et la convertit en une valeur décimale normalisée comprise strictement entre `0.0` et `1.0` (selon les bornes min/max que nous avons calibrées en fonction d'une bobine pleine et d'une bobine vide).
* **Bloc SCALE_X (Mise à l'échelle) :** Il récupère cette valeur normalisée et la projette sur une plage de sortie configurée entre `0.0` et `100.0`. 

Grâce à ce traitement, nous obtenons en temps réel un **pourcentage de filament fluide, linéaire et précis**, directement exploitable pour l'affichage dynamique sur les jauges de l'IHM.

Lors de la mise en ligne, le bloc GET a immédiatement validé la cohérence de notre architecture en renvoyant le code d'état **STATUS 16#0001**, prouvant qu'une communication Wi-Fi industrielle est établie, stable et prête pour le déploiement.

