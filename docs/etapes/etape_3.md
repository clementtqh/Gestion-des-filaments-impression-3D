---
layout: default
title: 3. Phase de Tests et Réglages
parent: Etapes de fabrication
nav_order: 3
---

# Étape 3 : Phase de Tests, Configuration IHM et Bilan d'Avancement

Cette dernière section détaille la phase de validation de notre système, la configuration visuelle de l'Interface Homme-Machine (IHM) sur TIA Portal V20, ainsi que le bilan technique précis de l'état d'avancement de notre projet.

---

## 🖥️ Partie 1 : Interface Homme-Machine (IHM) sur TIA Portal V20

L'aboutissement visuel de notre logique d'automatisation se situe sur l'écran de l'IHM, conçu pour les élèves et professeurs de la mini usine de l'école.

### Éléments graphiques et dynamiques intégrés :
* **Jauges de niveau :** Traduction visuelle du pourcentage calculé par les blocs de mise à l'échelle `NORM_X` et `SCALE_X`. La jauge se vide dynamiquement selon la consommation théorique de filament.
* **Seuils d'alertes visuels :** * 🟢 **Vert :** Quantité de filament optimale.
  * 🟡 **Orange :** Seuil d'approche (fin de bobine proche).
  * 🔴 **Rouge :** Seuil critique (changement requis immédiatement).

---

## 🧪 Partie 2 : Protocoles de Tests par Simulation

Pour contourner la problématique réseau terrain et valider la totalité de notre algorithme et de notre interface, nous avons mis en place des **tables de forçage de variables** sur TIA Portal V20.

* **Simulation de la consommation :** En injectant manuellement des valeurs de distance (simulant le capteur ToF s'éloignant du centre de la bobine), nous avons validé la parfaite linéarité de la conversion mathématique. Une valeur simulée à mi-course renvoie bien exactement **50%** sur l'IHM.
* **Validation du système d'alerte :** Lorsque la distance simulée franchit le seuil critique configuré (équivalent à moins de 10% de filament restant), le bit d'alarme s'active instantanément : le voyant rouge clignote sur l'IHM et la routine d'envoi du message de notification au professeur se déclenche avec succès dans notre environnement de test.

---

## 📊 Partie 3 : Bilan d'Avancement (85% Réalisé) et Perspectives

À l'échéance du projet, nous dressons un bilan technique honnête et rigoureux de notre système de suivi de filaments :

### ✅ Ce qui est finalisé et opérationnel à 100% :
1. **La partie Mécanique :** Conception CAO sur Onshape réussie, itérations de robustesse validées, supports de bobines et de capteurs ToF imprimés en 3D et assemblés.
2. **La partie Acquisition Électronique :** Programmation de l'ESP32 sur Arduino IDE fonctionnelle, traitement du signal du capteur ToF par moyenne glissante opérationnel.
3. **La logique Automate & IHM :** Programmation des blocs de calcul (`GET`, `NORM_X`, `SCALE_X`), gestion des alarmes et design de l'IHM prêts sur TIA Portal V20.

### ⚠️ Le point de blocage (Les 15% restants) :
Le projet est globalement **finalisé à 85%**. Notre seule et unique difficulté réside dans la **programmation et la configuration de la liaison Wi-Fi directe de l'automate pour le relier phys
