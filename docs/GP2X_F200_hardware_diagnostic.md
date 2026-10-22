# GP2X F200 — Notes de diagnostic matériel et cartographie de l'alimentation

> **Journal de réparation / base de connaissances communautaire**  
> État au **23 octobre 2026** · Langue : français · Statut : **diagnostic en cours**  
> Cible : **GamePark Holdings GP2X F200**, carte observée sur les photographies de cette réparation.

## À lire avant toute réutilisation

Ce document rassemble les observations effectuées sur **un exemplaire particulier** de GP2X F200. Il ne constitue **ni un schéma officiel**, ni une preuve que toutes les révisions de PCB sont identiques. Il distingue :

- **[MESURÉ]** : lecture au multimètre ou résultat de continuité rapporté pendant la réparation ;
- **[CONFIRMÉ]** : identification de composant vérifiée dans une fiche technique, ou connexion validée par une mesure électrique ;
- **[DÉDUIT]** : calcul ou interprétation plausible des mesures ;
- **[À VÉRIFIER]** : résultat non confirmé, ancien repérage douteux ou hypothèse ;
- **[ÉCARTÉ PROVISOIREMENT]** : cause devenue moins probable, sans garantie d'exclusion absolue.

**Ne pas traiter une hypothèse comme un diagnostic définitif.** Les tensions ont été relevées à plusieurs moments, avec différentes paires de piles et parfois des problèmes de contact ; elles ne sont donc pas nécessairement toutes simultanées.

---

## 1. Problème initial, réparation et symptôme restant

### État initial

- La GP2X F200 présentait des démarrages intermittents avec écran noir.
- Une pression mécanique, la fermeture de la coque ou la manipulation du connecteur secteur pouvaient affecter le comportement.
- Le connecteur DC d'origine était instable ; l'interrupteur `SW1` avait également des problèmes de contact.

### Interventions déjà réalisées

1. **Remplacement du connecteur d'alimentation DC** par un connecteur de type `DC-011C`, version CMS/coudée, annoncé pour fiche d'environ `2,5 × 0,7 mm`. La compatibilité mécanique et le montage sont ceux effectivement réalisés sur cet exemplaire ; ne pas généraliser sans vérifier la fiche et la polarité de l'adaptateur.
2. **Réparation/reprise de l'interrupteur coulissant `SW1`**.
3. Contrôle de la commutation du jack entre alimentation externe et piles.

### État après intervention

| Mode | Voyant bleu | Écran / démarrage | Observation |
|---|---|---|---|
| Secteur | Allumé, plus lumineux | **Démarre correctement** | Comportement désormais fiable après réparation du jack et de SW1 |
| 2 piles AA | Allumé, moins lumineux | **Écran noir / pas de démarrage** | Défaut restant |

**Objectif** : localiser la cause de l'absence de démarrage **uniquement sur piles** sans remplacer des composants au hasard.

---

## 2. Matériel et précautions de mesure

- Multimètre utilisé : **Klein Tools MM400**.
- Cordons : noir sur **COM**, rouge sur **VΩ** ; **ne pas utiliser les prises de courant** pour les tests décrits ici.
- Tensions : mode **V⎓ (DC)**, référence noire sur un point **GND** fiable.
- Continuité/résistance/capacité : **piles retirées et secteur débranché**. Vérifier l'absence de tension et laisser les condensateurs se décharger avant une mesure de capacité.
- Les mesures **Hz sur les nœuds de commutation** (`LX`, `SW`) au MM400 ne sont **pas concluantes** : le signal rapide/PWM peut ne pas être reconnu.
- Une ancienne série de mesures avait été réalisée par erreur en **V~ (AC)**, d'où des lectures incohérentes (dont ~6,13 V). **Ne pas réutiliser ces valeurs**.
- Une mesure de continuité a été effectuée accidentellement sur une pile NiMH, qui a chauffé. Ne pas refaire ce test ; isoler/remplacer toute pile qui chauffe anormalement, fuit ou paraît endommagée.
- Attention aux ponts entre broches fines lors des mesures sous tension.

**Conventions** : `MΩ` = mégaohms, `kΩ` = kilo-ohms, `Ω` = ohms. Exemple : **2,0 MΩ n'est pas 2 Ω**.

