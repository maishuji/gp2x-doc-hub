# GP2X F100 — composants et architecture matérielle

> **État de la documentation — 27 octobre 2026**
>
> Cette fiche concerne la **GP2X F100**, y compris ses révisions historiques First Edition, Normal Edition et MK2 lorsqu’aucune différence n’est précisée. Elle ne décrit pas la F200, la Wiz ou la Caanoo. Les références exactes de certains composants peuvent varier selon la révision de la carte mère ; ouvrir une console reste nécessaire pour établir la nomenclature d’un exemplaire particulier.

## Vue d’ensemble

La GP2X F100 est construite autour du système sur puce **MagicEyes MMSP2 MP2520F**. Ce composant réunit deux cœurs ARM, les contrôleurs mémoire et écran, des fonctions multimédias et de nombreuses interfaces. Il est complété par de la SDRAM, une mémoire NAND, un écran LCD, un codec audio, un contrôleur USB périphérique et un encodeur vidéo analogique.

| Groupe | Caractéristique principale |
|---|---|
| Système sur puce | MagicEyes MMSP2 MP2520F |
| Processeur principal | ARM920T, ARM 32 bits, environ 200 MHz à la fréquence nominale de la console |
| Coprocesseur | ARM940T programmable, environ 200 MHz |
| Mémoire vive | 64 Mio de SDRAM physiques |
| Stockage interne | 64 Mio de NAND flash |
| Stockage amovible | Carte SD ; interface SD/MMC et capacités SDIO matérielles |
| Écran | TFT LCD 3,5 pouces, 320 × 240 pixels, couleur 16 bits |
| Audio | Stéréo, codec AC’97, haut-parleurs et sortie casque |
| Commandes | Joystick numérique 8 directions avec clic, boutons d’action et de système |
| Connectique principale | USB device, port EXT, casque, alimentation externe et logement SD |
| Alimentation | Deux piles/accumulateurs AA ou alimentation externe régulée 3,3 V, centre négatif |

## Processeurs et système sur puce

### ARM920T principal

L’**ARM920T** exécute Linux et la majorité du code applicatif. C’est un cœur ARMv4T 32 bits doté d’une unité de gestion mémoire (MMU) et de caches. Les configurations du noyau Open2X pour F100 activent explicitement `CONFIG_CPU_ARM920T` et `CONFIG_ARCH_MMSP2`.

La fréquence communément documentée pour la GP2X est **200 MHz**. Un journal de démarrage historique mesure un `FCLK` proche de 199 MHz. Certains logiciels modifient cette horloge, mais l’overclocking n’est pas une caractéristique garantie : sa stabilité varie selon l’exemplaire et augmente la consommation et l’échauffement.

### ARM940T secondaire

L’**ARM940T** est un second cœur programmable intégré au MMSP2. Il n’exécute pas le système Linux principal comme un second processeur symétrique. Le logiciel doit charger du code dédié et communiquer avec lui par les mécanismes du SoC et la mémoire partagée.

Les firmwares historiques l’utilisent notamment dans la chaîne vidéo. Des programmes homebrew l’ont aussi employé pour du décodage audio, du rendu ou d’autres tâches spécialisées. Sa présence ne signifie donc pas qu’une application Linux ordinaire bénéficie automatiquement de deux CPU.

### Périphériques intégrés au MMSP2

Le MMSP2 apporte notamment les blocs utilisés par les pilotes historiques suivants :

- contrôleur SDRAM et contrôleur NAND ;
- contrôleurs d’affichage et framebuffer RGB ;
- processeur vidéo/post-traitement pour la mise à l’échelle et les plans d’image ;
- contrôleur SD/MMC ;
- contrôleur USB host OHCI ;
- interface audio AC’97 ;
- GPIO pour les boutons et autres signaux de carte ;
- UART, I²C, RTC, temporisateurs, interruptions et DMA ;
- interface de communication avec l’ARM940T.

Cette liste décrit les fonctions visibles dans le noyau historique et la documentation MMSP2. Leur disponibilité réelle dépend du câblage de la F100 et des pilotes du firmware.

## Mémoire vive et stockage

### SDRAM

La console possède **64 Mio de SDRAM physiques**. Il faut toutefois distinguer la quantité installée de la mémoire présentée au noyau : un journal de démarrage F100 historique annonce seulement **32 Mio à Linux**. L’autre partie peut être réservée par l’organisation mémoire du firmware, les tampons vidéo ou les usages du second cœur.

La répartition exacte n’est donc pas une constante de toutes les piles logicielles. Un firmware ou un noyau modifié peut employer une autre carte mémoire ; le développement bas niveau doit se fonder sur son propre bootloader, son noyau et son script d’édition de liens plutôt que sur la seule capacité physique.

### NAND interne

