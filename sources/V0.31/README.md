# SuperSAW V0.31 — préparation de l’essai réel

**Sources de développement. Aucun firmware ou module à installer dans ce ZIP.**

Le raccordement à la base Modwerk progresse : affectation à une piste, refus
d’une seconde SuperSAW dans la même Part, refus de retrait lorsqu’elle est utilisée,
puis retour à FLEX et retrait passent dans le modèle USB natif. Les protections
du firmware restent actives. Le moteur sonore V0.27 et ses douze fichiers gelés
restent identiques.

Le scénario USB/base/DSP final passe ses **18 contrôles** : note manuelle,
FUNC+STOP sans coupure, relâchement, STOP vers le silence, nouvelle note,
nouveau relâchement et retrait après retour à FLEX. Voir `TESTING.md` et
`evidence/usb-dsp-lifecycle.json`. Ces résultats concernent l’émulateur.
Aucun essai physique, aucune installation ou mise à jour de l’Octatrack n’a été fait.

## Raccordement

- Machine SUPERSAW, signature SW/1, descripteur ABI de 80 octets et START typé
  séparé de 16 octets. Une seule instance admise par Part via le sélecteur.
- Page SRC : TUNE, DETN, MIX, OSC, WIDE, LEVL ; OSC enregistré 0–6, affiché 1–7.
  Valeurs initiales : 64, 85, 127, 6, 127, 80.
- Production directe de paquets PCM32 stéréo, sans voix FLEX servant de support.
- MIDI et touches chromatiques utilisent les événements de la base ; leurs
  continuations natives de note tenue et de gate sont conservées.
- Relâchement et arrêt d’une voix native publient des tickets de génération.
  Le moteur audio les applique ; un ticket d’une ancienne note ne coupe pas
  sa remplaçante. Un START accepté après un arrêt peut redémarrer normalement.
- Le callback `module_key` observe STOP puis laisse la touche à la base.
  FUNC+STOP est exclu à partir des lignes de touches natives, même si FUNC
  était tenu avant le chargement du module. Aucun site partagé n’est patché.
- Le callback `ev_tail_kill` reste distinct : c’est la coupure native des queues.

## Ce qui a changé dans le banc d’essai

La fixture V0.30 stationnait les appels empruntés dans une ancienne boucle stock,
hors de la boucle idle reconnue par la base. Avec l’exécution réelle des
instructions idle et le retour à la boucle reconnue, l’affectation passe.
Aucun seuil CPU, compteur invité ou garde n’est forcé. Les valeurs CPU du modèle
ne sont pas des mesures du coût de SuperSAW sur le matériel.

L’horloge virtuelle est maintenant valide. PLAY et STOP doivent changer le
transport natif avant le test de note manuelle : une fenêtre de date ne peut
plus faire passer un échec de livraison de touche pour un défaut sonore.
Les essais négatifs antérieurs sont conservés dans `evidence/stop-regression.json`.

## Avant l’installation

Les préparatifs de la machine du développeur sont suivis séparément.
Aucun essai physique ou transfert de récupération n’est qualifié dans cet export public.

La base épinglée ne connaît pas encore SW/1 lorsque le module manque.
`SW1_BASE_PROPOSAL.patch` est une proposition **non appliquée** ; son test hôte
vérifie le sélecteur exact, la protection de validation et le routage silencieux,
avec et sans l’ajout proposé, plus la suite existante du registre.
Ce test ne qualifie pas le redémarrage d’une base modifiée.

Restent aussi les changements de Part/projet, les signatures enregistrées en
plusieurs exemplaires, les mutes, les scènes et l’enregistrement sur ce nouveau
raccordement, puis la compatibilité avec une base installable identifiée et les
mesures réelles CPU/DSP. Le coût CPU à zéro du descripteur demande le forfait
de repli de la base ; il ne signifie pas une consommation nulle.

Les images privées `NOT_FLASH_CANDIDATE` ne sont pas des fichiers à installer.
La première installation de base et le chargement d’un module par USB sont deux
opérations distinctes. Le catalogue public et `module:doctor` restent non qualifiés.

## Reproduire

Depuis un checkout SDK épinglé, avec clang, ASan/UBSan et m68k-elf :

- `python3 -B sdk/drafts/supersaw/verify.py`
- `python3 -B sdk/drafts/supersaw/verify_native.py --native CHEMIN_OCTABAM --stock MAIN_PRIVE --out NOUVEAU_DOSSIER`
- `python3 -B sdk/drafts/supersaw/verify_absent_signature.py --sdk . --out RAPPORT_JSON`
- `build_private.py --help` et `build_pipe.py --help` décrivent les constructions
  privées. Le paquet exige le conteneur original ; la fixture ColdFire exige sa
  section MAIN. Ces fichiers restent locaux et ne sont pas distribués.

Le banc USB utilise les classes SDK d’origine et les modèles RTOS/USB/CF natifs
par stdin/stdout, sans réseau ni API USB physique. Le profil `frame-dsp` utilise
les deux DSP en interprétation ; `frame-only` ne prouve pas la sortie sonore.
Les options `--step-idle --resume-idle` ne modifient que l’émulateur privé.

## Provenance et licences

Projet de José Aleixo. Moteur gelé depuis le commit V0.27 `18c1296` ; empreintes
dans `provenance.json`. SDK Modwerk : `b52d11e00a124a7e1ae35d23eff5b4d072ab961f`.
Elekloader : `3b31ef3c0e43c810e3acd0be6257e2484b20fc40`.

Moteur et passerelle sous MIT ; notices de Tim Hastie et Sam Banks conservées
dans `LICENSE-TIM` et `LICENSE-OCTABAM`. En-têtes SDK sous leurs licences
d’origine : `modwerk_page.h` MIT, `modwerk_events.h` GPL-3.0-or-later ;
`support/m68k_codegen.py` GPL-3.0-or-later. Texte GPL inclus.
