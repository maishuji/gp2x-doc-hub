# GP2X F200 — composants et architecture matérielle

> **État de la documentation — 27 octobre 2026**
>
> Cette fiche concerne la **GP2X F200** et signale séparément la révision communément appelée **F200(B)**. Elle ne doit pas servir à choisir un firmware sans identifier la révision exacte. Les références de composants observées sur une carte ne sont pas nécessairement universelles.

## Vue d’ensemble

La F200 est une évolution de la F100. Elle conserve le système sur puce **MagicEyes MMSP2 MP2520F**, ses deux cœurs ARM et l’architecture générale de mémoire, mais remplace le joystick par une croix directionnelle et ajoute un écran tactile résistif.

| Groupe | Caractéristique principale |
|---|---|
| Système sur puce | MagicEyes MMSP2 MP2520F |
| Processeur principal | ARM920T, environ 200 MHz |
| Coprocesseur | ARM940T programmable, environ 200 MHz |
| Mémoire vive | 64 Mio de SDRAM physiques |
| Stockage interne | 64 Mio de NAND flash |
| Stockage amovible | Carte SD ; SDHC dépendant du firmware et de la révision |
| Écran | TFT LCD tactile résistif, 3,5 pouces, 320 × 240, couleur 16 bits |
| Audio | Stéréo, haut-parleurs et sortie casque |
| Commandes | Croix directionnelle, écran tactile, boutons d’action et d’épaule |
| Connectique | USB, port EXT, casque, alimentation externe et logement SD |
| Alimentation | Deux AA ou alimentation externe régulée 3,3 V, centre négatif |

## Processeurs et périphériques intégrés

Le **MMSP2 MP2520F** est le même SoC de base que sur la F100 :

- l’ARM920T exécute Linux et les applications ;
- l’ARM940T sert de coprocesseur programmable et n’est pas un second CPU Linux symétrique ;
- le SoC fournit les contrôleurs SDRAM, NAND, écran, SD/MMC et USB host ;
- il intègre GPIO, UART, I²C, RTC, DMA, AC’97 et des blocs de traitement vidéo.

La fréquence nominale couramment documentée est d’environ **200 MHz** pour les deux cœurs. L’overclocking logiciel n’est pas garanti et peut provoquer instabilité, échauffement ou consommation excessive.

## Mémoire et stockage

La F200 conserve **64 Mio de SDRAM** et **64 Mio de NAND flash**. Comme sur la F100, la quantité de RAM exposée à Linux et la carte mémoire exacte dépendent du noyau et du firmware. La NAND contient bootloader, paramètres, noyau, système et espace interne selon le partitionnement installé.

La carte SD constitue le support amovible principal. Le support **SDHC** ne doit pas être déduit du seul matériel : il varie avec le firmware. La F200 d’origine et la F200(B) n’ont pas exactement le même chemin de mise à jour. Consulter le [guide des firmwares](firmwares-gp2x.md) avant toute opération.

## Affichage et écran tactile

L’écran intégré est un **TFT LCD 3,5 pouces** de **320 × 240 pixels**, généralement utilisé en couleur 16 bits. Contrairement à la F100, la F200 ajoute une dalle tactile résistive destinée à l’usage au stylet ou avec une pression physique.

Le tactile nécessite :

- un contrôleur matériel raccordé à la dalle ;
- un pilote adapté à la révision ;
- des données de calibration cohérentes avec l’écran.

Un firmware destiné à une autre révision peut désactiver ou dérégler le tactile. Le « full flash » officiel 4.0.2 est réservé à la F200(B) et ne doit pas être installé sur la F200 d’origine.

Le MMSP2 conserve ses contrôleurs framebuffer et ses fonctions de mise à l’échelle vidéo. La sortie télévision reste disponible via le port EXT et l’encodeur vidéo analogique de la plateforme, avec un câble adapté.

## Audio

La F200 utilise la chaîne audio stéréo du MMSP2 : interface AC’97, codec externe, amplification, deux haut-parleurs et prise casque 3,5 mm. Le port EXT peut également transporter l’audio destiné à une télévision ou une cradle.

Les sources communautaires signalent un niveau sonore différent ou plus élevé que sur la F100 dans certains logiciels. Cela peut révéler une différence d’amplification ou de configuration, mais ne suffit pas à identifier le codec exact de chaque carte. Lire le marquage réel avant une réparation au composant.

## Commandes et entrées

| Entrée | Fonction |
|---|---|
| Croix directionnelle | Quatre directions et diagonales par combinaisons |
| Écran tactile résistif | Pointage au stylet, après calibration |
| `A`, `B`, `X`, `Y` | Boutons d’action |
| `L`, `R` | Boutons d’épaule |
| `HOME` et `SELECT` | Commandes système ; `HOME` remplace l’appellation `START` de la F100 |
| `VOL+`, `VOL−` | Réglage du volume |

La F200 ne possède pas le clic central du joystick F100. Les logiciels qui l’utilisaient doivent proposer une autre combinaison ou être adaptés.

## USB, port EXT et sorties

Le connecteur USB vers PC et le port EXT demeurent présents, mais les fonctions logicielles ne sont pas identiques à celles de la F100. Les sources historiques signalent notamment l’absence des services réseau USB tels que telnet/Samba dans le firmware F200 standard et des différences de contrôleur USB selon la révision.

Le port EXT expose notamment :

- sortie S-Video et audio stéréo ;
- USB host et USB device ;
- UART 3,3 V ;
- JTAG et signaux de maintenance ;
- alimentation logique et masses.

Le port host ne fournit pas à lui seul un 5 V USB utilisable : une cradle ou breakout board correctement alimentée reste nécessaire. Voir la [référence du port EXT](gp2x-port-ext-reference.md).

## Alimentation

La F200 fonctionne avec deux piles ou accumulateurs AA, ou avec une alimentation externe régulée **3,3 V centre négatif**. La polarité est inhabituelle et une erreur peut endommager la console.

Le dépôt contient un [journal de diagnostic matériel F200](GP2X_F200_hardware_diagnostic.md). Il décrit une carte particulière et des mesures de réparation ; il ne constitue pas un schéma universel de toutes les F200.

## Différences essentielles avec la F100

- même famille MMSP2 et même résolution 320 × 240 ;
- écran tactile résistif ajouté ;
- croix directionnelle à la place du joystick numérique ;
- absence du clic joystick ;
- différences de firmware, d’USB et d’interface système ;
- révision F200(B) à distinguer impérativement pour le firmware et le tactile.

## Sources

- [GP2X Wiki archivé — F-200](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/F-200) : changements de commandes, tactile et incompatibilités observées.
- [GP2X Wiki archivé — fiche GP2X](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/GP2X) : architecture commune MMSP2, mémoire, écran et connectique.
- [Configuration Open2X F100/F200](https://sourceforge.net/p/open2x/code/HEAD/tree/trunk/kernel/linux-open2x-2.4.26_FW2-3/) : pilotes historiques de la plateforme MMSP2.
- [Firmwares GP2X F200 et F200(B)](firmwares-gp2x.md).
- [Référence technique du port EXT](gp2x-port-ext-reference.md).
- [Diagnostic matériel d’une F200](GP2X_F200_hardware_diagnostic.md).

Aucune F200 supplémentaire n’a été ouverte ou mesurée pour cette fiche. Confirmer la révision de carte et le marquage des puces avant toute réparation ou substitution.