La F100 possède **64 Mio de NAND flash** pour le bootloader, ses paramètres, le noyau, le système de fichiers et une zone de stockage interne. Un ancien journal Open2X montre une NAND Samsung de 64 Mio découpée en plusieurs partitions MTD, mais cette référence de puce et ce partitionnement ne doivent pas être supposés identiques sur toutes les révisions ou tous les firmwares.

Écrire directement dans la NAND ou remplacer le bootloader peut rendre la console inutilisable. Pour un développement moderne, privilégier les essais depuis une carte SD et conserver une console série fonctionnelle avant toute modification persistante.

### Carte SD

Le logement supérieur accepte les cartes **Secure Digital**. Les firmwares d’origine ont connu différentes limites de capacité et de système de fichiers ; il ne faut pas attribuer automatiquement à la F100 le support SDHC documenté pour des firmwares ou modèles ultérieurs. Le contrôleur possède des capacités SD/MMC et la documentation historique mentionne un support SDIO matériel, mais un périphérique SDIO nécessite également un pilote compatible.

## Affichage et vidéo

### Écran intégré

La F100 utilise un écran **TFT LCD de 3,5 pouces** :

- résolution physique de **320 × 240 pixels** (QVGA) ;
- couleur **16 bits**, soit jusqu’à 65 536 couleurs ;
- framebuffer historique couramment configuré en RGB 16 bits ;
- rétroéclairage piloté par l’électronique de la console ;
- **aucune dalle tactile** sur la F100.

Le GP2X Wiki historique identifie une dalle `TS35ND2501`, mais cette référence communautaire doit être confirmée sur la révision étudiée avant de commander une pièce de remplacement.

### Traitement vidéo

Le MMSP2 comporte des fonctions multimédias dédiées : contrôleurs de plans vidéo, mise à l’échelle et post-traitement. Le firmware officiel s’en sert pour lire des vidéos dont la résolution peut dépasser celle de l’écran puis les redimensionner vers 320 × 240. Les formats effectivement pris en charge dépendent du lecteur et des codecs du firmware, pas seulement du matériel.

### Sortie télévision

Un encodeur analogique **Conexant CX25874** est documenté par les sources historiques et apparaît dans la configuration du noyau F100. Il fournit une sortie télévision via le port `EXT`, notamment en S-Video avec le câble approprié. La documentation commerciale historique annonce des modes allant jusqu’à 720 × 480, mais l’application doit configurer la sortie et toutes les applications ne la gèrent pas automatiquement.

Le port EXT transporte séparément la luminance `Y`, la chrominance `C`, les canaux audio stéréo et leurs masses. Consulter la [référence du port EXT](gp2x-port-ext-reference.md) avant de fabriquer un câble.

## Audio

La chaîne audio repose sur l’interface **AC’97** du MMSP2 et un codec que les journaux historiques identifient comme un **Wolfson WM9711/WM9712**. Comme le journal ne distingue pas les deux références et que les révisions matérielles peuvent différer, contrôler le marquage de la puce pour une réparation au composant.

Les fonctions accessibles comprennent :

- lecture stéréo, deux canaux gauche et droit ;
- échantillons jusqu’à 16 bits dans la pile historique ;
- fréquences d’échantillonnage documentées de 8 à 48 kHz ;
- deux haut-parleurs intégrés ;
- sortie casque stéréo sur prise jack 3,5 mm ;
- sortie audio stéréo sur le port EXT pour un câble TV ou une cradle ;
- réglage du volume par deux boutons dédiés.

La F100 ne doit pas être décrite comme possédant une entrée microphone externe utilisable sans preuve propre à la carte : les capacités générales du codec AC’97 ne garantissent pas que chaque entrée soit câblée sur la console.

## Commandes et entrées utilisateur

La F100 se distingue de la F200 par son **joystick numérique**, et non par une croix directionnelle ou un écran tactile.

| Entrée | Fonction matérielle |
|---|---|
| Joystick | Huit directions numériques ; la tige peut être enfoncée comme bouton |
| `A`, `B`, `X`, `Y` | Quatre boutons d’action en façade |
| `L`, `R` | Deux boutons d’épaule |
| `START`, `SELECT` | Deux boutons de système en façade |
| `VOL+`, `VOL−` | Réglage du volume |
| Interrupteur d’alimentation | Mise sous tension et hors tension matérielle |

Les boutons sont lus par les GPIO du MMSP2 et exposés par le pilote d’entrée GP2X. Les variantes First Edition, Normal Edition et MK2 peuvent différer mécaniquement, en particulier autour du capuchon et du comportement du joystick, sans changer le principe d’une entrée numérique.

## Connecteurs et périphériques externes

### USB device

Le connecteur USB accessible sur la console permet principalement de relier la GP2X à un ordinateur. Les sources matérielles historiques identifient un contrôleur périphérique USB 2.0 **PLX NET2272** sur la F100. Selon le firmware, cette liaison peut servir au stockage de masse, au réseau USB ou à d’autres fonctions gadget.