---

## 3. Vue d'ensemble de la chaîne d'alimentation (partiellement vérifiée)

```text
                     PILES AA : 2,546 V mesurés
                               │
                     [chemin / protection]
                               │
                 rail vers L35 : ~2,245 V
                               │
                              L35
                               │
                               ▼
                    U34 / « HBCA »
                   ACT6305UC-T (boost)
                     EN ~2,323 V
                               │
                      OUT = FB ~2,000 V
                               │
                     ┌─────────┴──────────┐
                     │                    │
                    C233                 U35 IN
               ~100 µF nominal          ~1,99 V
                     │                    │
                    GND           U35 / « IAFM »
                                 ACT6906UCADJ-T
                                     buck
                                      │
                                  rail ~2 V
                                      │
                 [lien à U27 SHDN : À CARTOGRAPHIER]
                                      │
                                 U27 SHDN
                              0 V sur piles
                                      │
                                U27 désactivé
```

### Connexions réellement constatées

- **[CONFIRMÉ par ohmmètre]** `U34 OUT ↔ U34 FB` : environ **0,1 Ω**.
- **[CONFIRMÉ par ohmmètre]** `U34 OUT ↔ U35 IN` : environ **0,1 Ω**.
- **[CONFIRMÉ par ohmmètre]** Une broche du côté à deux pattes de `U34` est reliée à une extrémité de **L35**, à environ **0,1 Ω** ; l'autre extrémité de L35 était à **0,6 Ω** de cette broche.
- **[CONFIRMÉ par continuité]** Plusieurs pads/vias/une borne de condensateur près de `U35` appartiennent au même net que `U35 IN`.
- **[MESURÉ]** `U27 SHDN` et certains points sur la face opposée ont exactement la même réponse en tension (0 V sur piles ; ~2,014 V sur secteur ON), et une connexion avec un via voisin de U27 a été testée.
- **[À VÉRIFIER]** Le raccordement exact entre la sortie de `U35`, la bobine `L30`, le rail à ~2 V et `U27 SHDN`. **Ne pas affirmer que SHDN est directement relié à la broche SW de U35** sans nouvelle mesure de continuité et identification physique des broches.

Ce dessin est **fonctionnel** : il ne prétend pas reproduire toutes les pistes ni l'intégralité du séquencement de démarrage.

---

## 4. Tableaux des mesures

### 4.1 Batteries et rails généraux

| Point / contexte | Piles ON | Secteur ON | Remarques |
|---|---:|---:|---|
| Deux AA en série, mesure récente | **2,546 V** | — | Valeur du pack à ce stade ; préciser la charge utilisée lors d'une reproduction |
| AA alcalines observées plus tôt | ~1,32–1,33 V chacune | — | Pack ~2,66 V, relativement peu de chute rapportée sous charge |
| Trois points proches de SW1 (cerclés sur photo) | **2,273 V** | non relevé | Même tension **ne prouve pas** une liaison série avec U35 |
| `C253`, borne active | **1,813 à 1,860 V** | **4,960 V** | L'autre borne est GND ; rail à identifier précisément |
| Condensateur marqué `22 16V`, borne active | **2,370 V** | **3,330 V** | L'autre borne est GND ; il s'agit d'un condensateur, **pas de L38** |

### 4.2 Jack DC remplacé

| Vérification | Résultat | Interprétation |
|---|---|---|
| A–B sans fiche | BIP | Contact fermé dans cet état |
| C–E sans fiche | BIP | Liaison piles fermée dans cet état |
| C–E fiche introduite | Pas de BIP | Contact commuté par la fiche |
| Chute de tension C–E, piles ON | **~2,5 mV** | Très faible chute sur le contact de commutation mesuré |

**[ÉCARTÉ PROVISOIREMENT]** Le contact commuté du nouveau jack n'est plus le premier suspect.

### 4.3 U34 — `HBCA` / ACT6305UC-T

**Identification** : marquage supérieur `HBCA`; référence sérigraphiée correcte sur **cette carte : U34**. Une désignation antérieure **« U37 » était erronée**.

