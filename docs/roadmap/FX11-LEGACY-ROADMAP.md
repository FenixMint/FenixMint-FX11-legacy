# FX11 Legacy roadmap

Status: ACTIVE ROADMAP — 2026-10-07

## Phase 0 — preserve knowledge

Status: IN PROGRESS / ACCEPTED

- capture architecture;
- capture three variants;
- capture ThinkPad/Tiny11 reference state;
- capture visual/UX history;
- capture Agent/Guard/Fenix System decisions;
- capture LibreHardwareMonitor licensing;
- capture Tiny11/DWS inspiration and divergences;
- preserve future Linux-builder boundary.

## Phase 1 — Discovery / Audit v0.1

Status: NEXT

Goal: 100% READ-ONLY first pass.

First broad test target: HP stock Windows 11.

Audit:

- system OEM/model;
- baseboard/vendor/model;
- BIOS/UEFI;
- CPU/GPU/RAM;
- storage/NVMe;
- battery/laptop/desktop;
- network/audio/Bluetooth/camera;
- power capabilities;
- TPM/Secure Boot;
- WSL/Hyper-V;
- services;
- scheduled tasks;
- startup;
- Appx/MSIX;
- provisioned Appx;
- Optional Features;
- Windows capabilities;
- Defender;
- Firewall;
- Windows Update policy;
- privacy/AI policy;
- Office/OneDrive/Teams;
- OEM software;
- EFI/partitions READ-ONLY;
- LHM availability/sensor inventory when present.

Output should classify but perform no changes.

## Phase 2 — Knowledge Base / Planner

Build declarative rules and dependency-aware classifications.

Output:

- SAFE;
- RECOMMENDED;
- CONDITIONAL;
- OPTIONAL;
- KEEP;
- PROTECTED;
- UNKNOWN;
- NOT_SAFE_TO_CHANGE.

Generate a plan independent of apply.

## Phase 3 — Windows Retrofit controlled apply

One change at a time:

```
BEFORE
-> APPLY
-> VERIFY
-> PASS / FAIL
-> ROLLBACK
-> HISTORY
```

No giant debloat script.

## Phase 4 — Tiny Retrofit

Use the same rules against existing Tiny11.

Reference target: ThinkPad L13 Gen 2 AMD.

Detect what is already absent/present and do not reconstruct packages without a profile requirement.

## Phase 5 — FX11 Native Builder on Windows / PowerShell

Use official Windows 11 media.

Technical inspiration: regular `tiny11maker.ps1`.

Planned responsibilities:

- input ISO/WIM audit;
- edition/index selection;
- serviceability checks;
- offline Appx decisions;
- offline policy;
- unattended/first-boot setup;
- optional visual integration;
- First Run;
- install Agent;
- final build manifest;
- reproducible logs;
- verify final image.

No Tiny11 Core-style serviceability destruction.

## Phase 6 — Agent / Guard

Unify Privacy Guard + State Guard patterns behind declarative policy.

Modes:

- REPORT;
- ENFORCE selected accepted policy;
- drift history.

## Phase 7 — Fenix System

Package/refactor existing monitor into the repo.

Initial backend: official LibreHardwareMonitor external process/API.

Required:

- readiness polling;
- adaptive sensor mapping;
- multi-monitor;
- WorkingArea;
- click-through;
- Win+D persistence;
- graceful missing-sensor handling.

## Phase 8 — Visual Pack

Keep visual changes independently installable on retrofit systems.

Native builder can bake selected visual baseline into installation.

Future Control Center may expose appearance/start/taskbar choices.

## Later generation — Linux/offline builder

Out of immediate scope for FX11 Legacy.

The stronger Linux-based builder belongs to the next-generation builder line and should reuse exported FX11 knowledge/policy where practical.
