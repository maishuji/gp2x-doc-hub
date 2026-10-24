# Firmwares des consoles GamePark et GamePark Holdings

> **État des recherches — 23 octobre 2026**

Ce guide répertorie les principaux firmwares retrouvés pour la GP32, la GP2X F100, la GP2X F200, la GP2X F200(B), la Wiz et la Caanoo. Il indique leur compatibilité, leur provenance et, lorsque la notice d’origine a pu être vérifiée, leur méthode d’installation.

Le terme « GPX » ne correspond pas à un modèle identifié dans les archives consultées. Il est probablement employé ici pour parler de la **GP32**, prédécesseure de la GP2X. La GP32 reste une console distincte et ses firmwares ne sont compatibles avec aucun modèle GP2X, Wiz ou Caanoo.

## Avertissement critique

> **Flasher une mauvaise image, interrompre l’écriture ou perdre l’alimentation peut rendre la console inutilisable (« brick »). Les firmwares des différents modèles ne sont jamais interchangeables.**

Avant toute mise à jour :

1. identifier le modèle exact et, pour une F200, déterminer s’il s’agit d’une F200 d’origine ou d’une F200(B) ;
2. relever la version actuellement installée dans les informations système ;
3. vérifier que cette version satisfait le prérequis de la mise à jour choisie ;
4. lire intégralement le README livré dans l’archive avant de copier quoi que ce soit ;
5. utiliser des piles ou une batterie pleinement chargées, ou une alimentation stable adaptée à la console ;
6. employer une carte SD fiable et sauvegarder son contenu ;
7. ne jamais interrompre une mise à jour, même si l’affichage paraît figé ou brièvement corrompu.

Les archives référencées sont historiques. Leur présence sur OpenHandhelds ne constitue ni une garantie d’intégrité ni une validation sur matériel réel. Aucun flashage n’a été réalisé dans le cadre de cette recherche et aucune somme de contrôle indépendante n’a été retrouvée.

## Compatibilité en un coup d’œil

| Famille | Firmwares recensés | Compatibilité et restrictions principales |
|---|---|---|
| GP32 | Firmwares européens, AquaFish Multifirmware 5.01 et variantes spécialisées | GP32 uniquement. L’image dépend notamment du type d’écran — BLU, BLU+ ou FLU — et parfois d’une modification RAM 32 Mo. |
| GP2X F100 | Officiels 2.0.0, 2.1.2, 3.0.0, 4.0.0 et 4.1.0 ; Open2X DR7 F100 | Utiliser exclusivement une image F100. La mise à jour officielle 4.1.0 exige que la 4.0.0 soit déjà installée. |
| GP2X F200 d’origine | Officiels 4.0.0, 4.1.0 et 4.1.1 ; Open2X DR7 F200 | Ne pas utiliser le « full flash » 4.0.2 destiné à la F200(B) : il désactive notamment le tactile de l’ancienne F200. |
| GP2X F200(B) | Officiel 4.0.2 d’usine, puis mise à jour 4.1.0 | Le « full flash » 4.0.2 est réservé aux unités livrées d’usine en 4.0.2. La mise à jour 4.1.0 accepte ce point de départ. |
| GP2X Wiz | Officiels 1.1.0, 1.2.1 et 1.2.6 rev359 | Wiz uniquement. La 1.2.6 est la dernière version visible dans l’archive consultée. |
| GP2X Caanoo | Officiels 1.0.1, 1.0.6, 1.5.0, 1.6.0 et 1.6.1 rev167 | Caanoo uniquement. La notice 1.6.1 documente explicitement les départs depuis 1.5.0 et 1.0.1. |

## GP2X F100

### Firmwares disponibles

La [catégorie des firmwares GP2X sur OpenHandhelds](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42) contient plusieurs versions officielles historiques, notamment 2.0.0, 2.1.2, 3.0.0, 4.0.0 et 4.1.0.

| Version | Type | Où la trouver | Prérequis et remarques |
|---|---|---|---|
| 2.0.0, 2.1.2 et 3.0.0 | Officiel GPH | [Archive générale GP2X](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42) | Versions historiques. Lire la notice propre à chaque paquet avant installation. |
| 4.0.0 | Officiel GPH | [F100 4.0.0 — `F100_4.0.0_080307.zip`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2544) | F100 uniquement. Sert de version de départ obligatoire à la 4.1.0. |
| 4.1.0 | Officiel GPH | [F100 4.1.0 — `FW4.1.0-F100.zip`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2599) | **Exige la 4.0.0 déjà installée.** La notice avertit qu’un autre point de départ risque très probablement de briquer la GP2X. |
| Open2X DR7 | Communautaire | [Open2X DR7 F100 — `Open2x-DR7-F100.7z`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2751) | Paquet réservé à la F100 ; ne pas le remplacer par celui de la F200. |

### Installation

Les pages vérifiées pour les versions 4.0.0 et 4.1.0 établissent le modèle et les prérequis, mais ne fournissent pas une procédure complète suffisamment détaillée pour être reproduite ici sans ambiguïté. Après téléchargement :

