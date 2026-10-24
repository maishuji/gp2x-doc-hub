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
