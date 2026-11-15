# SuperSAW V0.31 — contrôles et limites

Contrôles du 15 novembre 2026, Mac arm64. **Aucun test sur matériel physique.**
Les résultats V0.30 conservés plus bas sont historiques ; leur blocage CPU ne
décrit pas la fixture corrigée.

| Contrôle V0.31 | Résultat observé | Limite |
|---|---|---|
| Empreintes du moteur | 12 fichiers identiques à V0.27 | Le raccordement USB est nouveau |
| Passerelle ASan/UBSan | 38 824 paquets, 320 000 échantillons stéréo | Simulation hôte |
| Oracle scalaire gelé | 212 600 appels, 1 695 200 échantillons stéréo | Régression du moteur, pas sortie complète firmware |
| Fixture ColdFire ciblée | 2 228 appels, aucun échec | Contextes fournis, pas RTOS/base/DSP |
| Paquet privé | ABI 8, START typé, relocalisations, reconstruction exacte et deux scans stricts réussis | Pas de publication ou installation physique |
| Base avec frames, sans DSP | 11 contrôles réussis avec le candidat voice-reset | Pas de preuve sonore ; candidat antérieur au callback de touche |
| Proposition SW/1 absente | Suite du registre + sélection/version/types/validation/silence réussies sous ASan/UBSan | Copie temporaire seulement, base d’essai inchangée |
| Scénario USB/base/deux DSP final | 18 contrôles réussis, 7 captures MAIN | Modèle USB en mémoire ; pas de cycle d’alimentation |

## Arrêt et livraison des touches

Les premiers essais par écriture brute dans une mailbox ne prouvent pas une
pression STOP. Les premiers essais de touche sans précontrôle n’établissent
pas non plus sa livraison à l’interface native. Ils restent classés non qualifiés.

Avec une horloge valide et PLAY/STOP vérifiés sur le transport, le paquet V0.30
et le candidat voice-reset seul laissent une note MIDI manuelle tenue sonner
après STOP lorsque le transport était déjà arrêté. Les mesures et empreintes
sont dans `evidence/stop-regression.json` : cette assertion reste en échec,
elle n’a pas été relâchée pour obtenir une réussite.

Le candidat final ajoute `module_key` pour observer la pression STOP, sans
prendre la touche à stock, en excluant FUNC+STOP. Les demandes sont transmises
par ticket au renderer. Les tests ciblés vérifient STOP, FUNC+STOP, une nouvelle
note après reset et la libération de touche qui ne coupe pas une note suivante.
Le scénario DSP complet exige une note audible, FUNC+STOP sans coupure,
MIDI-off vers le silence, redémarrage, STOP vers le silence, nouvelle note puis
nouveau relâchement. Le seuil de silence tolère au plus un LSB PCM16.

## Résultat final dans la base et les deux DSP

`evidence/usb-dsp-lifecycle.json` : 18 assertions réussies sur le paquet privé
`52df3143854f203044dde02d82a634bf2e5d5e1723c1adcd8c0e11a7afe2f476`.
Deux constructions de ce paquet sont identiques octet pour octet.
Image 35 336 octets, BSS 2 416 octets, 88 relocalisations, 38 mots de table,
15 copies vérifiées par empreintes ; aucun site stock propre au module.
Les classes UploadSession et UsbVendorTransport du SDK restent inchangées.

- Trial, Undo, réinstallation, Keep et descripteur relocalisé passent.
- Affectation T1 et refus d’une seconde SuperSAW passent, sans désactiver les gardes.
- PLAY/STOP changent le transport natif avant le test de note manuelle.
- MAIN sonne sur note MIDI, reste audible après FUNC+STOP, puis finit au silence
  après note-off. Une nouvelle note sonne ; STOP la coupe au silence.
- Une nouvelle note après STOP sonne et son relâchement finit au silence.
- Le retrait d’un module utilisé est refusé avec le code 7 ; après retour à FLEX,
  sa signature disparaît et le retrait réussit.

