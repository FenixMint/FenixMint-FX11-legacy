# FX11 HP audit checkpoint CP13 — automatic third-party services — 2026-10-08

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Scope

Read-only inventory of services configured for automatic startup whose executable path is outside the Windows directory.

Observed:

- AnyDesk Service — Running / Auto
- ESET Firewall Helper — Running / Auto
- ESET Forwarder — Running / Auto
- ESET Service — Running / Auto
- HopToDesk Service — Running / Auto
- HP LaserJet Service — Running / Auto
- Microsoft Edge Update Service (edgeupdate) — Stopped / Auto
- Microsoft Office Click-to-Run Service — Running / Auto
- WSL Service — Running / Auto

## Classification

### KEEP / PROTECTED

- ESET Service / ESET Forwarder / ESET Firewall Helper  
  Active security stack verified in CP10. Do not disable while ESET is the selected antivirus/security product.

- Microsoft Office Click-to-Run Service  
  Required for the installed Microsoft 365 servicing/runtime model. Keep.

- WSL Service  
  Active dependency: FedoraLinux-44 is currently running under WSL2. Keep.

### KEEP / ROLE-DEPENDENT

- AnyDesk Service  
  Current remote-access tool. Keep while unattended remote access is required.

- HP LaserJet Service  
  Printer/scanner-related service. Keep if the HP LaserJet device/workflow remains in use; do not classify as PC OEM bloat.

### REVIEW / POSSIBLE REDUNDANCY

- HopToDesk Service  
  A second unattended remote-access service is running automatically alongside AnyDesk.

  This is not automatically wrong, but it is a meaningful review item because simultaneous remote-control agents:
  - enlarge the exposed software surface;
  - add another always-on service;
  - may be redundant if HopToDesk is only being tested.

  No change is authorized. FX11 should ask which remote-access product the owner wants to retain before any APPLY action.

- Microsoft Edge Update Service (edgeupdate) — Auto but currently stopped  
  Do not infer a fault from the stopped state alone. Update services may be trigger/demand driven. Keep under review until Edge/WebView2 dependency and servicing strategy are understood.

## Architectural lesson

Automatic-start service discovery should not be treated as a "disable list".

For every service, FX11 must combine:
- vendor/function identity;
- active user role;
- security role;
- hardware/software dependency;
- redundancy;
- current state.

Only then classify it as KEEP / OPTIONAL / CONDITIONAL / UNKNOWN.

## Current notable finding

The only clear redundancy candidate in this batch is the coexistence of **AnyDesk** and **HopToDesk** as automatic running remote-access services.

This remains a REVIEW item only.
