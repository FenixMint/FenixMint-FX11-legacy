# FX11 HP audit checkpoint CP12 — startup inventory — 2026-10-07

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Observed startup entries

- OneDriveSetup — two entries, first-setup command
- OneDrive — background client
- Microsoft.Lists — OneDrive Sync Service component
- AnyDesk — control/startup component
- SecurityHealth — Windows Security notification tray
- RtkAudUService — Realtek audio service
- egui — ESET Security user interface/runtime component

No usernames, SIDs, hostnames or user-specific profile paths are stored in this checkpoint.

## Current interpretation

### KEEP / PROTECTED

- SecurityHealth — Windows Security UI/notification component.
- RtkAudUService — hardware audio support; treat as driver-adjacent and protected unless a hardware-specific investigation proves otherwise.
- egui / ESET — part of the active third-party security stack already verified in CP10.

### KEEP / USER-ROLE DEPENDENT

- OneDrive — expected for a Microsoft 365 / OneDrive workflow; do not disable merely as startup optimization.
- AnyDesk — remote-access software. If deliberately used for unattended access, classify KEEP. If no longer used, review separately rather than silently disabling it.

### REVIEW

- Microsoft.Lists / OneDrive.Sync.Service — likely part of the installed Microsoft sync stack; verify role before any decision.
- duplicate OneDriveSetup startup registrations — audit registration source and whether they are stale first-run entries before any change.

## Important FX11 rule

Startup optimization must be role-aware.

Do not disable:
- security components;
- hardware driver/support components;
- active remote-access tooling;
- cloud/sync tooling the user actually depends on;

just because they appear at logon.

## Next read-only check

Inspect the exact startup registration sources for the duplicate OneDriveSetup entries and Microsoft.Lists without collecting usernames or SIDs.

No APPLY action.

## Registration-source verification

Observed registry startup sources:

### HKLM Run

Present among the audited entries:

- SecurityHealth
- RtkAudUService
- egui / ESET

Not present there:

- OneDriveSetup
- OneDrive
- Microsoft.Lists
- AnyDesk

### HKCU Run

Present:

- OneDrive
- Microsoft.Lists

Not present there:

- OneDriveSetup
- AnyDesk
- SecurityHealth
- RtkAudUService
- egui

### RunOnce

No matching OneDriveSetup or Microsoft.Lists entries were returned.

## Interpretation

- OneDrive and Microsoft.Lists are ordinary current-user Run registrations.
- SecurityHealth, Realtek audio support and ESET are machine-wide Run registrations.
- the duplicate OneDriveSetup entries reported by Win32_StartupCommand do **not** originate from the currently queried HKLM/HKCU Run or RunOnce keys.
- AnyDesk startup also does not originate from those currently queried Run keys.

Therefore the remaining source must be discovered rather than guessed. Possible sources include another startup-registration location, service registration, startup folders, scheduled mechanisms, or entries belonging to another local profile.

FX11 must not delete duplicate-looking startup entries until their registration source is known.

## Next read-only step

Query Win32_StartupCommand only for the ambiguous entries and sanitize any SID-like location tokens before displaying/persisting them.

## Ambiguous startup source resolved

Observed:

- OneDriveSetup entry #1: `HKU\S-1-5-19\...\Run`
- OneDriveSetup entry #2: `HKU\S-1-5-20\...\Run`
- Microsoft.Lists: current-user Run entry
- AnyDesk: Common Startup

Interpretation:

- `S-1-5-19` is the built-in **LOCAL SERVICE** account.
- `S-1-5-20` is the built-in **NETWORK SERVICE** account.
- therefore the two OneDriveSetup records are not duplicate entries for the interactive user; they belong to two Windows service profiles.
- Microsoft.Lists is a normal per-user startup registration.
- AnyDesk is intentionally launched from the machine-wide Common Startup folder.

## Corrected startup classification

- OneDriveSetup pair: NOT A USER DUPLICATE / no cleanup action
- Microsoft.Lists: KEEP / Microsoft 365 sync stack unless user chooses otherwise
- AnyDesk: KEEP while unattended remote access is desired
- SecurityHealth / Realtek / ESET: KEEP as previously classified

## Architecture lesson

FX11 must resolve startup ownership before labelling entries as duplicates.

Two identical commands can legitimately belong to different security principals or startup scopes. Duplicate-looking output from `Win32_StartupCommand` is insufficient evidence for cleanup.