Les trois captures de fin (release, stopped, after-stop-release) ont une crête
nulle dans leur dernière fenêtre de 100 ms. Les autres captures sonnent sans
atteindre une extrémité PCM16. Empreintes dans `evidence/main-captures.json` ;
les WAV restent locaux et ne sont pas inclus dans l’archive.
Les entrées de construction de l’émulateur sont dans `evidence/emulator-inputs.json`.
Le changement d’horloge, l’exécution idle et le retour d’appel sont des adaptations
de l’hôte ; le firmware, ses seuils et ses compteurs n’ont pas été forcés.
La valeur CPU préaffectation de ce modèle ne mesure ni le matériel ni le coût
incrémental de SuperSAW pendant le jeu.

## Qualification qui reste à faire

- Mise en place de la protection SW/1 dans une base appropriée, puis restauration
  après redémarrage, absence du module et projet/Part de test.
- Tous les gestes STOP, les mutes, les Part/pattern/project switches, le jeu
  chromatique complet et l’enregistrement dans le nouveau raccordement.
- Locks, LFO, scènes et effets sur cette base ; comparaison MAIN à V0.27.
- Paquet compatible avec la base installable exacte, USB physique, redémarrage,
  mesure ColdFire/DSP et marge avec les sept pistes de samples.
- Récupération SysEx via le MT4 : port et câblage déclarés, transfert non effectué.

Les tests hôte ou émulateur ne remplacent aucune de ces observations physiques.
Le banc de développement ne contient aucune API de commande USB physique.

---

# Historique V0.30 — preuves et limites

Contrôles exécutés le 14 novembre 2026, Mac arm64. Résultats exacts dans
`evidence/local-checks.json`. `verify.py` est le point de reproduction.

| Contrôle | Résultat | Ce que cela prouve |
|---|---|---|
| Empreintes du moteur V0.27 | Identiques, 12 fichiers | Le moteur et ses tests de référence n’ont pas été modifiés |
| Passerelle, ASan/UBSan | 38 824 paquets, 320 000 échantillons stéréo | PCM32 et état du moteur égaux à la commande V0.27 sur les scénarios simulés |
| Oracle scalaire V0.26, ASan/UBSan | 212 600 appels, 1 695 200 échantillons stéréo | Régression du moteur optimisé contre le moteur scalaire gelé |
| Descripteur/page sur hôte | Réussi | Plages, valeurs initiales, affichage OSC, masque SRC et admission dans le menu |
| Compilation MCF54455 | 6 objets | C/assembleur acceptés, descripteur 80 octets, START 16 octets, callback typé |
| Garde SDK de génération | Réussie | Pas d’instruction détectée avec index incompatible ni piège late-combine surveillé |

Le test des paquets couvre tous les découpages 0–16, des paramètres fractionnels,
des valeurs d’enveloppe, START, relâchement, STOP et redémarrage. Il vérifie aussi
les sentinelles après les paquets et le refus sans mutation des tailles invalides,
la priorité d’un START en attente, le silence avant initialisation et après
sortie, et l’indépendance de deux états de piste. Il ne teste pas les courses
entre tâches et interruption, les destinataires réels des événements ou les
écritures MMIO du raccordement cible.

## Fixture ColdFire native

`verify_native.py` : 2 211 appels sans échec. Le module est lié à une adresse
privée de la mémoire de l’émulateur, et les registres/contexte sont fournis par
le test. Le firmware officiel fournit les continuations MIDI et clavier ;
aucune donnée du firmware n’est incluse dans ces sources.

- Les signatures SW/1 des huit pistes sont reconnues, une piste sample voisine
  ne l’est pas ; le prédicat chromatique utilise la piste de l’événement même
  lorsque la piste sélectionnée au panneau est différente.
- MIDI 84 écrit TUNE 64, la note tenue et le bit gate via la continuation native.
- Un MIDI-off d’une ancienne note ne coupe pas la voix courante ; celui de la
  note courante garde les écritures natives de mailbox/gate et finit au zéro PCM32.
- Même contrôle pour les touches chromatiques anciennes/courantes, avec les
  branches natives d’acceptation ou de refus.
- Rendu audible à la source, en-têtes/curseur, paramètres intacts, maintien DSP
  de l’enveloppe et tous les points de découpage de 0 à 16.
- Transition lecture/STOP : source coupée. Appel direct du callback tail-kill
  lorsque le transport est déjà arrêté : source coupée.
- Conservation des registres C appelés et de la pile aux points mesurés.