1. ouvrir l’archive sans modifier son contenu ;
2. lire le README et identifier la méthode exacte prévue par cette version ;
3. vérifier une nouvelle fois que la console est une F100 et que le firmware courant satisfait le prérequis ;
4. suivre uniquement les instructions du paquet choisi.

Ne pas transposer la séquence de touches de la Wiz ou de la Caanoo à la F100.

## GP2X F200 et F200(B)

La désignation F200 recouvre au moins deux révisions qu’il faut distinguer avant tout flashage :

- l’ancienne **F200**, généralement associée au firmware 4.0.0 ;
- la **F200(B)** plus récente, livrée d’usine avec le firmware 4.0.2.

Si la révision n’est pas certaine, ne pas flasher avant de l’avoir identifiée à partir de l’étiquette, de la version système et de la documentation de l’appareil.

| Version | Cible | Où la trouver | Prérequis et remarques |
|---|---|---|---|
| 4.0.0 | F200 d’origine | [Archive générale GP2X](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42) | Ne pas installer sur une F200(B). |
| 4.0.2 « full flash » | F200(B) livrée en 4.0.2 | [Archive générale GP2X](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42) | Réservé à cette révision. La notice avertit que cette image casse le tactile sur l’ancienne F200. |
| 4.1.0 | F200 et F200(B) | [F200/F200(B) 4.1.0 — `FW4.1.0-F200.zip`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2600) | Ancienne F200 : partir de 4.0.0. F200(B) : partir de sa 4.0.2 d’usine. |
| 4.1.1 | F200 | [F200 4.1.1 — `F200_4(1).1.1_080613.zip`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2647) | La page confirme la cible F200, mais n’expose ni prérequis ni procédure détaillée. Examiner le paquet avant toute installation. |
| Open2X DR7 | F200 | [Open2X DR7 F200 — `Open2x-DR7-F200.7z`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2752) | Paquet distinct de celui de la F100. La compatibilité avec la révision F200(B) n’a pas été établie par les sources consultées. |

### Installation

Pour les firmwares officiels GP2X F200 et Open2X, la procédure complète n’a pas été confirmée dans une notice en ligne assez précise. Il faut donc suivre le README du paquet exact, après avoir vérifié le modèle et la version de départ indiqués dans le tableau.

En particulier, ne pas supposer que les archives F200 4.1.0 et 4.1.1 s’installent de manière identique, et ne pas utiliser l’image 4.0.2 comme mise à niveau générique d’une F200.

## Open2X DR7

Open2X est un firmware communautaire pour la **GP2X F100/F200**, et non un firmware universel de la gamme GPH.

- utiliser [`Open2x-DR7-F100.7z`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2751) uniquement sur une F100 ;
- utiliser [`Open2x-DR7-F200.7z`](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42,2752) uniquement sur une F200 dont la compatibilité exacte a été confirmée ;
- ne jamais installer ces paquets sur une GP32, une Wiz ou une Caanoo ;
- conserver une copie du firmware officiel correspondant et étudier les possibilités de restauration avant le flashage ;
- suivre le README inclus, car aucune procédure Open2X DR7 suffisamment complète n’a été confirmée dans les pages publiques consultées.

## GP2X Wiz

### Firmwares disponibles

