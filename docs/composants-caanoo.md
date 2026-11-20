# Caanoo — composants et architecture matérielle

> **État de la documentation — 27 octobre 2026**
>
> La Caanoo est la génération qui suit la GP2X Wiz. Elle emploie également le SoC Pollux, mais sa mémoire, son écran, ses commandes, son audio et sa connectique diffèrent. Elle n’exécute pas directement tous les logiciels Wiz ou GP2X sans portage ou couche de compatibilité.

## Vue d’ensemble

| Groupe | Caractéristique principale |
|---|---|
| Système sur puce | MagicEyes Pollux VR3520F |
| Processeur | ARM926EJ-S, ARMv5TEJ, 533 MHz nominal |
| Accélération graphique | Moteur 3D intégré, OpenGL ES 1.1 |
| Mémoire vive | 128 Mio de DDR SDRAM, dont une zone réservée à la vidéo |
| Stockage système | Environ 128 Mio internes réservés au système selon les spécifications publiées |
| Stockage utilisateur | Carte SD/SDHC |
| Écran | TFT LCD tactile résistif, 3,5 pouces, 320 × 240 |
| Audio | Codec/DAC Wolfson WM1800 documenté, haut-parleurs, casque et microphone |
| Commandes | Joystick analogique, boutons, tactile, capteur de mouvement et vibration |
| USB | Port host USB 1.1 et liaison PC USB 2.0 via la connectique de la console |
| Alimentation | Batterie lithium-polymère interne d’environ 1 850 mAh |

## Processeur et graphisme

Le **MagicEyes Pollux VR3520F** intègre un cœur **ARM926EJ-S** à 533 MHz, une MMU, des caches et un accélérateur graphique annoncé compatible OpenGL ES 1.1. Il fournit aussi les contrôleurs de mémoire, affichage, NAND, SD, USB, audio et interfaces de périphériques.

La Caanoo partage donc une famille de SoC avec la Wiz, mais cela ne constitue pas une compatibilité binaire complète. Les bibliothèques, périphériques d’entrée, organisation du firmware et chemins de périphériques diffèrent.

L’overclocking logiciel au-delà de 533 MHz est rapporté dans la communauté, mais il n’est ni garanti ni sans coût : instabilité, consommation et température augmentent.

## Mémoire et stockage

La Caanoo dispose de **128 Mio de DDR SDRAM à 133 MHz** selon les spécifications publiées. Une partie, souvent décrite comme **16 Mio**, est réservée aux données vidéo ou textures ; la quantité disponible pour une application dépend du noyau et du système.

La documentation secondaire décrit environ **128 Mio de flash interne réservée au système**, sans stockage interne général comparable au gigaoctet NAND de la Wiz. Les applications et données utilisateur sont principalement placées sur carte **SD/SDHC**.

Les images de firmware Caanoo sont incompatibles avec celles de la Wiz et des GP2X F100/F200. Voir le [catalogue des firmwares](firmwares-gp2x.md).

## Affichage, tactile et sortie vidéo

La Caanoo utilise un **TFT LCD 3,5 pouces**, résolution **320 × 240 pixels**, avec dalle tactile résistive. Contrairement à l’AMOLED de la Wiz, son LCD emploie un rétroéclairage.

Le tactile doit être calibré après certaines mises à jour de firmware. Une dérive peut venir de la calibration, de la nappe, de la dalle résistive ou du contrôleur ; elle ne prouve pas immédiatement que le LCD est défectueux.

Pollux intègre une sortie vidéo analogique NTSC/PAL. Les spécifications secondaires indiquent une sortie composite pouvant produire 720 × 480 ou 720 × 576 entrelacé avec l’accessoire approprié. Le brochage et le câble ne doivent pas être assimilés au port EXT 24 broches de la F100/F200.

## Audio

Les spécifications publiques identifient un codec ou DAC stéréo **Wolfson Microelectronics WM1800**. La console comprend :

- haut-parleurs stéréo ;
- prise casque ;
- microphone intégré ;
- lecture audio PCM selon les capacités du pilote et du logiciel.

Pour une réparation, confirmer le marquage du composant et distinguer codec, amplificateur, prise casque, microphone et haut-parleurs.

## Commandes, capteurs et retour utilisateur

| Entrée ou périphérique | Fonction |
|---|---|
| Joystick analogique | Déplacement avec mesure analogique, distinct d’une croix numérique |
| `A`, `B`, `X`, `Y` | Boutons d’action |
| `L`, `R` | Boutons d’épaule |
| Boutons système | Navigation du firmware selon l’application ; consulter le marquage du boîtier pour leur libellé |
| Écran tactile résistif | Pointage au stylet ou par pression |
| Microphone | Capture audio intégrée |
| G-sensor | Détection de mouvement ou d’inclinaison |
| Moteur de vibration | Retour haptique commandé par le logiciel |

Le joystick analogique de la Caanoo ne doit pas être traité comme le joystick numérique 8 directions de la F100. Un portage doit lire les axes et appliquer une zone morte adaptée.

## USB et réseau

La Caanoo expose un port USB host standard de génération USB 1.1 pour certains périphériques et une liaison USB 2.0 vers un ordinateur via sa connectique. La disponibilité d’un périphérique dépend toujours du pilote du noyau et de son alimentation.

La console ne possède pas de Wi-Fi intégré. Le réseau sans fil officiel passe par un **dongle USB** vendu séparément ; un autre adaptateur n’est utilisable que si son chipset est pris en charge par le firmware.

## Alimentation

La batterie interne est une lithium-polymère d’environ **1 850 mAh**, avec une autonomie publiée d’environ cinq à six heures selon l’usage. Cette valeur n’est pas garantie sur une batterie ancienne.

Ne pas recharger une batterie gonflée ou endommagée. Un remplacement doit respecter tension, polarité, dimensions, connecteur, sonde éventuelle et circuit de protection.

## Différences essentielles avec la Wiz

- même famille Pollux et CPU ARM926EJ-S à 533 MHz ;
- 128 Mio de DDR au lieu de 64 Mio ;
- LCD tactile 3,5 pouces au lieu de l’AMOLED 2,8 pouces ;
- joystick analogique ;
- capteur de mouvement et moteur de vibration ;
- stockage utilisateur principalement sur SD/SDHC ;
- connectique, firmware et couche d’entrée propres à la Caanoo.

## Sources

- [Manuel anglais de la Caanoo dans OpenHandhelds](https://dl.openhandhelds.org/cgi-bin/caanoo.cgi?0,0,0,0,23) : archive répertoriant le manuel, le SDK et les logiciels historiques. Le lien mène à la catégorie contenant le PDF.
- [Wikipedia francophone — GP2X Caanoo](https://fr.wikipedia.org/wiki/GP2X_Caanoo) : spécifications détaillées Pollux, RAM, écran, audio, capteurs, USB et batterie ; source secondaire à recouper avant réparation.
- [Wikipedia anglophone — Caanoo](https://en.wikipedia.org/wiki/Caanoo) : historique et limites de compatibilité logicielle.
- [Catalogue des firmwares](firmwares-gp2x.md) : versions Caanoo et procédure de mise à jour.
- [magiceyes](https://github.com/zdiemer/magiceyes) : distinction contemporaine entre MMSP2 et Pollux et travail d’interopérabilité ; ce n’est pas une source constructeur.

Aucune Caanoo n’a été ouverte ou mesurée pour cette fiche. Les références de composants et le câblage doivent être confirmés sur la carte avant réparation.