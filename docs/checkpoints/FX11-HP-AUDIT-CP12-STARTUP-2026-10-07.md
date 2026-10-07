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
