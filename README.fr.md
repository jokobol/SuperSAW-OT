# SuperSAW-OT

**Une machine SuperSAW expérimentale pour une piste audio de l’Octatrack MKII, en cours d’intégration au runtime Modwerk.**

Projet porté par **José Aleixo — jokobol**, développé avec l’aide d’un assistant de programmation IA.

L’idée : un son de dents de scie empilées et désaccordées, avec six commandes : **TUNE, DETN, MIX, OSC, WIDE et LEVL**, dont une à sept oscillations en dents de scie. L’objectif est d’utiliser une piste SuperSAW et de garder les sept autres pour les samples.

## Où nous en sommes

**V0.31 : sources de développement, aucun fichier prêt à installer.**

Le moteur sonore et ses régressions sont testés. Le nouveau raccordement Modwerk passe **18 contrôles en émulation**, comprenant le chargement USB virtuel, Trial/Undo, l’affectation à une piste, les notes MIDI, leur relâchement, STOP, FUNC+STOP, le redémarrage et le retrait après retour à FLEX.

Les essais réels sur Octatrack n’ont pas commencé. La charge CPU/DSP physique reste à mesurer. L’installation attend notamment la protection de la signature SW/1 lorsque le module manque et la validation de la combinaison exacte base/module, puis des changements de projet et de Part.

Le dossier public contient une présentation, les sources et les résultats textuels. Il ne contient aucun OS Elektron, firmware modifié, module compilé ou fichier à flasher.

## La suite

Finaliser l’intégration et la protection des projets, préparer un candidat identifié, réaliser les essais sur une machine réelle, mesurer les ressources, puis demander la revue Modwerk pour une éventuelle première publication au catalogue.

[Présentation anglaise et avancement détaillé](README.md) · [Sources et contrôles techniques](sources/V0.31/README.md)

## Démo SuperSAW sur l’Octatrack

[▶ Voir et écouter la démo](SuperSAW-OT-demo.mp4)

Les crédits et licences d’origine sont conservés : voir [CREDITS.md](CREDITS.md).
