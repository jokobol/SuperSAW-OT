# SuperSAW-OT — current technical review snapshot

Updated 22 November 2026. Experimental source review; not an installable release.

[Download the source review archive](SuperSAW-OT-V0.32-source-review.zip) · [Development journal](JOURNAL.md)

Archive SHA-256: `c94980e59c55f3b9e2146526993ff77bcf425aa81d9bf00bf582d76ab1353475`

Source revision of the local Modwerk contribution branch: `26e8f811a5d047d0d26a78782297b0aad563dc29`.
This revision is not a commit in this standalone showcase repository.

## What is included

The V0.32 source draft, unchanged reference engine, source notices and licences,
SDK-facing bridge and declarations, host test clients, presentation material,
and historical/current test reports. A file-hash manifest is inside the archive.
The full Modwerk SDK and private test fixtures are separate prerequisites;
this archive is not a standalone build or installation tool.

No firmware, installable package, card image, compiled emulator or audio dump
is included. Five historical raw signature-memory fields are omitted from the
exported reports; all such omissions are listed in EXPORT_MANIFEST.json.
The local original reports remain unchanged.

## Current hardware evidence

On José Aleixo's Octatrack MKII, public Modwerk base 0.7.13:

- Unassigned RAM Trial/Undo passed with the exact previous identity restored.
- José reported sound on T1, sequencing, parameter locks, filter and delay,
  alongside short FLEX samples on T2–T4, without clicks or saturation.
- The timed listening exit failed with needs-recovery; a later check found
  normal mode and zero modules already at entry.
- After revising the host script to wait for operator confirmation, two
  consecutive short-mode checks confirmed Undo, previous identity and zero modules.

Exact tested private package SHA-256:
`52df3143854f203044dde02d82a634bf2e5d5e1723c1adcd8c0e11a7afe2f476`.
The reference engine is unchanged. Rebuilding this exact package through the
current public source recipe remains pending. Hardware tests of the private
package do not qualify an untested newly built public package.

Global CPU peaked at 57.1% over the full recorded listening session. This is
not an isolated renderer cost or a guarantee for other projects.

## Where to look in the archive

- REVIEW_REQUEST.md: detailed experimental review request.
- TESTING.md: chronological tests, failures and limits.
- SESSION_END_REVIEW.md: old timeout path and revised host confirmation flow.
- evidence/manual-confirm-v1-hardware-end-check-1.json and -2.json: two exits.
- SUBMISSION_STATUS.md: outstanding public packaging and qualification work.
- provenance.json and LICENSE files: frozen engine hashes and source attribution.

Public packaging, source/build parity, isolated renderer/rest cost, SW/1
absent-machine protection, saved projects, Keep and physical reboot remain
open. The underlying cause of the earlier firmware refusal is not established.
This snapshot has not been approved or listed by Modwerk.