L’ordre et la livraison des événements de notes par la base ne sont pas exercés.
Pas de démarrage RTOS, de notif d’enregistrement MIDI, de sortie DSP MAIN,
de contact USB, de capture LCD ou de mesure de temps matériel.
Les adresses et archives d’émulateur sont identifiées par leurs empreintes
dans `evidence/native-checks.json`. Le compteur d’appels n’est pas une mesure CPU.

La demande de relâchement est maintenant différée vers le moteur audio. Le
test hôte vérifie qu’elle ne modifie pas immédiatement le moteur et qu’un numéro
de génération périmé ne coupe pas la nouvelle note. Les interruptions aux
différents points du handler natif complet restent à tester.

## Non exécuté

- Affectation et rendu du prototype chargé dans la base Modwerk complète.
- Test des paquets à travers le DSP et comparaison de MAIN avec la V0.27.
- Distribution complète des événements MIDI/clavier, STOP global dans tous
  les états, commande DSP de redémarrage et coexistence avec les mutes.
- Affichage LCD, param locks/LFO/scènes sur la nouvelle base.
- Save/load, changement de Part, coexistence et protection si module absent.
- Cycle physique USB, remplacement, retrait d’un module utilisé et redémarrage.
- `module:doctor` : pas encore de manifeste de module installable.
- Tout test sur un Octatrack physique, mesure DTCN3, pourcentage CPU ou marge DSP.

Les tests de l’ancienne V0.27 ne qualifient pas ce nouveau raccordement USB.
Les compteurs d’appels et d’instructions ne sont pas des cycles matériels.

## Préparation de la base

Base privée reconstruite depuis le SDK épinglé et Elekloader
`3b31ef3c0e43c810e3acd0be6257e2484b20fc40` avec le conteneur OS 1.40C original
fourni localement. Les gardes du firmware, les contrôles de chevauchement, la
liaison et la relecture du conteneur passent. Aucun conteneur n’est distribué.

L’émulateur adapté du SDK démarre cette base jusqu’au contrôle RTOS M6a :
10 tâches créées, 190 dispatches, 11 contextes exécutés, aucune interruption audio.
Pas de carte/projet chargé, aucun module SuperSAW installé, aucun contact USB
ou matériel. Le profil privé n’est pas une preuve d’identité avec la base publique.
Les empreintes et limites sont dans `evidence/base-preparation.json`.

## Paquet et modèle USB dans la base — V0.30

Paquet privé ABI 8 : image 35 152 octets, BSS 2 384 octets, 85 relocalisations,
7 contributions/abonnements répartis dans 38 mots, aucun correctif de site stock.
Les 14 copies par empreintes reconstruisent exactement l’image compilée.
Deux scans stock-free inchangés et la vérification SDK du START typé passent.
Deux constructions finales donnent le même paquet octet pour octet.

Le premier test par socket est bloqué par l’environnement (`bind` refusé).
Le banc local par processus utilise ensuite les mêmes modèles RTOS, USB et
CompactFlash, avec les transferts EP0 pilotés par les classes SDK d’origine.
Aucune API USB physique n’est appelée, aucune protection du firmware n’est modifiée.

Le sous-ensemble `loader-only` passe : base vide, admission du Trial, Undo,
réinstallation/Keep, descripteur SW/1 relocalisé dans la table machines, retrait
du module inutilisé. La carte reste un objet en mémoire ; pas de redémarrage.
Voir `evidence/usb-loader.json` et `evidence/package-checks.json`.

Le scénario `full` **échoue encore à l’affectation T1**. Le prédicat du module
admet la piste, mais `mw_assign` refuse : CPU affiché 1000/1000, seuil 850/1000,
allocation de base 45 %. La fixture n’active pas l’interruption audio.
Ce refus est conservé dans `evidence/integration-blocked.json` ; le test
complet n’est pas transformé en réussite. Aucun seuil ni compteur n’est forcé.
Cela ne prouve ni une saturation réelle ni une marge CPU sur l’Octatrack.

Le chargement a révélé une autre erreur, corrigée : la banque native est à
0x400e21e0 dans cette base, alors que le prédicat V0.29 n’acceptait que la zone
à partir de 0x46000000. Le prédicat utilise maintenant les bornes du SDK ; la
fixture ColdFire teste les deux zones et le refus d’une borne invalide.
