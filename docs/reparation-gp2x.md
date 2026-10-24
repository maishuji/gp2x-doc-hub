# Diagnostic et réparation des consoles GP2X

Ce guide centralise les pannes et méthodes de diagnostic documentées pour les **GP2X F100 et F200**. La **Wiz** et la **Caanoo** appartiennent à la même famille commerciale, mais leur électronique et leurs pièces ne sont pas interchangeables par défaut. Les informations actuellement sourcées concernent surtout les F100/F200.

## Niveau de preuve

- **Constructeur** : manuel, schéma, logiciel ou consigne publiée par GamePark Holdings.
- **Communautaire historique** : ancien wiki ou forum ; utile, mais potentiellement incomplet ou obsolète.
- **Observation locale** : mesure réalisée sur un exemplaire précis, non généralisable sans comparaison.
- **Hypothèse** : piste à vérifier avant toute intervention.

La révision de la carte, le modèle exact et la source doivent toujours accompagner une référence de composant ou une mesure.

## Avant toute intervention

1. Identifier le modèle : F100, F200, Wiz ou Caanoo, puis photographier l'étiquette et la carte mère.
2. Noter le symptôme exact, la source d'alimentation, les accessoires connectés et ce qui déclenche ou interrompt la panne.
3. Sauvegarder la carte SD et, si la console démarre encore, éviter toute mise à jour de firmware pendant le diagnostic matériel.
4. Retirer les piles et débrancher l'alimentation avant toute mesure de continuité, résistance ou capacité.
5. Vérifier l'absence de tension et laisser les condensateurs se décharger avant une mesure hors tension.
6. Ne jamais mesurer la continuité d'une pile. Une NiMH a chauffé lors d'une telle erreur pendant le [diagnostic local d'une F200](GP2X_F200_hardware_diagnostic.md).
7. Sous tension, éviter tout pont entre les broches fines. Utiliser des pointes isolées et une masse fiable.
8. Ne remplacer aucun composant sur la seule base de son apparence ou d'une référence relevée sur une autre révision.

Une pile qui chauffe, fuit, gonfle ou paraît endommagée doit être isolée et remplacée selon les règles locales de collecte.

## Identifier la révision avant de réparer

L'[inventaire communautaire des versions matérielles](https://web.archive.org/web/20071013094214/http://wiki.gp2x.org/wiki/Hardware_Versions), archivé en 2007, décrit plusieurs F100 : First Edition Mk1, Mk1(B), Second Edition Mk1 et Mk2, puis annonce la F200.

Différences historiques utiles au diagnostic :

- Mk1(B) : remplacement signalé d'une résistance CMS marquée `654` par `614` pour réduire le scintillement ;
- Second Edition Mk1 : suppression de cales qui pouvaient presser le LCD, jack casque mieux soudé, contacts de piles modifiés et tige du joystick raccourcie ;
- Mk2 : joystick pivoté de 45°, capuchon concave et LCD différent, plus lumineux et moins sujet aux lignes ondulées ;
- F200 : écran tactile et croix directionnelle huit directions, sans clic central du joystick F100.

Ces descriptions sont communautaires et anciennes. Elles ne justifient pas de modifier une carte sans relever d'abord sa propre implantation.

## Diagnostic rapide par symptôme

| Symptôme | Contrôles non destructifs prioritaires | Pistes documentées |
|---|---|---|
| Aucun voyant, aucun démarrage | Piles chargées, polarité, contacts, interrupteur, jack et adaptateur vérifié | Contacts oxydés, jack ou interrupteur dessoudé/usé, étage d'alimentation |
| Voyant faible, écran noir sur piles | Comparer avec une paire NiMH saine puis avec une alimentation régulée correcte | Chute de tension, contacts de piles, convertisseur DC-DC ; voir l'étude de cas F200 |
| Écran noir mais son ou activité visible | Éclairage, nappe LCD, pression mécanique, différence entre démarrage à froid et à chaud | Nappe, rétroéclairage, LCD ou alimentation associée |
| Lignes, neige ou couleurs au démarrage | Essayer d'abord des piles saines et laisser la console éteinte avant un nouveau démarrage | Sous-tension, comportement de démarrage ancien, problème LCD selon révision |
| Taches claires ou sombres | Observer si elles changent lorsque la coque est contrainte, sans appuyer sur la dalle | Pression d'un élément derrière le LCD, rapportée sur certaines F100 |
| Pas de son au casque | Enfoncer complètement une fiche connue, tester un autre casque | Fiche mal engagée, jack dessoudé ou commutation du jack |
| Haut-parleurs actifs avec casque | Vérifier le jack et sa commutation mécanique | Soudure ou contact interne du jack sur certaines F100 |
| Direction F100 absente | Retirer seulement le capuchon et vérifier s'il frotte sur la coque | Capuchon trop bas, joystick déplacé ou usé |
| Joystick F100 mou | Manipulation très douce, sans forcer ; arrêter en cas de bruit de broyage | Mécanisme déplacé ou endommagé ; pièce candidate historique Alps |
| USB non reconnu | Autre câble mini-USB, autre port, écran USB de la console et carte SD insérée | Câble, ancien firmware ou système hôte |
| Carte SD invisible | Tester une carte connue, nettoyer les contacts, formater avec un lecteur externe | Compatibilité/formatage historique, connecteur ou firmware |
| Blocage après flash | Ne pas reflasher au hasard ; distinguer logo noir, écran blanc et absence totale d'activité | Firmware, U-Boot ou NAND ; récupération experte éventuelle |

## Alimentation

### F100

Deux sources communautaires historiques donnent pour l'entrée externe de la F100 **3,3 V**, environ **1 A**, avec **centre négatif** et une fiche annoncée `0,7 × 2,35 mm` : la page [External power supply](https://web.archive.org/web/20060222114348/http://wiki.gp2x.org/wiki/External_power_supply) et la page générale [GP2X](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/GP2X). Contrôler la polarité au multimètre avant connexion : une inversion peut détruire l'étage de conversion.

La page ancienne cite `U18`, marqué `LTSK`, et évoque le `LTC3402` comme remplacement. Cela ne constitue ni une équivalence garantie, ni une référence applicable à la F200, à la Wiz ou à la Caanoo.

Pour un défaut sur piles :

1. mesurer chaque pile séparément puis la paire sous charge ;
2. nettoyer les contacts sans abrasif agressif ;
3. rechercher ressort affaibli, corrosion, soudure fissurée ou chute de tension ;
4. comparer le comportement piles/secteur avec le même logiciel et sans périphérique ;
5. seulement ensuite suivre les rails d'alimentation à partir du schéma propre à la révision.

L'[ancienne liste de problèmes](https://web.archive.org/web/20080516010353/http://wiki.gp2x.org/wiki/GP2X_Issues) recommande historiquement des NiMH plutôt que les alcalines fournies. Cette page est elle-même marquée obsolète : elle sert ici d'inventaire de symptômes, pas de spécification moderne.

L'utilitaire communautaire [Battery Test 0.1](https://web.archive.org/web/20081007063632/http://wiki.gp2x.org/wiki/Battery_Test) était destiné à comparer objectivement l'autonomie et des variables associées. Sa page archivée ne conserve pas de protocole ni de téléchargement directement vérifiable : il peut servir de piste pour reproduire un test contrôlé, pas d'instrument étalonné. Pour comparer des piles, conserver la même console, le même firmware, la même fréquence CPU, la même luminosité et la même charge logicielle.

### F200

Le [journal de diagnostic matériel d'une F200](GP2X_F200_hardware_diagnostic.md) documente un jack DC instable, un interrupteur `SW1` défectueux et une panne restante limitée au fonctionnement sur piles. Sur cet exemplaire :

- un jack de type `DC-011C` a été monté avec succès, sans en faire une compatibilité universelle ;
- `U34` a été identifié comme `ACT6305UC-T` et `U35` comme `ACT6906UCADJ-T` ;
- `U27` est seulement considéré comme probablement `LTC3426` ;
- le défaut actuel de commande/conversion sur piles n'est pas encore attribué à un composant précis.

Les mesures détaillées, les fiches techniques et le protocole de reprise restent dans ce journal afin de ne pas généraliser un seul PCB.

### Wiz et Caanoo

Aucune spécification d'alimentation de réparation suffisamment corroborée n'est encore recensée ici. Ne pas utiliser un adaptateur ou un composant de F100/F200 par analogie. Relever l'étiquette constructeur, la polarité, le connecteur et la révision avant tout essai.

## Écran et tactile

Pour les F100, les archives communautaires signalent des lignes diagonales ou ondulées, du scintillement sur les premières révisions, des zones claires/sombres liées possiblement à une pression derrière le LCD et parfois deux films protecteurs superposés. Commencer par :

1. des piles saines ou une alimentation correcte ;
2. l'observation du défaut dès l'allumage et après démarrage ;
3. l'inspection de la nappe, de son verrou et des points de pression, appareil hors tension ;
4. une comparaison avec les défauts propres à la révision.

Ne pas presser la dalle et ne pas déverrouiller une nappe sans comprendre son connecteur. Pour la F200, séparer le diagnostic de l'affichage de celui de la couche tactile. La [page F-200](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/F-200) confirme l'écran tactile, mais ne fournit pas de procédure de réparation exploitable.

Les informations actuelles ne permettent pas de proposer une dalle de remplacement universelle pour F100, F200, Wiz ou Caanoo.

L'ancien [LCD Pixel Fixer 1.0](https://web.archive.org/web/20080829165034/http://wiki.gp2x.org/wiki/LCD_Pixel_Fixer) faisait défiler des images en boucle dans l'espoir de libérer un pixel bloqué. Sa propre page indique que la méthode n'avait pas été testée sur un pixel bloqué et qu'elle exigeait Python/Pygame. Elle ne peut pas réparer un pixel mort, une nappe, une dalle fissurée ou un défaut d'alimentation. Éviter une exécution prolongée sans surveillance et utiliser une alimentation stable.

## Audio

Sur certaines F100, le jack casque était insuffisamment soudé ; la révision Second Edition Mk1 aurait amélioré ce point. Avant d'ouvrir :

- tester un casque et un fichier connus ;
- insérer complètement la fiche, l'ajustement pouvant être serré ;
- vérifier séparément les haut-parleurs et les deux canaux du casque ;
- distinguer défaut matériel et ancien problème logiciel de routage/stéréo.

Après ouverture hors tension, inspecter les ancrages et soudures du jack à la loupe. Une reprise exige du flux, une panne adaptée et l'absence d'arrachement de piste ; ne pas chauffer longuement le connecteur.

## Contrôles

### Joystick F100

Le [joystick F100](https://web.archive.org/web/20060227235856/http://wiki.gp2x.org/wiki/GP2x_joystick) est un dispositif numérique à huit directions, pas un axe analogique. La [cartographie communautaire du PCB](https://web.archive.org/web/20060715072648/http://wiki.gp2x.org/wiki/Joystick_PCB_Layout) indique des contacts actifs à l'état bas, vus du dessus du PCB.

Si une direction manque, vérifier d'abord si le capuchon frotte contre la coque. Un joystick devenu mou peut parfois être remis en place par un mouvement très doux ; un bruit de broyage suggère au contraire un dommage et impose d'arrêter. L'ancien wiki cite l'Alps `RKJXL100401V` ou une pièce similaire, mais sa disponibilité et son équivalence électrique, mécanique et de brochage ne sont pas vérifiées.

La page [Joystick Alternatives](https://web.archive.org/web/20080708224341/http://wiki.gp2x.org/wiki/Joystick_Alternatives) recense des capuchons de remplacement et des transformations avec des pièces Xbox ou PSP. Elle ne fournit toutefois ni dimensions, ni brochage, ni procédure encore vérifiable. Ces exemples prouvent seulement que des modifications communautaires ont existé ; ils ne constituent pas des remplacements directs recommandés.

### Croix F200

La F200 utilise une croix huit directions et n'a pas le clic central de la F100. Les pages archivées évoquent des discussions de modification, sans procédure suffisamment précise pour être reproduite ici. Ne pas appliquer la cartographie ou une pièce du joystick F100 au D-pad F200.

## USB et carte SD

Pour une F100 qui n'apparaît pas sur l'ordinateur :

1. essayer un câble mini-USB de données connu ;
2. insérer une carte SD ;
3. ouvrir le mode de connexion USB sur la console ;
4. tester un lecteur de carte externe pour distinguer panne USB et panne SD ;
5. vérifier la version du firmware et consulter le [guide des firmwares](firmwares-gp2x.md).

L'ancien wiki attribue certains défauts aux premières versions de firmware. Il signale aussi des cartes et formatages incompatibles et recommande, si la console ne voit pas la carte, de la formater au moyen d'un lecteur externe. Sauvegarder les données avant tout formatage.

Pour une panne physique, inspecter poussière, broches pliées, soudure fissurée et arrachement du connecteur. Ne jamais redresser une broche sous tension.

## Ouverture, nettoyage et remontage

Aucune procédure de démontage complète et corroborée n'a encore été retrouvée. Avant ouverture :

- retirer piles, carte SD, stylet et câbles ;
- utiliser l'outil correspondant exactement aux vis ;
- classer les vis par emplacement et photographier chaque couche ;
- séparer les coques sans tirer sur les fils de haut-parleurs ni les nappes ;
- déverrouiller les connecteurs de nappe dans le bon sens ;
- nettoyer les contacts à l'alcool isopropylique adapté à l'électronique, en faible quantité, puis laisser sécher ;
- vérifier qu'aucun fil, mousse ou composant ne presse sur le LCD avant fermeture.

La page [GP2X Repair d'iFixit](https://www.ifixit.com/Device/GP2X) existe, mais ne contient actuellement ni guide ni question exploitable.

## Pièces et références recensées

| Élément | Modèle/révision | Référence ou description | État de la preuve |
|---|---|---|---|
| Joystick | F100 | Alps `RKJXL100401V` ou similaire | Candidate communautaire historique, équivalence non vérifiée |
| Convertisseur `U18` | Ancienne F100 | marquage `LTSK`, `LTC3402` évoqué | Remplacement rapporté, non universel |
| Jack DC | Un exemplaire F200 | type `DC-011C`, coudé CMS, fiche annoncée env. `2,5 × 0,7 mm` | Montage local réussi uniquement |
| Convertisseur `U34` | PCB F200 observé | Active-Semi `ACT6305UC-T`, SOT-23-5 | Identification locale confirmée |
| Régulateur `U35` | PCB F200 observé | Active-Semi `ACT6906UCADJ-T` | Identification locale confirmée |
| Convertisseur `U27` | PCB F200 observé | probablement `LTC3426` | Hypothèse à confirmer |
| Condensateur `C233` | PCB F200 observé | environ 100 µF ; `TPSD107M010R0065/0080` étudié | Valeur/candidate non validées comme remplacement |

Avant achat : mesurer dimensions, pas, hauteur, empreinte, brochage, tension, courant, ESR et température. Une référence « similaire » n'est pas une pièce compatible.

## Console bloquée après une mise à jour

Commencer par exclure les causes simples : piles faibles, adaptateur incorrect, carte SD défectueuse, fichiers destinés à un autre modèle et redémarrage trop rapide. Suivre ensuite le [guide des firmwares](firmwares-gp2x.md), qui distingue F100, F200, Wiz et Caanoo.

L'ancien guide [Unbrick Your GP2X](https://web.archive.org/web/20060209013437/http://wiki.gp2x.org/wiki/Unbrick_Your_GP2X) ne concernait que certaines F100 dont U-Boot restait intact et qui atteignaient encore le logo noir. Il avertissait de vérifier que la console était réellement briquée avant toute intervention.

Une [version plus complète du guide de récupération](https://web.archive.org/web/20080706201855/http://wiki.gp2x.org/wiki/Unbricking_a_GP2X) proposait cette classification historique :

- logo noir visible : U-Boot probablement encore actif ;
- écran de chargement visible : noyau probablement chargé, système de fichiers racine à suspecter en premier ;
- logo noir sans écran de chargement : noyau potentiellement endommagé ;
- aucun logo : panne d'alimentation, U-Boot/NAND ou matériel à distinguer avant toute écriture.

Ce guide déconseillait formellement le flash sur piles, recommandait une alimentation stable et une console série, et séparait les images U-Boot, noyau et système de fichiers. Ses images et commandes visaient d'anciens firmwares F100 : ne pas les appliquer directement à une F200, une Wiz, une Caanoo ou une autre version. Ne jamais flasher U-Boot pour réparer un simple système de fichiers corrompu.

Pour une corruption plus basse, les archives décrivent une récupération F100 par JTAG :

- [présentation du JTAG](https://web.archive.org/web/20060310211202/http://wiki.gp2x.org/wiki/JTAG) ;
- [chargement d'un U-Boot en RAM avec OpenOCD et GDB](https://web.archive.org/web/20071108173627/http://wiki.gp2x.org/wiki/Unbrick_Your_GP2X_With_Openocd) ;
- [ancienne procédure Windows par cradle/JTAG](https://web.archive.org/web/20071108135627/http://wiki.gp2x.org/wiki/Unbricking_a_GP2X_using_JTAG).

Ces procédures datent de 2006–2007, utilisent des versions anciennes d'OpenOCD/GDB, un port parallèle, une cradle ou un adaptateur EXT/JTAG et des images spécifiques. Elles peuvent aggraver une corruption ou écrire une image incompatible. Elles sont conservées comme références historiques, **pas comme tutoriel prêt à exécuter**. Vérifier le modèle, le brochage, les niveaux logiques, l'image U-Boot et la possibilité d'une sauvegarde NAND avant toute écriture. Les documents sur le [port EXT](gp2x-port-ext-reference.md) et la [cradle](gp2x-cradle-et-breakout-board.md) complètent le repérage matériel.

## Fiche de réparation à conserver

Pour rendre un cas réutilisable, noter :

```text
Modèle et révision :
Marquage de la carte :
Symptôme initial :
Source d'alimentation et mesures :
Carte SD / firmware :
Déclencheur mécanique ou thermique :
Contrôles effectués :
Mesures avec points de référence :
Intervention :
Résultat et limites :
Photos et sources :
```

Une réparation réussie sur secteur ne prouve pas que le chemin des piles fonctionne, et inversement.

## Sources principales et limites

- [Hardware Versions — ancien GP2X Wiki, capture du 13 octobre 2007](https://web.archive.org/web/20071013094214/http://wiki.gp2x.org/wiki/Hardware_Versions) : révisions F100 et différences rapportées.
- [GP2X Issues — ancien GP2X Wiki, capture du 16 mai 2008](https://web.archive.org/web/20080516010353/http://wiki.gp2x.org/wiki/GP2X_Issues) : inventaire historique marqué obsolète par le wiki.
- [External power supply — ancien GP2X Wiki, capture du 22 février 2006](https://web.archive.org/web/20060222114348/http://wiki.gp2x.org/wiki/External_power_supply) : alimentation F100 et risque d'inversion de polarité.
- [GP2x joystick](https://web.archive.org/web/20060227235856/http://wiki.gp2x.org/wiki/GP2x_joystick) et [Joystick PCB Layout](https://web.archive.org/web/20060715072648/http://wiki.gp2x.org/wiki/Joystick_PCB_Layout) : fonctionnement et contacts du joystick F100.
- [F-200 — ancien GP2X Wiki, capture du 23 juillet 2008](https://web.archive.org/web/20080723135020/http://wiki.gp2x.org/wiki/F-200) : différences générales de la F200.
- [Docs & Papers — ancien GP2X Wiki, capture du 5 janvier 2006](https://web.archive.org/web/20060105035416/http://wiki.gp2x.org/wiki/Hardware_Documentation) : index historique de documentation ARM et MMSP2.
- [Getting started with GP2X hardware hacking — capture du 11 mai 2008](https://web.archive.org/web/20080511184827/http://wiki.gp2x.org/wiki/Getting_started_with_GP2X_hardware_hacking) : composants F100, ports, NAND et avertissements de bas niveau.
- [GP2X Repair — iFixit](https://www.ifixit.com/Device/GP2X) : page appareil encore vide de guides au moment de la consultation.
- [Diagnostic local d'une GP2X F200](GP2X_F200_hardware_diagnostic.md) : observations et mesures sur un exemplaire, diagnostic encore en cours.

Les anciens wikis mêlent mesures, témoignages et suppositions. Les liens archivés peuvent perdre leurs téléchargements ou images. Aucune procédure de ce document ne vaut confirmation par le schéma et la révision de la console concernée.

## Journal des ajouts

- **24 octobre 2026** : ajout des archives Battery Test, LCD Pixel Fixer et Joystick Alternatives ; classification des niveaux de corruption F100 et précautions de reflash.
- **24 octobre 2026** : création du guide ; alimentation, écran, audio, contrôles, USB/SD, pièces recensées, précautions et récupération F100 par JTAG ; lien vers l'étude de cas F200.
