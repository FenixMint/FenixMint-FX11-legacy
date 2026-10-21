# FX11 Native Builder: Windows-host first, Linux-host later

Date: 2026-10-21  
Status: **OWNER PREFERRED IMPLEMENTATION ORDER / OWNER DIRECTION**, not an acceptance of untested code.

## OWNER statement

OWNER: "mysle, żę najpierw zrobimy bulider uruchamiany na windows a pozniej na linux. chyba tak pojdzie szybciej"

Interpretation:
- Prioritize an **FX11 Builder that runs on Windows** and produces its first validated FX11 Windows installation image.
- After the Windows-hosted process and acceptance criteria have been established, continue / mature the **Linux-hosted builder**.
- This changes development priority, **not the existence or ownership** of the already developed Linux-native `FenixMint/FX11-Builder` repository.
- Initial reason: Windows-native tooling offers a more direct route for Windows image servicing and validation; **speed is a hypothesis to verify**, not a promised delivery date or benchmark.

## Existing work to retain

This decision is additive. Preserve:
- `FenixMint/FenixMint-FX11-legacy` with its Windows/PowerShell Native Builder, Tiny Retrofit, Windows Retrofit, policy/knowledge, Agent/Guards, Fenix System and independent visual layer.
- `FenixMint/FX11-Builder` with Linux-native ISO/WIM pipeline, audit/manifests, partition/WinPE/GRUB/installer work, tests and its separate project history.
- Historical tiny11 attribution and the FX11 independence/security posture.
- FX OS Linux (Fedora KDE first, LMDE later) as a **separate distribution modification/configuration track**; this is NOT the same as hosting an FX11 Windows-image builder on Linux.

No deletion, overwrite of history, forced repository merge, regression of Linux work or new distribution fork is authorized.

## Practical implementation direction — PLANNED, not implemented by this decision

### Stage W1: Windows builder minimal end-to-end

Work within the Windows/PowerShell generation, reusing and reviewing current code before implementation:
1. read-only discovery of host, input ISO and available Windows editions; verify provenance and SHA-256 of input media;
2. produce a reproducible build plan / selected profile, with safe default protections for Defender, Update, SmartScreen, WebView2, WinRE, servicing and mandatory business software dependencies;
3. manipulate a copied image through supported Windows servicing tools (e.g. DISM/PowerShell/ADK where appropriate), without modifying source media in place;
4. build a bootable candidate ISO with manifest, hashes and clear audit report of all requested deltas;
5. test in a disposable VM using genuine Windows media, then verify first boot, update/security/repair/compatibility behavior;
6. only after demonstrated success, consider First Run, UI, optional visual integration and wider host/hardware support as separately planned milestones.

Exact ISO tool choice, supported Windows version(s), profiles, GUI/CLI, and validation gates are **OPEN** until an implementation plan is reviewed; do not declare any stage complete on the basis of this note.

### Stage L: Linux-hosted FX11 builder

Once the Windows flow provides a trustworthy reference and measured outcomes:
- use the existing Linux-native FX11 Builder as a foundation, not a disposable prototype;
- compare and explicitly reuse **declarative product policy, manifests, audit/test fixtures and expected installed-system outcomes**;
- implement host-specific tooling as a different backend where necessary. Do not assume DISM/PowerShell image servicing can be ported 1:1 to Linux;
- validate real source media, WinPE boot, full install and security behavior independently; do not infer completion from Windows-hosted testing.

## Priority and relationship to existing roadmap

The existing `docs/roadmap/FX11-LEGACY-ROADMAP.md` lists Discovery, policy/planner and retrofit work before Phase 5 (Native Windows Builder). The newly stated OWNER preference **changes the near-term priority toward a Windows-hosted builder proof-of-concept**, not a silent cancellation of those phases. Mandatory read-only audit, safe planning and validation capabilities remain dependencies of the builder.

The Linux-hosted builder has a longer development horizon **by priority**, not because its project is abandoned or technically superseded.

## Non-negotiable safety and transparency

- No unreviewed blanket debloat; no Tiny11 Core-style breakage of security or servicing.
- No user data or boot/partition changes without an explicitly scoped, tested and approved operation.
- No changes to local installations or ISO were made as part of this documentation decision.
- Windows images come from legitimately obtained, user-supplied Microsoft media; do not distribute Microsoft's binaries through the source repository.
- `DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD` remains binding.
- **FX11 Quality > FX11 Uniformity**.

## Cross-project references

- [FX11 Legacy roadmap](../roadmap/FX11-LEGACY-ROADMAP.md)
- [Earlier FX OS family decision](2026-10-21-FX-OS-CROSS-PROJECT-DIRECTION.md)
- [Linux-native builder: Windows-first priority note](https://github.com/FenixMint/FX11-Builder/blob/main/docs/decisions/2026-10-21-WINDOWS-FIRST-SEQUENCING-NO-LOSS-OF-LINUX-WORK.md)

No tests, code modifications, packaging, VM runs or deployment are asserted by this documentation checkpoint.