« USB 2.0 » décrit ici le contrôleur périphérique relié au PC. Cela ne signifie pas que le port USB host du MMSP2 fonctionne à haut débit USB 2.0.

### USB host sur le port EXT

Le MMSP2 fournit un contrôleur **USB host OHCI**, donc USB 1.1 full-speed/low-speed, dont les lignes `D+` et `D−` sont disponibles sur le port EXT. La prise ne fournit pas directement une alimentation USB 5 V conforme : un câble, une breakout board ou une cradle doit fournir le 5 V aux périphériques, avec une alimentation et une protection adaptées.

Le noyau doit également posséder les pilotes nécessaires. Une souris ou un clavier HID est plus simple à prendre en charge qu’un périphérique nécessitant un pilote absent du Linux historique.

### Port EXT 24 broches

Le connecteur inférieur expose plusieurs groupes de signaux :

- vidéo S-Video et audio stéréo ;
- USB host et USB device ;
- UART logique 3,3 V ;
- JTAG ;
- alimentation logique 3,3 V et masses ;
- un signal de reset dont le comportement peut varier selon la révision.

Ce connecteur permet l’usage d’une cradle ou d’une breakout board. Il ne faut jamais raccorder directement un port série RS-232 à l’UART 3,3 V, ni injecter 5 V sur les broches 3,3 V. Voir le [guide de la cradle et des breakout boards](gp2x-cradle-et-breakout-board.md) ainsi que la [référence détaillée du port EXT](gp2x-port-ext-reference.md).

### Autres connecteurs

- logement de carte SD sur le bord supérieur ;
- prise casque stéréo 3,5 mm ;
- entrée d’alimentation externe 3,3 V, centre négatif ;
- compartiment pour deux piles ou accumulateurs AA.

La polarité centre négatif est inhabituelle. Vérifier la tension, la polarité et la régulation avant de brancher une alimentation moderne.

## Ce que la F100 ne possède pas

Pour éviter les confusions avec les modèles suivants :

- pas d’écran tactile intégré ;
- pas de croix directionnelle de type F200 : la F100 emploie un joystick numérique ;
- pas de Wi-Fi ou Bluetooth intégré ;
- pas de stockage SDHC à présumer sans vérifier le firmware et le pilote ;
- pas de compatibilité matérielle ou firmware automatique avec la Wiz ou la Caanoo.

Des périphériques USB réseau ou Bluetooth peuvent être raccordés par une interface host correctement alimentée, mais seulement si le noyau fournit le pilote correspondant.

## Révisions et limites de cette fiche

Les appellations **First Edition**, **Normal Edition** et **MK2** recouvrent des modifications de fabrication et d’ergonomie. Les changements documentés incluent notamment le joystick et certains comportements du port EXT. Cette fiche décrit l’architecture commune ; elle ne constitue pas une nomenclature de réparation par révision.

Avant de remplacer un écran, un codec, une NAND ou un composant d’alimentation :

1. relever le modèle et les inscriptions extérieures ;
2. photographier la carte mère et noter sa révision ;
3. lire le marquage réel du composant ;
4. comparer brochage, tension et boîtier à une fiche technique ;
5. ne pas commander une pièce sur la seule base d’une référence communautaire.

## Sources

### Sources techniques primaires ou proches du matériel

- [Configuration Open2X du noyau Linux 2.4.26 pour F100](https://sourceforge.net/p/open2x/code/HEAD/tree/trunk/kernel/linux-open2x-2.4.26_FW2-3/.config.f100) : architecture MMSP2/ARM920T, pilotes NAND, framebuffer, audio AC’97, SD, USB, entrée GP2X, UART et CX25874.
- [Arbre du noyau Open2X](https://sourceforge.net/p/open2x/code/HEAD/tree/trunk/kernel/) : pilotes et configurations historiques de la plateforme.
- [GP2X Wiki archivé — Docs and Papers](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/Hardware_Documentation) : index historique des manuels ARM, documentation MMSP2, NET2272, encodeur vidéo et codec audio.
- [GP2X Wiki archivé — USB Host](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/USB_Host) : câblage host, nécessité d’une alimentation USB 5 V externe et journal de démarrage F100 détaillé.

### Sources secondaires et communautaires

- [GP2X Wiki archivé — fiche GP2X](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/GP2X) : dimensions, commandes, écran, connecteurs et caractéristiques audio/vidéo publiées par la communauté.
- [Wikipedia — GP2X](https://en.wikipedia.org/wiki/GP2X) : synthèse historique et caractéristiques générales, utilisée comme source secondaire.
- [Référence du port EXT maintenue dans ce dépôt](gp2x-port-ext-reference.md).
- [Sources techniques pour la migration du firmware F100](sources-techniques-migration-f100.md).

Aucune console n’a été ouverte ni mesurée pour cette fiche. Les capacités logicielles citées proviennent des sources historiques ; elles ne valent pas validation de chaque firmware, périphérique ou révision matérielle.
