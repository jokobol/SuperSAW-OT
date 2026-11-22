# SuperSAW-OT

**Une machine de synthèse monophonique expérimentale pour l’Octatrack MKII, testée en RAM sur une machine réelle et en préparation pour une revue technique Modwerk.**

Projet porté par **José Aleixo — jokobol**, développé avec une assistance IA au codage.

SuperSAW-OT associe de un à sept oscillateurs à dents de scie, avec désaccordage et répartition stéréo. Une source destinée aux basses, aux leads et aux textures plus épaisses, en utilisant les possibilités de jeu de l’Octatrack.

## Les six paramètres SRC

| Paramètre | Fonction |
|---|---|
| TUNE | Hauteur du son |
| DETN | Désaccordage entre les oscillateurs |
| MIX | Équilibre entre oscillateur central et oscillateurs désaccordés |
| OSC | Nombre d’oscillateurs, de 1 à 7 |
| WIDE | Répartition stéréo |
| LEVL | Niveau de sortie |

La machine reste monophonique, avec une instance par Part. OSC modifie le nombre d’oscillateurs, pas le nombre de notes indépendantes. Le sub et les accords sont reportés pour privilégier la validation de cette version.

## Essais sur Octatrack MKII

Les essais privés sur la base Modwerk 0.7.13 ont confirmé le son sur T1, avec :

- Séquençage et P-locks sur TUNE, DETN et WIDE.
- Filtre en FX1 et delay en FX2, avec P-locks.
- Samples FLEX courts sur T2, T3 et T4.
- Aucun clic ni saturation signalés pendant cette écoute.

Les essais matériels antérieurs avaient également donné des retours positifs sur l’AMP, les LFO et STOP.

Le journal complet de la session du 22 novembre comporte 600 mesures et un maximum de **57,1 % de CPU global**. Ces mesures concernent tout l’instrument dans ce projet : elles ne représentent pas le coût de SuperSAW seul.

## Fin des essais RAM

Une tentative de retrait automatique à dix minutes a échoué avec `needs-recovery`. Une vérification ultérieure a trouvé la machine déjà en mode normal, avec une identité connue et zéro module.

Le script demande désormais une confirmation manuelle avant le retrait. **Deux contrôles courts consécutifs ont confirmé l’Undo, l’identité précédente restaurée et zéro module**, après STOP → T1 sur FLEX → AMP → Q.

La cause interne du refus précédent reste à établir.

## État du projet

**Prototype fonctionnel en cours de qualification. Aucun module prêt à installer n’est distribué dans ce dépôt.**

Les sources publiques dans `sources/V0.31` constituent un instantané historique. Le dossier de travail actuel et ses derniers rapports ne sont pas encore intégralement publiés ici.

Restent notamment :

- L’intégration du paquet dans la chaîne publique Modwerk.
- La mesure du coût propre au moteur.
- La protection des projets lorsque la machine SuperSAW est absente.
- La qualification des sauvegardes, du Keep et des redémarrages physiques.
- Les contrôles et la revue nécessaires avant une éventuelle publication.

Le moteur sonore de référence reste inchangé pendant ces derniers essais.

## Démo

[▶ Voir et écouter la démo SuperSAW](SuperSAW-OT-demo.mp4)

Cette vidéo illustre une étape antérieure du prototype.

## Suivi et crédits

- [Journal des essais](JOURNAL.md)
- [Présentation anglaise](README.md)
- [Instantané historique des sources V0.31](sources/V0.31/README.md)
- [Crédits et licences](CREDITS.md)

Les crédits et licences des composants utilisés sont conservés. Aucun OS Elektron, firmware modifié ou fichier à flasher n’est distribué ici.