La [catégorie Wiz d’OpenHandhelds](https://dl.openhandhelds.org/cgi-bin/wiz.cgi?0,0,0,0,42) conserve les versions historiques 1.1.0 et 1.2.1, ainsi que la version finale visible :

- [Wiz 1.2.6 rev359 — `GP2XWiz_Firmware_v1.2.6_rev359_20100915.zip`](https://dl.openhandhelds.org/cgi-bin/wiz.cgi?0,0,0,0,42,488).

Cette version annonce des améliorations de performances, des ajustements de l’horloge et de la gestion de la carte SD, ainsi que l’ajout de la connexion Wi-Fi.

### Installer la version 1.2.6

La notice GPH archivée décrit la procédure suivante :

1. charger complètement la batterie ou assurer une alimentation USB stable ;
2. utiliser une carte SD d’au moins 1 Go ;
3. copier les fichiers du firmware sur la carte SD conformément à l’arborescence de l’archive ;
4. insérer la carte dans la Wiz éteinte ;
5. maintenir le bouton `R`, puis allumer la console ;
6. relâcher `R` lorsque l’écran de mise à jour apparaît ;
7. attendre environ 15 minutes sans couper l’alimentation ;
8. redémarrer la console lorsque la mise à jour est terminée ;
9. recalibrer l’écran tactile dans les paramètres, car ses valeurs sont réinitialisées ;
10. supprimer les fichiers de mise à jour de la carte SD après le succès de l’opération.

La notice précise que certaines images peuvent être temporairement corrompues à l’écran pendant le patch ; l’affichage doit redevenir normal à la fin. En cas de message d’erreur, elle recommande d’éteindre puis de rallumer, et d’essayer une autre carte SD si le problème persiste.

## GP2X Caanoo

### Firmwares disponibles

La [catégorie Caanoo d’OpenHandhelds](https://dl.openhandhelds.org/cgi-bin/caanoo.cgi?0,0,0,0,42) contient notamment les versions 1.0.1 « full flash », 1.0.6, 1.5.0, 1.6.0 et 1.6.1 rev167.

- [Caanoo 1.6.0](https://dl.openhandhelds.org/cgi-bin/caanoo.cgi?0,0,0,0,42,638) ;
- [Caanoo 1.6.1 rev167 — `v1.6.1_rev167_caanoo.zip`](https://dl.openhandhelds.org/cgi-bin/caanoo.cgi?0,0,0,0,42,743).

La 1.6.1 est la dernière version visible dans cette archive. Elle annonce une amélioration de la séquence de démarrage interne et un changement du logo de démarrage.

### Installer la version 1.6.1 depuis la 1.5.0

1. décompresser le contenu de l’archive à la racine de la carte SD ;
2. insérer la carte dans la Caanoo éteinte ;
3. maintenir le bouton `R` pendant l’allumage ;
4. relâcher `R` lorsque le message `Firmware upgrading…` apparaît ;
5. attendre le message `Firmware Upgrade Complete` sans interrompre l’alimentation.

### Installer la version 1.6.1 depuis la 1.0.1

1. décompresser le contenu de l’archive à la racine de la carte SD ;
2. insérer la carte dans la Caanoo éteinte ;
3. maintenir `R` pendant l’allumage ;
4. relâcher `R` lorsque `Firmware upgrading…` apparaît ;
5. après le message `Bootloader was updated`, éteindre la console si la notice incluse le demande, puis répéter le démarrage en maintenant `R` ;
6. attendre le message `Firmware Upgrade Complete` sans interrompre l’alimentation.

La page officielle archivée documente explicitement ces deux points de départ. Pour une Caanoo équipée d’une autre version, ne pas inventer un chemin de mise à jour : consulter le README du paquet correspondant et, si nécessaire, appliquer les versions intermédiaires qu’il exige.

## GP32 : une console et des variantes distinctes

La [catégorie des firmwares GP32 sur OpenHandhelds](https://dl.openhandhelds.org/cgi-bin/gp32.cgi?0,0,0,0,42) contient notamment :

- AquaFish Multifirmware 5.01 en version standard ;
- une version AquaFish 5.01 **BLU+ distincte** ;
- un firmware européen pour les modèles BLU à écran Samsung ;
- un firmware européen pour les modèles BLU à écran taïwanais ;
- un EuroFirmware destiné aux modèles FLU ;
- des versions spéciales réservées aux GP32 modifiées avec 32 Mo de RAM ;
- `Flashtool`, décrit comme effectuant le flashage puis une vérification ;
- d’anciens outils de flashage qui, eux, ne vérifient pas l’écriture.

Il n’existe donc pas de procédure GP32 générique sûre. Il faut d’abord déterminer la variante d’écran — normale, FLU, BLU ou BLU+ — et savoir si la RAM a été modifiée. Une image destinée à une autre variante peut produire un affichage inutilisable ou endommager le fonctionnement de la console.

Aucune procédure suffisamment complète et corroborée n’a été retrouvée pendant cette passe. Le présent guide déconseille donc de flasher une GP32 sur la seule base du nom du fichier. Il faut obtenir la notice du firmware exact et privilégier un outil qui vérifie l’écriture.

## En cas d’échec

- Ne pas enchaîner des tentatives avec des images différentes au hasard.
- Noter le modèle, la révision, le firmware de départ, le nom exact de l’archive et le dernier message affiché.
- Vérifier d’abord la carte SD et son arborescence sur un ordinateur.
- Rechercher une procédure de récupération propre au modèle et au bootloader concerné.
- Les méthodes telles que JTAG, les images NAND ou les « NAND blankers » sont des outils de récupération experts : une procédure prévue pour une autre révision peut aggraver la panne.

Ce document ne fournit pas de procédure de débrick générique, car aucune méthode unique n’est sûre pour toute la gamme.

## Sources et limites

Sources principales consultées le 23 octobre 2026 :

- [OpenHandhelds — firmwares GP2X F100/F200](https://dl.openhandhelds.org/cgi-bin/gp2x.cgi?0,0,0,0,42) ;
- [OpenHandhelds — firmwares Wiz](https://dl.openhandhelds.org/cgi-bin/wiz.cgi?0,0,0,0,42) ;
- [OpenHandhelds — firmwares Caanoo](https://dl.openhandhelds.org/cgi-bin/caanoo.cgi?0,0,0,0,42) ;
- [OpenHandhelds — firmwares GP32](https://dl.openhandhelds.org/cgi-bin/gp32.cgi?0,0,0,0,42).

Les descriptions et notices originales sont anciennes, parfois succinctes, et les sections de commentaires modernes de l’archive contiennent du spam ; ces commentaires n’ont pas été utilisés comme sources. Les fichiers n’ont pas été exécutés, leur contenu n’a pas été audité et leur installation n’a été testée sur aucune console réelle.

Voir aussi le [répertoire des sites web consacrés à la GP2X](sites-web-gp2x.md) pour trouver d’autres archives et ressources historiques.
