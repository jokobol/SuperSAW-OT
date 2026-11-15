# SuperSAW-OT

**An experimental SuperSAW track machine for the Elektron Octatrack MKII, being developed for the Modwerk runtime.**

Project by **José Aleixo — jokobol**, developed with AI coding assistance.

[Présentation en français](README.fr.md) · [Technical source snapshot](sources/V0.31/README.md) · [Test evidence](sources/V0.31/TESTING.md)

## The idea

Bring a wide, detuned saw sound to one Octatrack audio track, with six source controls:

| Control | Purpose |
|---|---|
| TUNE | Pitch |
| DETN | Oscillator detuning |
| MIX | Oscillator balance |
| OSC | One to seven saw oscillators |
| WIDE | Stereo width |
| LEVL | Source level |

The intended setup uses one SuperSAW track and leaves the other seven tracks available for samples. Real-device CPU/DSP headroom for that setup has not been measured.

## Current stage — V0.31 source draft

**Work in progress: not ready to install. No firmware or installable module is included.**

The synthesis engine has passed regression tests. The new Modwerk bridge passes **18 checks in an emulator**, including virtual USB Trial/Undo, track assignment, MIDI note and release, STOP, FUNC+STOP, restart, and removal after leaving the track. Seven MAIN output captures were checked; their hashes are included in the evidence.

These results exercise a private base and modeled hardware. They do not establish physical USB reliability, real CPU/DSP cost, saved-project survival, or reboot behavior on an Octatrack.

Before a first real-device trial, we still need to qualify the exact installable base/module combination. In particular, the pinned base needs to recognize the SW/1 signature when the module is missing. A proposed base change has passed host tests on temporary copies; it has not been applied to the tested base or qualified through a reboot.

## Roadmap

- [x] SuperSAW source engine and controls
- [x] Frozen-engine regression checks
- [x] Draft runtime integration and direct stereo packets
- [x] Controlled virtual USB/base/DSP scenario: 18 checks
- [x] Manual STOP/restart regression and FUNC+STOP behavior in the emulator
- [ ] Integrate and qualify missing-module SW/1 protection in an appropriate base
- [ ] Validate project/Part changes and the exact installation candidate
- [ ] First Octatrack MKII hardware trial
- [ ] Measure CPU/DSP headroom and coexistence with sample tracks and effects
- [ ] Hardware persistence/removal qualification and Modwerk first-release review

## About this repository

This is a public development showcase with a source snapshot. It is not a Modwerk catalogue release or a replacement for the full Modwerk SDK. The snapshot belongs under `sdk/drafts/supersaw/` in the pinned SDK for integration work. Build prerequisites and limits are documented in the technical folder.

The official OS stays with its owner and is not included. The private base, compiled packages, emulator binaries and audio captures are not included either. Source evidence paths have been normalized for this public export; measured code/package hashes are retained.

## Credits and licensing

José Aleixo leads the project. Existing engine/integration references and tools retain the credits and licenses of **Tim Hastie**, **Sam Banks / Octabam**, and **Modwerk / Elekloader** contributors. See [CREDITS.md](CREDITS.md) and the license files in the source snapshot. Per-file licenses apply; this repository does not replace them with a single blanket license.

Feedback on runtime integration, missing-module behavior and hardware qualification is welcome.
