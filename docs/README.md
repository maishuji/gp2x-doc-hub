# Documentation GP2X

Ce dossier rassemble des guides techniques et des inventaires consacrés aux consoles GP2X et à leur écosystème historique.

## Matériel et accessoires

- [Diagnostic et réparation des consoles GP2X](reparation-gp2x.md) : pannes connues, contrôles par symptôme, précautions, pièces recensées et récupération logicielle experte.
- [GP2X Cradle et breakout boards — guide pratique](gp2x-cradle-et-breakout-board.md) : comprendre les accessoires, choisir une solution, raccorder des périphériques USB, une télévision ou une console série, et diagnostiquer les problèmes courants.
- [Port EXT de la GP2X — référence technique](gp2x-port-ext-reference.md) : signaux disponibles, brochage historique, niveaux électriques et précautions pour fabriquer ou contrôler un adaptateur.
- [GP2X F200 — diagnostic matériel et alimentation](GP2X_F200_hardware_diagnostic.md) : notes de réparation et mesures sur une F200 particulière.

## Logiciels et ressources historiques

- [Environnement Docker et dev container pour GP2X](environnement-developpement-gp2x.md) : image devkitPro, configuration minimale et limites par rapport aux anciens SDK Linux/Open2X.
- [Firmwares des consoles GamePark et GamePark Holdings](firmwares-gp2x.md)
- [Sources techniques sur Open2X et la GP2X F100](sources-techniques-migration-f100.md) : inventaire annoté du SVN Open2X et contraintes noyau 2.4/Buildroot.
- [Inventaire des projets et ressources liés à la GP2X](projets-github-gp2x.md)
- [Répertoire de sites web consacrés à la GP2X](sites-web-gp2x.md)

## Parcours conseillé

Pour utiliser une cradle existante, commencer par le [guide pratique](gp2x-cradle-et-breakout-board.md), en particulier les sections sur l’identification et l’alimentation. Consulter la [référence du port EXT](gp2x-port-ext-reference.md) uniquement pour contrôler un câble, fabriquer une interface ou travailler avec l’UART et le JTAG.

Les sources GP2X sont anciennes et parfois contradictoires. Chaque document distingue autant que possible les faits corroborés, les observations communautaires et les points qui doivent encore être vérifiés sur le matériel concerné.