| Broche fonctionnelle | Piles ON | Secteur ON | État |
|---|---:|---:|---|
| `OUT`, pin 5 | **2,000 V** (puis ~2,03 V au démarrage sur C233) | **2,791 V** | Mesuré |
| `FB`, pin 1 | **2,000 V** | **2,791 V** | Mesuré ; relié à OUT à 0,1 Ω |
| `EN`, pin 3 | **2,323 V** | **3,465 V** | Mesuré ; actif haut |
| `GND`, pin 2 | **0 V** | 0 V attendu | Masse identifiée |
| `LX`, pin 4 | **2,334 V** lors d'une série | non noté séparément | Nœud commuté : lecture DC moyenne peu interprétable |
| Autre lecture antérieure côté 2 broches | 2,284 V | — | Valeur **antérieure contradictoire** avec les 2,000 V simultanés sur OUT/FB ; garder comme historique, ne pas fusionner |

**Correspondance physique mesurée** (orientation utilisée lors des relevés : côté à 2 pattes et côté à 3 pattes ; toujours vérifier visuellement l'orientation avant toute soudure) :

```text
       côté à 2 broches
         OUT      LX
        pin 5    pin 4
          o        o
       ┌─────────────┐
       │ U34 / HBCA  │
       └─────────────┘
          o    o    o
         FB   GND   EN
        pin 1 pin 2 pin 3
       côté à 3 broches
```

**Résistances/continuité, hors tension** :

| Entre points | Résultat |
|---|---:|
| U34 OUT ↔ U34 FB | **0,1 Ω** |
| U34 OUT ↔ broche 3-pattes extérieure opposée (EN) | **1,84 MΩ** |
| U34 OUT ↔ GND | **~2,0 MΩ** (unité explicitement confirmée) |
| U34 LX ↔ L35 extrémité la plus proche | **0,1 Ω** |
| U34 LX ↔ autre extrémité de L35 | **0,6 Ω** |
| U34 OUT ↔ U35 IN | **0,1 Ω** |

**Référence constructeur** : ACT6305UC-T, boîtier **SOT-23-5**, `FB=OUT` sélectionne une **consigne fixe de 3,3 V** ; `EN` est actif à l'état haut (seuil haut indiqué **0,6 V** dans la fiche). Voir [fiche ACT6305](https://www.semiconductors-ic.com/files/d3/act6305uc-t.pdf).

### 4.4 L35 et sortie associée

| Mesure | Piles ON | Secteur ON |
|---|---:|---:|
| L35 côté gauche tel que photographié | **2,188 V** | **3,319 V** |
| L35 côté droit tel que photographié | **2,245 V** | **3,460 V** |
| Différence absolue des lectures DC | **57 mV** | **141 mV** |

**Mise en garde** : le côté relié à `LX` est un **nœud de commutation**, donc ces différences en V⎓ ne sont pas une mesure fiable de la chute ohmique réelle de la bobine ni de son courant. Le lien `LX–L35` a été établi en **ohms hors tension**, ce qui est plus solide.

### 4.5 C233 — condensateur raccordé à U34 OUT / U35 IN

- **[IDENTIFIÉ sur carte]** `C233`, gros condensateur CMS jaune/orange portant notamment le code **`107 A`**.
- **[MESURÉ]** Capacité **129 µF** au Klein MM400, directement **en circuit**.
- **[DÉDUIT]** `107` est compatible avec **100 µF nominal** pour ce type de condensateur ; la valeur mesurée en circuit **ne certifie ni la capacité isolée, ni l'ESR**.
- **[MESURÉ]** Sur piles, la borne OUT de C233 atteint **~2,03 V immédiatement** après passage ON ; aucune excursion visible vers 3,3 V au multimètre.
- **[À VÉRIFIER]** Tension nominale, boîtier, polarité exacte et série du condensateur **sur la base de sa référence constructeur/mesures dimensionnelles**. L'interprétation du caractère `A` seul n'est **pas suffisante** pour commander sans risque.

### 4.6 U35 — `IAFM` / ACT6906UCADJ-T

**Identification par marquage** : `IAFM` correspond à `ACT6906UCADJ-T`, régulateur **buck** synchrone, boîtier SOT-23-5. Fiche : [ACT6906](https://www.ic-components.kr/files/a2/ACT6906UCADJ-T.pdf).

| Point mesuré sur la photo (orientation de l'utilisateur) | Piles ON | Secteur ON |
|---|---:|---:|
| côté gauche, haut | **1,940 V** | **2,700 V** |
| côté gauche, bas | **~0,0003 V** | **0,600 V** |
| côté droit, haut | **0 V** | **2,016 V** |
| côté droit, milieu | **0 V** | **0 V** |
| côté droit, bas | **1,993 V** | **2,700 V** |

**Identification fonctionnelle par vérifications croisées** :

- La broche à **~0,600 V sur secteur** se comporte comme **FB** (référence d'asservissement de l'ACT6906 ajustable = **0,6 V**).
- La broche toujours **0 V** est compatible avec **GND**.
- La broche à **~1,993 V piles / 2,70 V secteur**, reliée à **U34 OUT à 0,1 Ω**, est **U35 IN**.
- Les autres correspondances visuelles doivent être reprises à l'aide de la marque d'orientation du boîtier et de la fiche : **ne pas propager un pinout physique spéculatif**.

**Fiche constructeur, fonctions logiques** : pin 1 `EN`, pin 2 `G`, pin 3 `SW`, pin 4 `IN`, pin 5 `FB`.

**Pont de retour U35** : résistances marquées `244` et `104` ; une borne commune à FB. Sur secteur ON :

- côté non commun de `244` : **~2,006 V** ; OFF : 0 V ;
- côté non commun de `104` : **0 V** ;
- FB : **0,600 V**.

Cela est compatible avec `244 ≈ 240 kΩ` comme résistance haute, `104 ≈ 100 kΩ` comme résistance basse, pour régler la sortie vers ~2 V. À **~1,99 V sur U35 IN en mode piles**, son fonctionnement n'est pas démontré ; le rail régulé est absent et le défaut peut être **une conséquence de l'alimentation U34 insuffisante**.

### 4.7 U27 — probablement LTC3426, convertisseur boost 6 broches

L'identification `U27 = LTC3426` découle d'une correspondance documentaire GP2X F200 et d'une signature de broches/tensions très convaincante. **Vérifier le marquage sur la puce avant toute commande**. Fiche officielle : [Analog Devices LTC3426](https://www.analog.com/media/en/technical-documentation/data-sheets/3426fb.pdf).

| Fonction / pin LTC3426 | Piles ON | Secteur ON | Observation |
|---|---:|---:|---|
| `SW`, pin 1 | **2,567 V** | **3,435 V** | Valeur DMM moyenne d'un nœud de commutation |
| `GND`, pin 2 | **~0 V** | **~0 V** | Masse |
| `FB`, pin 3 | **0,860 V** | **1,224 V** | ~1,22 V nominal quand régulation active |
| `SHDN`, pin 4 | **~0 V** | **2,010 V** | **Désactivé sur piles, activé sur secteur** |
| `VOUT`, pin 5 | **2,356 V** | **3,330 V** | Sortie non régulée sur piles |
| `VIN`, pin 6 | **2,576 V** | **3,463 V** | Entrée présente dans les deux modes |

Les côtés physiques (sur la photo utilisée) étaient : gauche haut `SHDN`, gauche milieu `VOUT`, gauche bas `VIN` ; droite haut `FB`, droite milieu `GND`, droite bas `SW`. Cette **orientation s'applique uniquement à la photo de travail**.

**Contrôle de commande U27** :

| État | `SHDN` U27 |
|---|---:|
| Piles OFF | 0 V |
| Piles ON | **~0,0002 V** |
| Secteur OFF | 0 V |
| Secteur ON | **~2,009 V** |

Le LTC3426 a une entrée SHDN active haut. Le constat que **SHDN ne monte pas sur piles** est solide ; cela explique pourquoi U27 ne régule pas dans ce mode. Le défaut est potentiellement **en amont de sa commande**, non dans U27 lui-même.

**Autres mesures hors tension** (in-circuit, donc non interprétables comme résistances isolées) :

| État interrupteur | SHDN ↔ VIN | SHDN ↔ GND |
|---|---:|---:|
| OFF | ~1,088 kΩ | ~1,066 kΩ |
| ON | ~1,084 kΩ | ~1,062 kΩ |

La variation est faible. Il n'a pas été démontré de connexion de cuivre directe `SW1 ↔ SHDN`.

### 4.8 Autres circuits, non prioritaires

D'autres circuits d'alimentation initialement appelés `U37` et `U38` ont fait l'objet de mesures exploratoires. Leurs marquages (`HBCA`/`YCXB` dans les premières notes) et **leurs numéros de carte ont été confondus** au cours de l'échange. Pour éviter une nomenclature trompeuse :

- L'**ACT6305 marqué HBCA** étudié en détail ici est **U34**, proche de **L35** et **C233**.
- Ne pas déduire une seconde identité `U37 = HBCA` sans photographie et sérigraphie séparées.
- Les rails secondaires `U37/U38` éventuels restent **à réidentifier** ; pas de conclusion de panne.

---

## 5. Calculs et interprétation

### 5.1 Différences de tension sous piles

**Valeurs prises à différentes étapes :**

```text
Vpack      = 2,546 V
Vpoints    = 2,273 V
VL35 droit = 2,245 V
VU34 OUT   = 2,000 V
VU35 IN    ≈ 1,993 V
```

Différences numériques :

```text
2,546 − 2,273 = 0,273 V
2,546 − 2,245 = 0,301 V
2,245 − 2,000 = 0,245 V
2,000 − 1,993 = 0,007 V
```

- **0,273 V** est la différence pack → points à 2,273 V, **pas une preuve qu'il existe un composant série précis**.
- **0,245 V** est la différence entre l'extrémité de L35 mesurée à ~2,245 V et `U34 OUT ~2,000 V`. Cela **ressemble** à une alimentation résiduelle via le convertisseur non régulant, **mais ne prouve pas** une diode ni une panne interne précise.
- **7 mV** entre U34 OUT et U35 IN est cohérent avec leur continuité à **0,1 Ω** (mesures non strictement simultanées).

### 5.2 Retour de tension U35 : sortie calculée

Hypothèse du diviseur de tension (marquages EIA standards) :

- `244` → **240 kΩ** (branche haute) ;
- `104` → **100 kΩ** (branche basse) ;
- `VFB` nominal ACT6906ADJ = **0,600 V**.

Formule :

\[
V_{out} = V_{FB}\left(1 + \frac{R_{haut}}{R_{bas}}\right)
\]

\[
V_{out} = 0{,}600\left(1 + \frac{240}{100}\right)
= 0{,}600 \times 3{,}4
= \boxed{2{,}040\ \mathrm{V}}
\]

Mesure du côté haut du diviseur sur secteur : **~2,006 V**, écart de **34 mV** (~**1,7 %**), compatible avec la tolérance des composants et les relevés. U35 semble donc **réguler normalement le rail ~2 V sur secteur**.

### 5.3 Consigne et erreur U34

D'après la fiche ACT6305 : `FB` directement relié à `OUT` → sortie nominale **3,3 V** (plage indiquée 3,2–3,4 V dans les conditions du constructeur).

| Mode | Consigne théorique | Sortie mesurée | Écart |
|---|---:|---:|---:|
| Piles ON | 3,300 V | **2,000 V** | **−1,300 V** (~−39,4 %) |
| Secteur ON | 3,300 V | **2,791 V** | **−0,509 V** (~−15,4 %) |

**Important** : la GP2X **démarre sur secteur malgré les 2,791 V** sur U34 OUT. Cette anomalie dans les deux modes empêche d'affirmer que « U34 seul est définitivement HS ». Possibilités : charge particulière en circuit, chemins alternatifs, comportement de démarrage, mesure prise en des conditions différentes, défaut local du circuit, ou identification à recontrôler. Il faut comparer les conditions de mesure et, idéalement, observer `LX` à l'oscilloscope sous protection appropriée.

### 5.4 Bobine L35 : résistance approximative

Mesures hors tension depuis la broche `LX` :

```text
vers borne L35 A : 0,1 Ω
vers borne L35 B : 0,6 Ω
écart           : 0,5 Ω
```

On peut **estimer grossièrement** une résistance de l'enroulement + contacts de l'ordre de **0,5 Ω**. À ces niveaux, la résistance des pointes et des contacts du MM400 est significative : **ce n'est pas une mesure quatre fils de DCR**. La bobine n'est pas ouverte ; son inductance et son comportement sous charge ne sont pas vérifiés.

### 5.5 Capacité de C233

`107` est compatible avec **100 µF** ; mesure en circuit **129 µF**, soit une différence arithmétique de **+29 %** par rapport à 100 µF. On ne peut pas interpréter cette différence comme la tolérance réelle du composant, à cause du **montage en circuit**. **ESR non mesurée**.

---

## 6. Hypothèses de panne, classées par niveau de preuve

### A. Manque de commande U27 sur piles — **fait établi**

- `U27 VIN` reçoit ~2,576 V sur piles à une étape du diagnostic.
- `U27 SHDN` reste à ~0 V sur piles, mais monte à ~2,009 V sur secteur.
- Le boost principal ne régule pas sur piles : conséquence compatible avec SHDN à zéro.

**À expliquer** : quel rail, composant ou circuit doit créer l'état haut de `U27 SHDN` en mode batteries ?

### B. U34/HBCA ne fournit pas sa tension cible — **fait établi, cause inconnue**

- EN est haut, FB court-circuité intentionnellement à OUT.
- OUT ne monte qu'à ~2,0 V piles / ~2,79 V secteur au lieu des 3,3 V attendus selon la fiche.
- OUT est directement connecté à l'entrée de U35.

**Causes possibles non départagées** : U34 défectueux ; charge dynamique excessive ; problème de condensateur de sortie/ESR ; défaut intermittent ; mode de fonctionnement ou séquencement spécifique ; mesure à reproduire simultanément sur une paire AA saine.

### C. C233 détérioré — **possible, non établi**

- Capacité en circuit ~129 µF : ne démontre pas une panne.
- ESR / comportement HF non mesuré.
- **Éviter d'acheter/dessouder uniquement sur la base de cette mesure.**

### D. L35 détériorée — **moins probable, non exclue**

- Conductrice entre ses deux extrémités.
- Aucune mesure d'inductance ou de saturation sous charge.

### E. Perte importante au jack DC — **moins probable après remplacement**

- Commutation validée ; chute mesurée de 2,5 mV sur contact testé en mode piles.

### F. Piles insuffisantes — **à retester proprement**

- Une paire a présenté ~2,546 V ensemble ; valeurs historiques diverses.
- Au moins une NiMH a chauffé lors d'un mauvais test de continuité et ne doit pas servir de référence sans contrôle.
- Utiliser une paire de piles **saines**, pleinement chargées et appariées ; relever **pack sous charge** et `U34 OUT` dans les mêmes secondes.

---

## 7. Protocole de reprise conseillé (sans dessoudage immédiat)

### Série 1 — valeurs DC comparables, même séance

1. GP2X remontée juste assez pour fonctionner sans court-circuit, masse commune stable.
2. Deux AA saines, **piles ON**, relever au même moment :
   - pack AA sous charge ;
   - côté alimentation de L35 (**pas LX**) ;
   - U34 `EN`, `OUT/FB` ;
   - U35 `IN`, `FB` ;
   - U27 `VIN`, `SHDN`, `VOUT` ;
   - éventuellement borne active de C253.
3. Relever exactement les mêmes points **secteur ON**.
4. Noter si le voyant et l'écran changent pendant les mesures. Éviter de modifier l'appui mécanique sur le PCB.

### Série 2 — validation topologique hors tension

- Confirmer une nouvelle fois `U34 OUT ↔ U35 IN` ~0 Ω.
- Déterminer si `U27 SHDN` rejoint la **sortie filtrée** de U35 (après L30), une autre sortie, ou réellement un nœud `SW` : tester la continuité vers **chaque côté de L30** et vers le **côté haut de la résistance 244**, hors tension.
- Repérer une éventuelle alimentation alternative / diode-OR vers SHDN en mode piles.
- Archiver des photos nettes des **deux faces** avec même orientation, échelle et référence de carte.

### Série 3 — si le défaut persiste

- Avec un **oscilloscope adapté** et une méthode de masse sûre, observer `LX` U34 en mode piles et secteur pour constater une réelle activité de commutation. **Le 0 Hz au MM400 ne permet pas ce verdict.**
- Rechercher une surcharge dynamique sur le rail U34 OUT/U35 IN et mesurer si possible le courant sans improviser de court-circuit avec le multimètre.
- Mesurer C233 **hors circuit** (capacité + ESR si appareil approprié), plutôt que considérer 129 µF in-circuit comme probant.
- Ne commander/remplacer U34 qu'après comparaison coûts/risques et confirmation du boîtier et des pistes.

### Journal vierge réutilisable

| Date | Source | Pack (V) | L35 entrée (V) | U34 EN (V) | U34 OUT (V) | U35 IN (V) | U27 SHDN (V) | Écran |
|---|---|---:|---:|---:|---:|---:|---:|---|
| AAAA-MM-JJ | Piles ON | | | | | | | |
| AAAA-MM-JJ | Secteur ON | — | | | | | | |

---

## 8. Inventaire / références pour réparation

| Position carte | Marquage | Identification | Boîtier / contrainte | Statut |
|---|---|---|---|---|
| **U34** | `HBCA` | **Active-Semi ACT6305UC-T**, boost synchrone | **SOT-23-5**, brochage exact obligatoire | **[CONFIRMÉ]** par marquage + fiche |
| **U35** | `IAFM` | **Active-Semi ACT6906UCADJ-T**, buck réglable | SOT-23-5, brochage différent de U34 | **[CONFIRMÉ]** par marquage + fiche |
| **U27** | non consigné de façon lisible | **Analog Devices / Linear Technology LTC3426**, boost | 6 broches TSOT-23 | **[FORTEMENT PROBABLE]**, vérifier marquage |
| **C233** | `107 A ...` | **~100 µF**, condensateur CMS polarisé probable | **Dimensions, tension nominale et polarité à confirmer** avant achat | **[À VÉRIFIER]** |
| **L35** | — | Inductance associée à U34 | Valeur d'inductance non identifiée | **[IDENTIFIÉE fonctionnellement]** |
| Jack DC | — | **DC-011C** SMD/coudé monté lors de cette réparation | Vérifier encombrement, brochage, contacts de commutation et polarité | **[REMPLACÉ]** |

**Achat de C233 :** la référence **TPSD107M010R0065** / **TPSD107M010R0080** (tantale 100 µF, 10 V, boîtier D pour certaines variantes) avait été évoquée comme **candidate**, **pas comme équivalence physique validée**. Ne pas la promouvoir comme remplacement garanti tant que **L × l × h**, tension et polarité ne sont pas confirmés. Les tantales ont une polarité à respecter ; pour de nombreuses séries, la bande marque le **positif**, mais vérifier la datasheet exacte.

**Achat de U34 :** rechercher exactement **ACT6305UC-T**, `HBCA`, SOT-23-5 ; la disponibilité peut être limitée. Ne pas acheter un boost SOT-23-5 quelconque : pinout, FB et comportement peuvent différer.

---

## 9. Erreurs corrigées pendant l'enquête

Ce tableau est volontairement conservé : il évite qu'un futur lecteur reproduise les mêmes erreurs.

| Ancienne affirmation | Correction |
|---|---|
| « HBCA = U37 » | **Faux pour ce PCB** : le HBCA étudié avec L35/C233 est **U34**. |
| « Le condensateur marqué 22 16V est L38 » | Faux : c'est **un condensateur** ; sa référence exacte est à confirmer. |
| « SHDN U27 est forcément relié directement à SW U35 » | **Non démontré**. Des tensions identiques ne suffisent pas ; une liaison après inductance serait aussi possible. |
| « Les points à 2,273 V sont nécessairement en série avant U35 » | **Non démontré** sans cartographie de pistes. |
| « HBCA définitivement HS » | Trop catégorique ; **sortie anormale, cause encore ouverte**. |
| « C233 129 µF est correct / HS » | Aucune des deux conclusions n'est garantie **en circuit** ; pas de mesure ESR. |
| « 0 Hz sur LX = le régulateur ne commute pas » | Faux : le **MM400 ne permet pas** d'en conclure l'absence d'activité HF. |
| « 2.000 Ω entre OUT et GND » | Mauvaise lecture : l'utilisateur avait indiqué **2,0 MΩ**. |
| « 3,3 V régulés sur toute la carte côté secteur » | Plusieurs rails distincts existent ; ne pas confondre `U34 OUT`, `U35 OUT`, `U27 VOUT`, `C253`. |

---

## 10. Sources primaires et documentation utile

- **ACT6305UC-T — datasheet Rev. 3 (15 novembre 2012)** : [PDF, Active-Semi](https://www.semiconductors-ic.com/files/d3/act6305uc-t.pdf). Marquage `HBCA`, pinout FB/G/EN/LX/OUT, réglage 3,3 V avec FB=OUT, seuil EN, plage de démarrage.
- **ACT6906UCADJ-T — datasheet** : [PDF, Active-Semi](https://www.ic-components.kr/files/a2/ACT6906UCADJ-T.pdf). Marquage `IAFM`, buck réglable, FB 0,6 V, pinout EN/G/SW/IN/FB.
- **LTC3426 — datasheet officielle Analog Devices** : [PDF](https://www.analog.com/media/en/technical-documentation/data-sheets/3426fb.pdf) ; [fiche produit](https://www.analog.com/en/products/LTC3426.html). Pinout SW/GND/FB/SHDN/VOUT/VIN, FB ~1,22 V, SHDN actif haut.
- **Klein Tools MM400** : consulter la documentation de l'appareil pour les modes V⎓, continuité, résistance et capacité ; toujours sélectionner explicitement le bon mode.

Les identifications **U34/ACT6305** et **U35/ACT6906** sont appuyées par les codes imprimés et les datasheets. La correspondance **U27/LTC3426 sur cette carte** reste à confirmer par une photographie du marquage ou une documentation de révision PCB.

---

## 11. Organisation suggérée pour un dépôt Git

```text
gp2x-hardware/
├── README.md                      # Présentation et index
├── docs/
│   ├── gp2x-f200-power-diagnosis.md  # Ce rapport
│   ├── pcb-revisions.md           # Versions F100/F200, changements connus
│   ├── power-rails.md             # Rails, séquencement, cartographie validée
│   ├── components.md              # Nomenclature et correspondance marquages
│   └── measurement-protocol.md    # Méthodes et sécurité
├── data/
│   └── measurements.csv           # Mesures datées, état piles / secteur
├── media/
│   ├── board-front/              # Photos originales, non annotées
│   ├── board-back/
│   └── schematics/                # Dessins VALIDÉS, pas visuels approximatifs
└── LICENSE
```

**Bonne pratique communautaire** : chaque pinout ou chemin de piste publié doit porter une source (photo de sérigraphie, datasheet ou référence d'une mesure de continuité). Éviter de publier des annotations de photos produites automatiquement sans validation électrique. Ajouter un identifiant de révision de PCB et un numéro de série **masqué** pour les contributions externes.

---

## 12. Conclusion actuelle

1. **Le jack remplacé et SW1 permettent désormais un démarrage fiable sur secteur.**
2. **En mode piles, U27 reçoit du VIN mais son SHDN reste bas** : explication directe de l'absence de régulation U27.
3. **U34/HBCA est activé et alimente directement U35 IN, mais sa sortie reste à ~2 V sur piles**, alors que `FB = OUT` demande théoriquement 3,3 V.
4. **Le même U34 OUT n'atteint que ~2,791 V sur secteur** malgré un démarrage correct : élément contradictoire important, qui interdit de déclarer U34 définitivement défectueux à ce stade.
5. **C233 affiche ~129 µF en circuit**, insuffisant pour diagnostiquer sa santé ou son ESR.
6. **Prochaine priorité** : campagne de mesures DC simultanées avec de bonnes piles, puis cartographie de la sortie U35 vers U27 SHDN ; oscilloscope si possible avant changement de composants.

**Statut final : panne d'alimentation sur piles localisée à la chaîne de conversion/activation, composant responsable non encore prouvé.**
