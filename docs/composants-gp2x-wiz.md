# GP2X Wiz — composants et architecture matérielle

> **État de la documentation — 27 octobre 2026**
>
> La Wiz succède aux GP2X F100/F200 mais utilise une autre plateforme matérielle. Un logiciel ou firmware F100/F200 n’est donc pas automatiquement compatible. Les références de composants ci-dessous proviennent de spécifications publiées et doivent être contrôlées sur la carte pour une réparation.

## Vue d’ensemble

La Wiz est organisée autour du système sur puce **MagicEyes Pollux VR3520F**. Contrairement au MMSP2 bicœur de la F100/F200, Pollux utilise un **ARM926EJ-S** principal et intègre un accélérateur 3D compatible OpenGL ES 1.1.

| Groupe | Caractéristique principale |
|---|---|
| Système sur puce | MagicEyes Pollux VR3520F |
| Processeur | ARM926EJ-S, ARMv5TEJ, 533 MHz nominal |
| Accélération graphique | Moteur 3D intégré, OpenGL ES 1.1 |
| Mémoire vive | 64 Mio de DDR SDRAM |
| Stockage interne | 1 Gio de NAND flash |
| Stockage amovible | SD/SDHC |
| Écran | AMOLED tactile, 2,8 pouces, 320 × 240 |
| Audio | DAC stéréo Cirrus Logic CS43L22, haut-parleurs, casque et microphone |
| Commandes | Croix directionnelle, tactile, boutons d’action et d’épaule |
| Alimentation | Batterie lithium-polymère interne de 2 000 mAh |

## Processeur et graphisme

L’**ARM926EJ-S** est un cœur ARMv5TEJ avec MMU et caches, destiné à exécuter Linux et les applications. Sa fréquence nominale publiée est **533 MHz**. Des logiciels permettent d’autres fréquences, mais l’overclocking n’est pas une capacité garantie et réduit l’autonomie.

Le SoC Pollux intègre :

- contrôleurs DDR et NAND ;
- contrôleur d’affichage et blocs de traitement vidéo ;
- accélérateur 3D annoncé compatible OpenGL ES 1.1 ;
- contrôleurs SD/SDHC, USB, GPIO, temporisateurs, DMA et interfaces série ;
- fonctions d’encodage de sortie vidéo analogique selon la configuration de la carte.

Les chiffres commerciaux de débit 3D ne prédisent pas les performances d’un jeu : pilote, bibliothèque, bande passante et charge CPU restent déterminants.

## Mémoire et stockage

La Wiz dispose de **64 Mio de DDR SDRAM** et de **1 Gio de NAND flash**. La NAND héberge le firmware, ses données et les logiciels préinstallés ; toute sa capacité n’est pas disponible comme espace utilisateur.

Le logement SD accepte les cartes **SD et SDHC**. Le support exact de capacité et de système de fichiers dépend du firmware. Ne pas utiliser la procédure de mise à jour d’une GP2X F100/F200 ou d’une Caanoo : les images sont propres à la Wiz.

## Affichage et tactile

L’écran est un **AMOLED 2,8 pouces** de **320 × 240 pixels**, associé à une dalle tactile résistive. L’AMOLED produit sa propre lumière par pixel et n’emploie pas le même rétroéclairage qu’un LCD traditionnel.

Conséquences pratiques :

- très bon contraste et noirs profonds ;
- calibration tactile nécessaire ;
- vieillissement et marquage possibles sur une dalle ancienne ;
- dalle, tactile et nappe spécifiques, non interchangeables par simple analogie avec la F200 ou la Caanoo.

## Audio

Les spécifications publiées identifient un DAC stéréo **Cirrus Logic CS43L22**. La console comprend :

- haut-parleurs stéréo ;
- sortie casque stéréo ;
- microphone intégré ;
- audio PCM historique jusqu’à 16 bits et 48 kHz selon la pile logicielle.

La présence du microphone distingue la Wiz des caractéristiques sûres documentées pour la F100. Pour une réparation, vérifier séparément DAC, amplificateur, prise casque et haut-parleurs.

## Commandes et entrées

| Entrée | Fonction |
|---|---|
| Croix directionnelle | Navigation numérique |
| Écran tactile résistif | Pointage au stylet ou par pression |
| `A`, `B`, `X`, `Y` | Boutons d’action |
| `L`, `R` | Boutons d’épaule |
| `MENU` et `SELECT` | Commandes système selon le logiciel |
| Volume | Réglage audio par boutons dédiés |
| Microphone | Entrée audio intégrée |

Le boîtier commercial ne conserve pas le second pavé directionnel montré sur certains prototypes. Les logiciels doivent utiliser la disposition finale de production.

## USB, extension et sorties

La Wiz fournit une connexion USB vers PC et une interface d’extension propre à sa génération. Les spécifications distinguent une connexion périphérique USB 2.0 et des capacités host USB 1.1. L’alimentation et le câblage host doivent être vérifiés avec l’accessoire exact ; ne pas reprendre le brochage EXT de la F100/F200.

Une sortie TV analogique est documentée pour la plateforme avec l’accessoire adapté. Le câble et son brochage ne doivent pas être supposés identiques à ceux de la GP2X F100/F200.

## Alimentation

La Wiz emploie une batterie lithium-polymère interne d’environ **2 000 mAh**. Une batterie gonflée, chaude ou endommagée ne doit pas être rechargée. Son remplacement exige de respecter dimensions, polarité, connecteur, tension nominale et circuit de protection.

## Différences essentielles avec les autres modèles

- SoC Pollux monocœur ARM926EJ-S, et non MMSP2 ARM920T/ARM940T ;
- 64 Mio de DDR et 1 Gio de NAND ;
- écran AMOLED 2,8 pouces tactile ;
- accélération 3D OpenGL ES 1.1 ;
- batterie lithium-polymère interne ;
- microphone intégré ;
- firmware, SDK et connectique d’extension propres à la Wiz.

## Sources

- [Wikipedia — GP2X Wiz](https://en.wikipedia.org/wiki/GP2X_Wiz) : synthèse des spécifications Pollux, mémoire, écran, audio et batterie ; source secondaire signalant elle-même sa dépendance aux sources primaires.
- [Archive OpenHandhelds pour Wiz](https://dl.openhandhelds.org/cgi-bin/wiz.cgi) : firmwares, SDK et logiciels historiques.
- [Catalogue des firmwares](firmwares-gp2x.md) : versions et précautions propres à la Wiz.
- [magiceyes](https://github.com/zdiemer/magiceyes) : projet contemporain confirmant la distinction MMSP2 pour GP2X et Pollux pour Wiz/Caanoo ; ce n’est pas une documentation constructeur.

Aucune Wiz n’a été ouverte ou mesurée pour cette fiche. Les références de puces doivent être confirmées sur la carte avant achat d’une pièce.