# SuperSAW-OT

**An experimental monophonic synth track machine for the Octatrack MKII, tested in RAM on a real instrument and being prepared for technical review by Modwerk.**

Project by **José Aleixo — jokobol**, developed with AI coding assistance.

[Présentation en français](README.fr.md) · [Development journal](JOURNAL.md)

## SuperSAW on the Octatrack

[▶ Watch and listen to the demo](SuperSAW-OT-demo.mp4)

This video shows an earlier stage of the prototype.

## The idea

SuperSAW-OT combines one to seven saw oscillators, with detuning and stereo spread. It provides a source for basses, leads and thicker textures while using the Octatrack’s sequencer, AMP, parameter locks, LFOs and track effects.

| SRC control | Purpose |
|---|---|
| TUNE | Pitch |
| DETN | Oscillator detuning |
| MIX | Balance between the centre oscillator and detuned oscillators |
| OSC | Number of oscillators, from 1 to 7 |
| WIDE | Stereo spread |
| LEVL | Source output level |

The machine is monophonic, with one instance per Part. OSC changes oscillator count, not the number of independently played notes. A sub oscillator and chord mode are deferred to focus on qualifying this version.

## Octatrack MKII hardware tests

Private RAM trials on Modwerk base 0.7.13 confirmed sound on T1, with:

- Sequencing and parameter locks on TUNE, DETN and WIDE.
- A filter in FX1 and delay in FX2, including parameter locks.
- Short FLEX samples on T2, T3 and T4.
- No audible clicks or saturation reported during this listening session.

Earlier hardware trials also produced positive reports for AMP envelopes, LFO modulation and STOP.

The complete 22 November session contains 600 readings and a maximum of **57.1% global CPU**. These readings describe the whole instrument in that project, not the isolated cost of SuperSAW.

## Ending RAM trials

An automatic exit at ten minutes failed with `needs-recovery`. A later check found the instrument already in normal mode, with a known identity and zero modules.

The host script now requests operator confirmation before timed Undo. **Two consecutive short-mode hardware checks confirmed Undo, the previous identity and zero modules**, following STOP → T1 FLEX → AMP → Q.

The internal cause of the earlier refusal remains unresolved.

## Current status

**Functional prototype undergoing qualification. No installable module is distributed in this repository.**

The public sources in `sources/V0.31` are a historical snapshot. The current working dossier and latest reports have not yet been fully published here.

The reference sound engine remained unchanged during the latest trials.

Work still includes:

- Integration into Modwerk’s public source/package build process.
- Measuring the isolated renderer and at-rest CPU cost.
- Protecting projects when the SuperSAW machine is absent.
- Qualifying Part/project operations, Keep, removal and physical reboot persistence.
- Completing the required checks and technical review before a possible public release.

## About this repository

This is a public development showcase, not an approved Modwerk catalogue release or a replacement for the full Modwerk SDK.

No Elektron OS, modified firmware or flashable image is distributed here. Compiled module packages, private card images and emulator binaries are not included.

Historical technical documentation:

- [V0.31 source snapshot](sources/V0.31/README.md)
- [V0.31 test evidence](sources/V0.31/TESTING.md)
- [Development journal and newer hardware reports](JOURNAL.md)

## Credits and licensing

José Aleixo leads the project. Existing engine/integration references and tools retain the credits and licenses of **Tim Hastie**, **Sam Banks / Octabam**, and **Modwerk / Elekloader** contributors.

See [CREDITS.md](CREDITS.md) and the license files in the source snapshot. Per-file licenses apply; this repository does not replace them with a single blanket license.

Feedback on runtime integration, missing-module behaviour and hardware qualification is welcome.
