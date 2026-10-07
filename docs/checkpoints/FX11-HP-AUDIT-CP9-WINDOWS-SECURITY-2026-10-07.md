# FX11 HP audit checkpoint CP9 — Windows security baseline — 2026-10-07

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Observed state

### Microsoft Defender Antivirus

Reported by `Get-MpComputerStatus`:

- AntivirusEnabled: False
- RealTimeProtectionEnabled: False
- BehaviorMonitorEnabled: False
- IoavProtectionEnabled: False
- NISEnabled: False
- AntispywareEnabled: False
- IsTamperProtected: False

Defender service:

- WinDefend: Stopped
- StartType: Manual

This is a **security finding requiring explanation**, not an optimization target.

Do not enable/disable services yet. First determine whether another antivirus product is registered with Windows Security Center and whether Defender is intentionally in passive/disabled state.

### Windows Firewall

All three profiles report enabled:

- Domain: enabled
- Private: enabled
- Public: enabled

This is currently a positive baseline finding.

### Windows Update / transfer services

Observed:

- UsoSvc: Running / Automatic
- wuauserv: Running / Manual
- BITS: Stopped / Manual

BITS being stopped while idle is not by itself an error; it is demand-started.

## Current classification

- Windows Firewall: KEEP / PROTECTED
- Windows Update servicing: KEEP / PROTECTED
- Defender: REVIEW / SECURITY INVESTIGATION
- Tamper Protection: REVIEW
- BITS stopped/manual: no issue inferred from this signal alone

## Next read-only verification

Determine:

1. which antivirus products Windows Security Center currently knows about;
2. Defender preference state;
3. Windows Security Center service state.

No APPLY action until the Defender state is explained.
