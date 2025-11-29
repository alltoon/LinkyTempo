# Intégration Home Assistant : Linky Tempo

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg)](https://github.com/hacs/integration)
[![Version](https://img.shields.io/github/v/release/alltoon/LinkyTempo)](https://github.com/alltoon/LinkyTempo/releases)

Une intégration personnalisée pour Home Assistant qui récupère votre consommation électrique Linky et la répartit automatiquement selon les **6 tranches tarifaires de l'offre EDF Tempo** (Bleu/Blanc/Rouge et Heures Pleines/Heures Creuses).

Contrairement aux intégrations standards, celle-ci reconstitue l'historique précis en croisant votre courbe de charge avec la couleur historique du jour Tempo.

## Fonctionnalités

*   🚀 **Configuration facile** via l'interface utilisateur (UI).
*   📊 **6 capteurs d'énergie** distincts (parfaits pour le Dashboard Énergie).
*   🎨 **Détection automatique de la couleur** du jour (via API communautaire).
*   🕒 **Gestion précise des heures** : respecte les horaires Tempo (HC de 22h à 06h).
*   💾 **Indépendant** : Pas besoin de capteur externe ou de MQTT.

## Prérequis

1.  Avoir un compteur **Linky**.
2.  Récupérer un **Token d'accès** Enedis.
    *   Cette intégration utilise le proxy de Bokub.
    *   👉 [Cliquez ici pour générer votre Token et voir votre PRM](https://conso.boris.sh/)

## Installation (via HACS Recommandé)

1.  **Ouvrez HACS** dans Home Assistant.
2.  Allez dans "Intégrations" > menu (3 points en haut à droite) > "Dépôts personnalisés".
3.  Ajoutez l'URL de ce dépôt : `https://github.com/alltoon/LinkyTempo`.
4.  Catégorie : **Intégration**.
5.  Cliquez sur "Ajouter", puis cherchez "Linky Tempo" dans la liste et installez-le.
6.  **Redémarrez Home Assistant**.

## Configuration

1.  Allez dans **Paramètres** > **Appareils et services**.
2.  Cliquez sur **Ajouter une intégration** (en bas à droite).
3.  Cherchez **Linky Tempo**.
4.  Remplissez le formulaire :
    *   **PRM** : Votre Point de Livraison (14 chiffres).
    *   **Token** : Votre jeton d'accès (commence souvent par `ey...`).

> **Note :** Les données ne remontent qu'une fois par jour (le matin pour la veille). Lors de la première installation, attendez quelques secondes que les capteurs se créent. Ils afficheront la consommation d'hier.

## Bonus : Affichez les couleurs Tempo avec un thème

Pour distinguer visuellement les jours Tempo dans vos tableaux de bord, vous pouvez utiliser un thème dédié.

1.  **Installez le thème** :
    *   Allez dans HACS > Tableau de bord (Frontend).
    *   Ajoutez ce dépôt : `https://github.com/alltoon/LinkyTempoTheme`
    *   Installez le thème "LinkyTempoTheme".
2.  **Activez le thème** :
    *   Allez sur votre profil utilisateur (en cliquant sur votre nom en bas à gauche).
    *   Sélectionnez **LinkyTempoTheme** dans le menu déroulant "Thème".

Cela vous permettra d'assigner des couleurs spécifiques à vos capteurs en fonction de la couleur Tempo du jour.

## Configuration du Dashboard Énergie

C'est ici que cette intégration prend tout son sens !

1.  Allez dans **Paramètres** > **Tableaux de bord** > **Énergie**.
2.  Dans la section "Consommation d'électricité", cliquez sur **Ajouter une source**.
3.  Ajoutez successivement vos 6 nouveaux capteurs :
    *   `sensor.linky_tempo_bleu_heures_creuses`
    *   `sensor.linky_tempo_bleu_heures_pleines`
    *   `sensor.linky_tempo_blanc_heures_creuses`
    *   ... (et ainsi de suite pour les 6)
4.  Pour chaque capteur, sélectionnez **"Utiliser un prix statique"** et entrez le tarif du kWh correspondant à votre contrat (ex: 0.1296 pour Bleu HC).

## Fonctionnement technique

Cette intégration effectue les actions suivantes toutes les 4 heures :
1.  Récupération de la **Courbe de Charge** (pas de 30 min) de la veille via l'API Enedis (Proxy Bokub).
2.  Récupération de la **Couleur Tempo** historique pour chaque jour concerné via `api-couleur-tempo.fr`.
3.  Calcul et répartition des Wh dans les bons "bacs" (ex: Si on est le 25/11 à 23h00 et que le jour est BLANC -> Ajout dans `Blanc HC`).
4.  Mise à jour des capteurs dans Home Assistant.

## Crédits

*   Basé sur les travaux de [Bokub](https://github.com/bokub/ha-linky) pour l'accès API.
*   Utilise l'API [api-couleur-tempo.fr](https://www.api-couleur-tempo.fr/) pour l'historique des couleurs.