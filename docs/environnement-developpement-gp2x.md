# Environnement Docker et dev container pour GP2X

Une image Docker maintenue existe pour développer avec la chaîne **devkitPro** : [`devkitpro/devkitarm`](https://hub.docker.com/r/devkitpro/devkitarm). Sa description mentionne explicitement la GP2X et son [Dockerfile public](https://github.com/devkitPro/docker/blob/master/devkitarm/Dockerfile) installe le paquet `gp2x-dev`.

## Ce que cette image couvre

L'image fournit la chaîne ARM de devkitPro ainsi que les environnements GBA, NDS, 3DS, GP32 et GP2X. Pour la GP2X, les projets publics associés comprennent notamment :

- [`devkitPro/gp2x-core`](https://github.com/devkitPro/gp2x-core) : fichiers de démarrage, scripts d'édition de liens et règles de construction ;
- [`devkitPro/gp2x-tools`](https://github.com/devkitPro/gp2x-tools) : outils comme `mko2x` ;
- [`devkitPro/gp2x-examples`](https://github.com/devkitPro/gp2x-examples) : exemples framebuffer, audio, DMA, second CPU et OpenOCD ;
- [`devkitPro/orcus`](https://github.com/devkitPro/orcus) : bibliothèque bare-metal.

Cette chaîne est particulièrement adaptée aux exemples devkitPro et au développement **bare-metal**. Elle ne doit pas être confondue avec les anciens SDK Linux/Open2X servant à produire des applications liées aux bibliothèques du firmware officiel, par exemple SDL 1.2. Un projet historique qui attend un compilateur `arm-open2x-linux-*`, un sysroot GPH/Open2X ou des bibliothèques dynamiques de la console peut nécessiter une seconde image dédiée.

L'image ne constitue pas non plus un SDK commun pour la Wiz ou la Caanoo. Ces consoles doivent être traitées séparément tant que leurs chaînes et sysroots ne sont pas ajoutés et testés.

## Image recommandée

Pour un environnement reproductible, utiliser un tag daté plutôt que `latest` :

```text
devkitpro/devkitarm:20260610
```

Le manifeste de ce tag a été vérifié le 24 octobre 2026. Il publie au moins des variantes Linux `amd64` et `arm64`. L'image compressée représente environ 520 à 537 Mo selon l'architecture.

`latest` reste pratique pour tester les mises à jour, mais il est mutable. Pour une reproductibilité stricte, il est aussi possible de fixer le manifeste :

```text
devkitpro/devkitarm@sha256:116afba8df8453961de2936ffab20dd441edf4d682856c1ec8b0e53d7ed0bbf5
```

Un digest rend toutefois les mises à jour de sécurité manuelles. Le tag daté est généralement un meilleur compromis pour un dépôt de développement.

## Dev container minimal

Créer `.devcontainer/devcontainer.json` dans le projet GP2X :

```json
{
  "name": "GP2X devkitPro",
  "image": "devkitpro/devkitarm:20260610",
  "remoteUser": "root",
  "containerEnv": {
    "DEVKITPRO": "/opt/devkitpro",
    "DEVKITARM": "/opt/devkitpro/devkitARM"
  },
  "customizations": {
    "vscode": {
      "extensions": [
        "ms-vscode.cpptools",
        "ms-vscode.cmake-tools"
      ]
    }
  }
}
```

Les variables sont déjà définies dans l'image ; les répéter dans le dev container rend le contrat de l'environnement visible. Le bloc `customizations.vscode` est facultatif et peut être retiré avec un éditeur qui n'utilise pas ces extensions.

Pour garantir que les outils nécessaires sont présents lors de la création du conteneur :

```json
{
  "name": "GP2X devkitPro",
  "image": "devkitpro/devkitarm:20260610",
  "remoteUser": "root",
  "containerEnv": {
    "DEVKITPRO": "/opt/devkitpro",
    "DEVKITARM": "/opt/devkitpro/devkitARM"
  },
  "postCreateCommand": "test -x /opt/devkitpro/devkitARM/bin/arm-none-eabi-gcc && command -v mko2x && make --version"
}
```

`postCreateCommand` ne compile pas encore le projet : il échoue immédiatement si le compilateur, `mko2x` ou `make` manque.

## Dockerfile dérivé

Pour ajouter des outils sans modifier l'image publiée, utiliser `.devcontainer/Dockerfile` :

```dockerfile
FROM devkitpro/devkitarm:20260610

RUN apt-get update \
    && apt-get install -y --no-install-recommends gdb-multiarch \
    && rm -rf /var/lib/apt/lists/*
```

Puis référencer ce fichier :

```json
{
  "name": "GP2X devkitPro",
  "build": {
    "dockerfile": "Dockerfile"
  },
  "remoteUser": "root"
}
```

N'ajouter OpenOCD ou l'accès USB/JTAG qu'en fonction du matériel réellement utilisé. Donner au conteneur `--privileged` est trop large pour une simple compilation ; préférer un passage explicite du périphérique et des règles adaptées.

## Vérification avec les exemples officiels

Après ouverture du conteneur :

```sh
git clone https://github.com/devkitPro/gp2x-examples.git
cd gp2x-examples
make
```

Cette commande est une piste de validation fondée sur le Makefile du dépôt. Elle doit encore être exécutée dans l'image avant de considérer l'ensemble des exemples comme compatible avec le tag choisi.

Pour un projet particulier, vérifier son Makefile :

- `arm-none-eabi-*`, `$(DEVKITARM)` ou inclusion des règles devkitPro : cette image est probablement adaptée ;
- `arm-open2x-linux-*`, ancien sysroot GPH/Open2X ou bibliothèques SDL du firmware : cette image seule est probablement insuffisante ;
- chaîne Wiz/Caanoo : compatibilité non établie.

## Émuler la GP2X sur un ordinateur

Oui, c'est encore possible — et il existe maintenant des projets plus proches de l'émulation de la console entière que les anciens émulateurs de jeux. Ils ne sont pas des images Docker : on les compile ou on les exécute sur l'ordinateur. Le dev container peut fournir un environnement Linux reproductible pour compiler les sources, mais il ne fournit ni firmware ni jeux.

### GPort2X : démarrer firmware et carte SD

[GPort2X](https://github.com/Jacob-Matthew-Cook/GPort2X) est un émulateur de l'environnement GP2X pour Linux moderne. Selon son README et son [état technique daté du 8 octobre 2026](https://github.com/Jacob-Matthew-Cook/GPort2X/blob/main/docs/STATUS.md), il sait démarrer le firmware de référence et lancer des programmes depuis une image de carte, avec un moteur interpréteur/JIT pour hôtes x86-64/AArch64 ; un moteur natif armhf est aussi décrit pour les noyaux AArch64 compatibles 32 bits. Le projet documente des tests de jeux, mais ce niveau de compatibilité est celui rapporté par son auteur, pas une validation indépendante de chaque titre.

Le README fournit ce chemin de compilation Linux :

```sh
git clone https://github.com/Jacob-Matthew-Cook/GPort2X.git
cd GPort2X
make
make test
```

SDL2 est facultatif pour compiler ; sans SDL2, le backend reste sans fenêtre. Pour la fenêtre SDL2, le README indique que les fichiers de développement SDL2 doivent être installés sur l'hôte de compilation. Dans un dev container, afficher la fenêtre exige aussi une configuration d'affichage de l'hôte (Wayland/X11) et son transfert au conteneur : construire dans le conteneur ne rend pas automatiquement la fenêtre visible.

Chaque exécution avec firmware et carte requiert des fichiers **fournis par l'utilisateur** : dump du firmware et image de carte SD. GPort2X documente l'image de carte en lecture seule ; les données de jeu ne sont pas incluses. Respecter les droits applicables et ne pas partager ces dumps. Voir le [README GPort2X](https://github.com/Jacob-Matthew-Cook/GPort2X) pour les options exactes et les limites actuelles.

### magiceyes : lancer des jeux GP2X/Wiz/Caanoo sur PC

[magiceyes](https://github.com/zdiemer/magiceyes) annonce l'émulation de titres GP2X F100/F200, Wiz et Caanoo. La [version 0.6.0](https://github.com/zdiemer/magiceyes/releases) fournit un paquet Windows prêt à lancer ; son README documente également un build Linux depuis les sources. Ce projet vise l'exécution de jeux et la réimplémentation de leurs interfaces matérielles, plutôt qu'une distribution complète avec démarrage générique du firmware. Il n'inclut pas les jeux ni les données de firmware ; son tableau de compatibilité est maintenu par le projet et évolue.

C'est une piste pour tester rapidement un `.gpe` sur Windows, ou construire/lancer l'émulateur sur Linux. Pour une émulation qui démarre le firmware GP2X complet et une image de carte, GPort2X est la piste plus directement adaptée d'après les fonctions documentées. Les résultats de compatibilité publiés par chaque projet ne sont pas des validations indépendantes.

### Projets historiques ou prototypes à ne pas confondre

- [Ginge](https://github.com/notaz/ginge) est une couche de compatibilité pour exécuter certains programmes GP2X sur d'autres appareils ARM Linux ; son README précise que ce n'est **pas** un émulateur complet et qu'il ne cible pas directement un PC Linux x86.
- [dolos2x](https://github.com/Orkie/dolos2x) est un prototype basé sur Unicorn qui annonce pouvoir lancer U-Boot Open2X et quelques homebrews ; son README garde le démarrage Linux parmi les tâches à faire. Ce n'est donc pas encore le choix pratique pour lancer un firmware complet.
- Les anciens ports d'émulateurs (MAME, PicoDrive, etc.) émulent les systèmes qu'ils exécutent sur GP2X ; ils ne sont pas nécessairement des émulateurs de la console GP2X elle-même.

### Conteneur conseillé selon le besoin

- **Compiler des applications bare-metal devkitPro** : utiliser `devkitpro/devkitarm` comme décrit plus haut.
- **Compiler ou tester l'émulateur GPort2X sur Linux** : partir d'un conteneur Linux de développement avec GCC, GNU Make et, si la fenêtre est souhaitée, SDL2 et la configuration d'affichage transférée. Aucun conteneur spécialisé maintenu par le projet GPort2X ou magiceyes n'a été identifié pendant cette recherche.
- **Développer une application Linux/Open2X** : le conteneur devkitPro seul ne fournit pas le sysroot historique `arm-open2x-linux-*`. L'émulateur est une aide de test, pas un SDK ni un remplacement de la toolchain.

## Autres images trouvées

La recherche Docker Hub renvoie également :

- `abandonedcart/devkitarm` : copie tierce très peu utilisée ; aucun avantage établi sur l'image devkitPro ;
- `hkubedevtest/gp2xb35w` : image sans description exploitable et très peu téléchargée.

Aucune image trouvée sous le terme `open2x` n'offrait une base publique identifiable. L'image officielle de l'organisation devkitPro est donc le meilleur point de départ actuellement recensé, à condition que le projet vise bien sa chaîne GP2X bare-metal.

## Sources

- [Image Docker Hub `devkitpro/devkitarm`](https://hub.docker.com/r/devkitpro/devkitarm)
- [Dockerfiles officiels devkitPro](https://github.com/devkitPro/docker)
- [Dockerfile `devkitarm`](https://github.com/devkitPro/docker/blob/master/devkitarm/Dockerfile)
- [Composants GP2X de devkitPro](https://github.com/devkitPro/gp2x-core)
- [Outils GP2X de devkitPro](https://github.com/devkitPro/gp2x-tools)
- [Exemples GP2X de devkitPro](https://github.com/devkitPro/gp2x-examples)

## Journal des vérifications

- **24 octobre 2026** : image `devkitpro/devkitarm` trouvée ; Dockerfile confirmé avec `gp2x-dev` ; manifeste du tag `20260610` vérifié pour Linux `amd64` et `arm64` ; distinction documentée entre devkitPro bare-metal et anciens SDK Linux/Open2X.
- **26 octobre 2026** : ajout de GPort2X, magiceyes, Ginge et dolos2x à l'inventaire des options d'émulation ; informations recoupées avec les README et, pour GPort2X, son état technique daté du 8 octobre 2026. Aucun build ni lancement de ces émulateurs n'a été réalisé localement.
