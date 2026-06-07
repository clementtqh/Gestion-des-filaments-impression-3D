---
layout: home
nav_order: 1
title: Accueil
permalink: /
---

# Bienvenue sur notre Projet 05 Gestion des filaments impression 3D

Bienvenue dans la documentation du projet 05 Gestion des filaments impression 3D. Ce site a pour but de fournir toutes les informations nécessaires pour comprendre, utiliser et reproduire efficacement notre projet.

[Notre repo GitHub](https://github.com/Makerspace-Amiens/template-project){: .btn .btn-primary .fs-5 .mb-4 .mb-md-0 }

---

## Maquette 3D Interactive du Support

*Utilisez votre souris ou votre doigt pour faire pivoter (clic gauche), déplacer (clic droit) et zoomer sur le support.*

<script src="https://embed.gvtc.com/js/stl_viewer.min.js"></script>

<div id="stl_cont" style="width:100%; height:500px; border:1px solid #dee2e6; border-radius:6px; background:#ffffff;"></div>

<script>
    var stl_viewer = new StlViewer(
        document.getElementById("stl_cont"),
        { 
            models: [ {id: 0, filename: "images/support_filaments.stl", color: "#E60000"} ],
            zoom: 85
        }
    );
</script>

---

## À propos du Projet
Au sein de l'usine école 4.0 d'UniLaSalle, nous disposons d'imprimantes 3D qui consomment une quantité importante de filament. Actuellement, ces machines sont installées à l'intérieur d'armoires de stockage, ce qui engendre une absence totale de visibilité technique et d'ergonomie matérielle.

En effet, aucune solution ne permet de connaître en temps réel la quantité de filament restante. Parallèlement, le mode de rangement actuel dans les armoires manque de structures adaptées, transformant chaque changement de bobine en une manipulation complexe et désorganisée.

Pour répondre à ces problématiques, notre projet consiste à installer un support de stockage optimisé pouvant accueillir 14 bobines. Grâce à l'intégration d'une HMI, nous permettrons à l'utilisateur de connaître avec précision, sous forme de pourcentage, le moment où une action de maintenance est nécessaire, incluant le changement de la bobine.

## Vidéo

<video src="images/video_groupe_05.mp4" controls title="Vidéo Projet 05" style="width: 100%;"></video>

---
