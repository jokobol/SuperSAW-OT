# Development journal

## 16 November 2026 — First hardware tests


Quick SuperSAW-OT update: it now makes sound on my Octatrack MKII! 🎉

In private RAM trials, I’ve tested the six sound controls, sequencing, AMP envelopes, P-locks and LFO modulation, including DETN/WIDE. No audible clicks during these musical tests, and RELEASE gives a smooth end to the notes.

Total CPU readings were around 60% during the sequence, with a full-session peak of 66.1%. These are whole-device readings, not an isolated SuperSAW benchmark.

The latest trial ended with a confirmed Undo and zero modules retained. Earlier intermittent Undo refusals remain under investigation.

Next: testing SuperSAW alongside sample tracks. This is still experimental — multi-track coexistence and Keep/reboot persistence remain untested. No installable release yet.

I’m a musician developing this with AI coding assistance. Thanks to the Modwerk community!

# Journal de SuperSAW-OT

## Session du 15 au 16 novembre 2026 — premiers essais matériels

Projet de José (jokobol), Octatrack MKII, projet de test SS_TEST.
Essais privés en RAM, une seule piste SuperSAW, sans Keep ni sauvegarde
pendant les essais. Base Modwerk publique `27c9d36f…` ; paquet exact
`52df3143854f203044dde02d82a634bf2e5d5e1723c1adcd8c0e11a7afe2f476`.
Le moteur sonore reste inchangé ; la version de dossier est V0.32 source draft.
Les sauvegardes CF et Mac ont été confirmées par José avant installation.

### Résultats d’écoute rapportés par José

| Fonction | Observation |
| --- | --- |
| Déclenchement / STOP | Son déclenché sur T1 ; STOP arrête le son. |
| DETN | Effet souhaité clairement obtenu. |
| MIX | Variation progressive ; légère remontée de volume perçu entre 18 et 0, à mesurer. |
| WIDE | Variation de largeur stéréo nettement perçue. |
| OSC | Changements d’épaisseur par pas, acceptés pour le choix du timbre ; plage affichée exacte encore à confirmer. |
| LEVL | Variation douce, sans clic ni saturation ; aucun son perceptible à 0 via MOTU 828. |
| TUNE | Hauteur réactive et résultat apprécié, y compris dans le grave ; justesse non mesurée. |
| Séquenceur | Quatre trigs, notes courtes réglées avec AMP HOLD/REL ; répétitions régulières et relâchement doux. |
| P-locks / LFO | Modulations essayées, notamment DETN/WIDE ; aucun clic signalé. |

Ces observations sont des écoutes utilisateur, sans enregistrement audio analysé.

### Relevés et retrait

La dernière session comporte 503 relevés sur environ 8 min 23 s. Les relevés
présentés pendant la séquence montrent 59,7 à 60,5 % CPU ; le maximum de tout
le journal est 66,1 %. DSP affichés alors : pistes 1–4 à 38,5 %, pistes 5–8
à 41,9 %. Les relevés n’ont pas de marqueurs d’action : aucun pic n’est
attribué à un geste précis. Ce sont les charges globales de l’instrument,
pas le coût isolé de SuperSAW ni une qualification de huit pistes simultanées.

Fin de session : STOP, T1 remis sur FLEX, Q dans le Terminal. Le journal
confirme Undo, zéro module conservé et libération USB. Voir
`evidence/hardware-sequence-controls-20261116.json`.

### Incidents et corrections du lanceur

Deux sessions antérieures ont refusé le retrait (`needs-recovery`). Les
contrôles ultérieurs ont trouvé la base déjà normale et l’inventaire vide ;
on n’attribue donc pas leur restauration au script de récupération.
La cause de ces refus reste ouverte. La saisie FIN était brouillée visuellement
par les relevés CPU : désormais les mesures restent enregistrées sans défiler,
CPU/C affiche le dernier relevé, FIN/Q demande la fin et les avertissements
préservent la saisie. Les refus explicitement acquittés peuvent être repris
pendant 10 s, conformément au minimum du site ; une réponse de transport
inconnue arrête toujours le processus. Aucun garde-fou invité n’a été modifié.

La console et le retrait ont réussi au cours des essais suivants, dont la
session séquencée ; cela ne démontre pas la disparition de tous les cas de refus.

### Reprise prévue

José termine les essais pour cette nuit. Prochain scénario : SuperSAW sur T1
avec samples sur d’autres pistes, en augmentant progressivement la charge.
Restent à qualifier : coexistence avec les sept pistes de samples, notes MIDI
et jeu chromatique, justesse, niveaux mesurés, Keep et redémarrage, installation
publique et revue Modwerk. Aucun module n’a été soumis au catalogue ou publié
par cette mise à jour. Aucune nouvelle installation n’est nécessaire ce soir.
