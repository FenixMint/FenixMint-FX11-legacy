# FX11 HP audit checkpoint CP11 — OEM/printer software and detector false positives — 2026-10-07

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Installed HP-related applications observed

The application inventory is dominated by HP LaserJet / scanning software rather than PC-management utilities:

- HP LaserJet Pro MFP M426-M427
- HP LJ M426M427 Scan / HP Scan
- HP Product FWUpdater
- HP Unified IO
- HPDXP
- HPLJProMFPM426M427
- hppLaserJetService
- hppM31426427LaserJetService
- I.R.I.S. OCR
- LJDXPHelperUI

Interpretation:

These items appear primarily tied to printer/scanner functionality. FX11 must not classify them as generic OEM bloat merely because the publisher is HP.

## Services observed

Likely HP platform / support services:

- HP App Helper HSA Service — stopped/manual
- HP Diagnostics HSA Service — stopped/manual
- HP Network HSA Service — stopped/manual
- HP System Info HSA Service — stopped/manual
- HP DSU LAN/WLAN/WWAN Switching Service — running/automatic
- HP DSU Service — running/automatic
- HP Insights Analytics — stopped/disabled
- HP Print Scan Doctor Service — stopped/manual
- HP Services Scan — stopped/manual
- HP LaserJet Service — running/automatic

Current interpretation:

- HP Insights Analytics is already disabled; no action needed.
- printer/scanner services should be treated as KEEP until printer use is established.
- manual/stopped HSA capability services are low-priority review items, not an immediate optimization target.
- DSU switching/hotkey services require hardware-role verification before classification.

## Detector false positives discovered

The first HP audit filter used a loose case-insensitive substring match on `HP`.

That produced false positives:

- `hpatchmon` / Hotpatch Monitoring Service was selected because its service name begins with `hp`, but that does not establish HP ownership.
- `shpamsvc` / Shared PC Account Manager was selected because its service name contains `hp`, but that does not establish HP ownership.
- scheduled tasks `RemoteTouchpadSyncDataAvailable` and `TouchpadSyncDataAvailable` were selected because the word `Touchpad` contains the letters `hp` consecutively.

Therefore the current regex is unsuitable for production FX11 OEM detection.

## FX11 discovery rule correction

OEM detection must not use a bare substring such as:

```
-match 'HP|Hewlett|Wolf'
```

Preferred evidence hierarchy:

1. exact/anchored vendor prefixes such as `^HP(?:$|[ _-])`;
2. publisher/vendor metadata;
3. executable/company metadata where needed;
4. known HP service/package identifiers;
5. model-specific adapter rules.

Unknown or ambiguous matches remain UNKNOWN and must not be modified.

## Current classification

- HP printer/scanner stack: KEEP / REVIEW BY FUNCTION
- HP Touchpoint Analytics: already disabled
- HSA capability services: OPTIONAL-CANDIDATE / REVIEW, but no change now
- HP DSU services: CONDITIONAL / verify hardware role
- false-positive Windows services/tasks: NOT HP; exclude from OEM classifier

## Architecture lesson

This audit found a real classifier defect before any APPLY phase. That is exactly the intended value of read-only validation on real systems.
