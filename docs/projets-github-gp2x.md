# Inventaire des projets et ressources liés à la GP2X

> **Première version — 23 octobre 2026**

Cet inventaire recense des projets et ressources publics dont la description, le nom ou les informations disponibles indiquent un lien avec la famille GP2X. Il couvre notamment GitHub, GitLab et SourceForge. Il sert de point de départ pour explorer l’écosystème et préserver des références, pas de catalogue exhaustif ni de garantie que chaque projet se compile ou fonctionne encore.

## Périmètre et méthode

Les recherches ont été effectuées sur [GitHub](https://github.com/search?q=GP2X&type=repositories), [GitLab](https://gitlab.com/explore/projects?name=gp2x) et le [répertoire SourceForge](https://sourceforge.net/directory/?q=gp2x). Les entrées ci-dessous ont été retenues lorsque leur lien avec GP2X apparaissait clairement dans la page du projet ou sa documentation. Certaines pages décrivent des projets historiques ; leur présence en ligne ne prouve ni une maintenance actuelle ni la disponibilité d’un binaire utilisable.

La famille GP2X comprend plusieurs appareils (notamment GP2X F100/F200, Wiz et Caanoo). Les projets pour Wiz/Caanoo ou les portages vers d’autres plateformes sont signalés comme tels : leur présence ne signifie pas qu’ils sont compatibles avec la F100/F200. La colonne d’activité donne le dernier push affiché par l’API GitHub, le dernier commit exposé par l’API GitLab, ou la date du fichier le plus récent dans le flux RSS SourceForge consulté (selon la plateforme). Pour GitLab, un import peut donner une date de dépôt postérieure au dernier changement d’origine. Pour SourceForge, l’absence de publication dans le flux ne signifie pas nécessairement que le projet n’a jamais été mis à jour. Ces dates ont été relevées le 23 octobre 2026. Les résumés s’appuient sur les informations publiques des projets ; ils ne constituent pas des essais sur matériel. Vérifier le README, les prérequis, la licence, le modèle ciblé et la date des derniers changements avant réutilisation. Un miroir ou une copie d’un projet déjà listé est signalé afin de ne pas le compter comme projet distinct.

## Outils système, développement et matériel

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [devkitPro/gp2x-core](https://github.com/devkitPro/gp2x-core) | Développement | Dernier push : 2020-08-13 | Composants de base devkitARM pour la prise en charge GP2X. |
| [devkitPro/gp2x-tools](https://github.com/devkitPro/gp2x-tools) | Développement | Dernier push : 2020-07-04 | Outils associés à la chaîne GP2X de devkitPro. Consulter le dépôt pour le détail des composants. |
| [devkitPro/gp2x-examples](https://github.com/devkitPro/gp2x-examples) | Développement | Dernier push : 2022-01-27 | Exemples de projets GP2X. |
| [maishuji/gp2x-cmake-sdl-simple-test](https://github.com/maishuji/gp2x-cmake-sdl-simple-test) | Développement | Dernier push : 2025-11-09 | Exemple minimal d’application GP2X utilisant l’ancienne chaîne officielle, CMake et SDL. |
| [devkitPro/orcus](https://github.com/devkitPro/orcus) | Bibliothèque bas niveau | Dernier push : 2022-02-02 | Bibliothèque bare-metal GP2X, annoncée comme fonctionnant sans Linux. |
| [Orkie/open2x-bootloader](https://github.com/Orkie/open2x-bootloader) | Démarrage / firmware | Dernier push : 2020-11-14 | Projet de bootloader Open2x lié à GP2X. Vérifier les modèles et les instructions avant toute utilisation : un bootloader peut rendre l’appareil inutilisable s’il est mal choisi ou mal installé. |
| [Gatito-Miau/FX00_Battery_Meter](https://github.com/Gatito-Miau/FX00_Battery_Meter) | Matériel / logiciel | Dernier push : 2024-03-22 | Indicateur de batterie pour GP2X F100 et F200, selon la description du dépôt. |
| [sammynilla/gp2x-wiz](https://github.com/sammynilla/gp2x-wiz) | Documentation | Dernier push : 2023-02-12 | Base de connaissances GP2X/Wiz compilée à partir de sources trouvées en ligne et d’archives Web. C’est un projet de documentation, pas un logiciel système. |
| [Jacob-Matthew-Cook/GPort2X](https://github.com/Jacob-Matthew-Cook/GPort2X) | Compatibilité / exécution | Dernier push : 2026-10-10 | Projet de couche de compatibilité en espace utilisateur pour exécuter logiciels et images de cartes GP2X sur Linux moderne et appareils ARM. Projet distinct d’un émulateur matériel complet ; voir sa documentation pour les limites. |

## Interfaces et logiciels système

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [mtorromeo/gmenu2x](https://github.com/mtorromeo/gmenu2x) | Menu système | Dernier push : 2018-07-18 | Menu pour appareils embarqués, développé à l’origine pour GP2X. GitHub marque ce dépôt comme archivé. |
| [Cl0v3rDev/gp2xmenu](https://github.com/Cl0v3rDev/gp2xmenu) | Menu système | Dernier push : 2021-01-18 | Fork du code source de gp2xmenu. Vérifier le contexte et l’origine du fork avant de le comparer au logiciel officiel. |
| [alxm/inimaker](https://github.com/alxm/inimaker) | Utilitaire / Wiz | Dernier push : 2016-08-07 | Application GP2X Wiz pour créer et nettoyer des fichiers INI utilisés par le menu des jeux. Le dépôt GitHub est marqué comme archivé. |
| [tcolar/Stylus2X](https://github.com/tcolar/Stylus2X) | Utilitaire / écran tactile | Dernier push : 2013-09-06 | Scripts Python et outils pour l’écran tactile de GP2X, dont un clavier à l’écran. Confirmer le modèle cible avant usage. |
| [Ryochan7/YouTubed-2x](https://github.com/Ryochan7/YouTubed-2x) | Outil PC | Dernier push : 2011-04-21 | Téléchargeur/convertisseur vidéo produisant des fichiers compatibles GP2X ; ce n’est pas une application à exécuter sur la console. Le dépôt est marqué comme archivé. |
| [misato/RomSelector](https://github.com/misato/RomSelector) | Utilitaire / Wiz | Dernier push : 2019-11-04 | Sélecteur de ROM pour GP2X/Wiz, selon la description. |

## Émulation et compatibilité de jeux

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [notaz/ginge](https://github.com/notaz/ginge) | Compatibilité / émulation | Dernier push : 2016-01-16 | « GINGE Is Not Gp2x Emulator » : projet de compatibilité GP2X. Le nom précise qu’il ne s’agit pas d’un émulateur GP2X au sens classique. |
| [Orkie/dolos2x](https://github.com/Orkie/dolos2x) | Émulation | Dernier push : 2021-05-09 | Dépôt intitulé « GP2X Emulator ». Vérifier le niveau d’émulation et son état dans le README. |
| [zdiemer/magiceyes](https://github.com/zdiemer/magiceyes) | Émulation | Dernier push : 2026-09-21 | Émulateur multiplateforme annoncé pour GP2X F100/F200, Wiz et Caanoo, construit autour de Unicorn. Dépôt actif au moment de la consultation ; compatibilité réelle par titre à vérifier. |
| [zdiemer/magiceyes-compat](https://github.com/zdiemer/magiceyes-compat) | Compatibilité | Dernier push : 2026-08-26 | Suivi de compatibilité par titre pour magiceyes (GP2X/Wiz/Caanoo). Complément au projet précédent, pas un émulateur indépendant. |
| [gameblabla/cps2emu](https://github.com/gameblabla/cps2emu) | Émulateur / port | Dernier push : 2018-07-11 | Ancien émulateur CPS-2 publié à l’origine pour GP2X. |
| [SONIC3D/PicoDrive](https://github.com/SONIC3D/PicoDrive) | Émulateur / port | Dernier push : 2014-02-03 | Copie de sources PicoDrive provenant du site de notaz ; l’adéquation avec la GP2X et la provenance exacte des changements sont à vérifier dans le dépôt. |
| [j2me-preservation/phoneME-GP2X-SDL](https://github.com/j2me-preservation/phoneME-GP2X-SDL) | Java / émulation | Dernier push : 2021-04-11 | Miroir d’une version SDL de phoneME pour GP2X, GP2X Wiz et Caanoo, indiqué comme publié « tel que trouvé ». |

## Jeux, moteurs et bibliothèques

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [theZiz/sparrow3d](https://github.com/theZiz/sparrow3d) | Moteur 3D | Dernier push : 2021-03-19 | Rendu logiciel destiné à plusieurs consoles portables ouvertes, dont GP2X, Wiz, Caanoo et Pandora. |
| [digisims/WizEngine](https://github.com/digisims/WizEngine) | Moteur de jeu | Dernier push : 2019-01-09 | Moteur pour GP2X Wiz et Caanoo ; ne pas en déduire une compatibilité avec F100/F200. |
| [lschmid83/Dragon-Island](https://github.com/lschmid83/Dragon-Island) | Jeu | Dernier push : 2024-07-12 | Jeu de plateforme 2D et éditeur de niveaux ; la description cite PC, Android, GP2X et PSP. |
| [dedeibel/GP2X-of-life](https://github.com/dedeibel/GP2X-of-life) | Jeu | Dernier push : 2009-10-04 | Jeu de la vie pour console GP2X ou Linux avec SDL. |
| [gp2xdev/gp2x-flobopuyo](https://github.com/gp2xdev/gp2x-flobopuyo) | Jeu | Dernier push : 2015-07-22 | Portage GP2X du jeu Linux FloboPuyo. |
| [ledow/stppc2x](https://github.com/ledow/stppc2x) | Jeu / port | Dernier push : 2024-06-04 | Portage GP2X de la collection de casse-têtes portables de Simon Tatham. |
| [ant512/Chroma-X](https://github.com/ant512/Chroma-X) | Jeu | Dernier push : 2019-09-29 | Shoot’em up vertical SDL/GP2X utilisant une palette de quatre couleurs. |
| [tonsomo/AlephOne](https://github.com/tonsomo/AlephOne) | Jeu / port | Dernier push : 2019-08-21 | Port GP2X du moteur de jeu Aleph One. |
| [tonsomo/in2x](https://github.com/tonsomo/in2x) | Jeu / émulation | Dernier push : 2019-08-21 | Émulateur Intellivision pour GP2X. |
| [tonsomo/Barrage](https://github.com/tonsomo/Barrage) | Jeu / port | Dernier push : 2019-08-21 | Port GP2X de Barrage. |
| [tonsomo/JurlX2](https://github.com/tonsomo/JurlX2) | Jeu / port | Dernier push : 2019-08-21 | Port GP2X du jeu Jurl. |
| [tonsomo/hex-a-hop](https://github.com/tonsomo/hex-a-hop) | Jeu / port | Dernier push : 2019-08-21 | Port GP2X de Hex-a-hop. |
| [TriForceX/SkyRoads](https://github.com/TriForceX/SkyRoads) | Jeu | Dernier push : 2025-08-14 | Projet/interpréteur SkyRoads pour GP2X basé sur le code source du jeu original. |
| [rixed/egoboo2x](https://github.com/rixed/egoboo2x) | Jeu | Dernier push : 2022-07-11 | Version GP2X du jeu 3D Egoboo ; la description indique qu’elle nécessite `gpu940`. |
| [kaosnyrb/CaveOfDoom](https://github.com/kaosnyrb/CaveOfDoom) | Jeu | Dernier push : 2022-06-17 | Petit jeu SDL écrit pour GP2X. |
| [ttesla/TheSiren](https://github.com/ttesla/TheSiren) | Jeu | Dernier push : 2022-04-13 | Jeu SDL 1.2 dont les sources incluent un port GP2X. |
| [senquack/tileworld-for-handhelds](https://github.com/senquack/tileworld-for-handhelds) | Jeu / port | Dernier push : 2018-04-13 | Port de Tile World destiné aux consoles 320×240, dont GP2X, Wiz et GCW Zero/Dingux. |
| [senquack/Koules-for-Handhelds](https://github.com/senquack/Koules-for-Handhelds) | Jeu / port | Dernier push : 2014-10-05 | Conversion SDL du jeu Koules pour consoles portables, dont GP2X, Wiz et GCW Zero. |
| [joyrider3774/Sokoban](https://github.com/joyrider3774/Sokoban) | Jeu / moteur | Dernier push : 2026-07-17 | Remaniement d’un moteur Sokoban initialement conçu pour GP2X avec SDL 1. Le dépôt actuel mentionne aussi SDL 3 : ne pas confondre la version actuelle avec le port d’origine. |
| [pulkomandy/oldplay](https://github.com/pulkomandy/oldplay) | Multimédia / port | Dernier push : 2016-12-10 | Lecteur de musique de jeux vidéo pour GP2X et autres consoles portables. |
| [lifelike/gplua2x](https://github.com/lifelike/gplua2x) | Langage / bibliothèque | Dernier push : 2010-12-08 | Portage de Lua sur GP2X, avec bibliothèques Open2x, des fonctions SDL et un outil pour lancer des scripts autonomes. |
| [RafaVico/exp_library](https://github.com/RafaVico/exp_library) | Bibliothèque de jeu | Dernier push : 2019-12-16 | Bibliothèque C pour ajouter des succès aux jeux, conçue pour GP2X et PC selon sa description. |

## Projets GitLab

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [a2x](https://gitlab.com/seipy/a2x) | Framework / développement | Dernier commit : 2018-11-24 | Framework C multiplateforme dont la documentation décrit une compilation pour GP2X, ainsi que Wiz, Caanoo et Open Pandora. |
| [fenix-2x84](https://gitlab.com/chenz/fenix-2x84) | Langage / port | Dernier commit : 2018-01-24 | Version de l’interpréteur Fenix compatible avec celle utilisée sur GP2X, selon la description du projet. |
| [gp2x-control-test](https://gitlab.com/david.a.redick/gp2x-control-test) | Test / contrôles | Dernier commit : 2020-07-09 | Test des boutons et du joystick ; le README indique GP2X firmware v2. Projet historique ; la date visible peut correspondre à un import sur GitLab plutôt qu’à une modification du code d’origine. |
| [gp2x-sketch](https://gitlab.com/david.a.redick/gp2x-sketch) | Application / dessin | Dernier commit : 2012-02-11 | Application de dessin de type Etch-a-Sketch, testée sur GP2X firmware v2. Projet historique ; vérifier l’historique d’origine avant d’interpréter la date comme une reprise du développement. |
| [inimaker](https://gitlab.com/alxm/inimaker) | Utilitaire / Wiz — miroir | Dernier commit : 2011-08-28 | Copie GitLab du projet [alxm/inimaker sur GitHub](https://github.com/alxm/inimaker), déjà répertorié plus haut ; ne constitue pas un projet distinct. |

## Projets SourceForge

| Projet | Domaine | Dernière activité visible | Résumé et portée indiquée |
|---|---|---|---|
| [Open2x](https://sourceforge.net/projects/open2x/) | Système / développement | Dernière publication : 2005-10-24 | Ressources Open2x, notamment des sources de noyau et d’U-Boot liés à GP2X. Vérifier les modèles et la documentation avant toute utilisation système. |
| [Wizipedia](https://sourceforge.net/projects/wizipedia/) | Application / Wiz | Dernière publication : Non relevée dans le flux de fichiers consulté | Lecteur Wikipédia hors ligne destiné à GP2X Wiz ; cela ne démontre pas la compatibilité F100/F200. |
| [Simon Tatham's Portable Puzzle Collection for GP2X](https://sourceforge.net/projects/stppc2x/) | Jeu / port | Dernière publication : 2009-02-16 | Port GP2X de la collection de casse-têtes de Simon Tatham. Le même projet est aussi listé sur GitHub sous [ledow/stppc2x](https://github.com/ledow/stppc2x). |
| [libfixgl](https://sourceforge.net/projects/libfixgl/) | Bibliothèque graphique | Dernière publication : Non relevée dans le flux de fichiers consulté | Bibliothèque OpenGL en virgule fixe ciblant notamment GP2X, selon la description SourceForge. |
| [Termula2x](https://sourceforge.net/projects/termula2x/) | Utilitaire / terminal | Dernière publication : 2008-08-03 | Terminal UNIX pour GP2X, avec éditeur vi selon la description du projet. |
| [VICE](https://sourceforge.net/projects/vice-emu/) | Émulation | Dernière publication : 2026-10-21 | Émulateur de systèmes Commodore dont la page mentionne GP2X parmi les plateformes. Cette mention ne prouve pas qu’une version récente fonctionne sur la console. |
| [Wizznic!](https://sourceforge.net/projects/wizznic/) | Jeu / Wiz | Dernière publication : 2020-04-04 | Jeu de puzzle de type Puzznic présenté pour Wiz, Linux et Windows ; la page consultée ne revendique pas de compatibilité F100/F200. |

## Sources et recherches pour prolonger l’inventaire

- [Recherche GitHub des dépôts « GP2X »](https://github.com/search?q=GP2X&type=repositories) et [sujet `gp2x`](https://github.com/topics/gp2x) — résultats et projets balisés, non exhaustifs.
- [Sujet GitHub `gp2x-wiz`](https://github.com/topics/gp2x-wiz) — projets étiquetés spécifiquement Wiz.
- [Recherche de projets GitLab « gp2x »](https://gitlab.com/explore/projects?name=gp2x) — résultats susceptibles de varier avec le temps.
- [Répertoire SourceForge recherchant « gp2x »](https://sourceforge.net/directory/?q=gp2x) — résultats susceptibles de contenir des doublons ou des projets mentionnant seulement la plateforme.
- [Historique des issues GitHub](https://docs.github.com/en/issues/tracking-your-work-with-issues/about-issues) — peut aider à retrouver des informations de compatibilité laissées par les utilisateurs.

Les anciens catalogues et archives consacrés à GP2X peuvent fournir des pistes supplémentaires, mais leur accessibilité et l’intégrité des fichiers n’ont pas été vérifiées dans le cadre de cette mise à jour.

## Mise à jour de l’inventaire

Pour ajouter une entrée, vérifier que le dépôt est bien public et qu’il existe toujours, lire son README et identifier précisément le modèle GP2X concerné. Indiquer explicitement s’il s’agit d’un port, d’un outil PC, d’une documentation, d’un projet Wiz/Caanoo ou d’un projet propre à F100/F200. Ne pas présenter un dépôt comme maintenu ou fonctionnel sans preuve récente.
