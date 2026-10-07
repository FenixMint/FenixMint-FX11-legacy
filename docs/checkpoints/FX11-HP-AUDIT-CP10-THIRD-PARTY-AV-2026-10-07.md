# FX11 HP audit checkpoint CP10 — third-party antivirus context — 2026-10-07

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Windows Security Center registration

Observed antivirus registrations:

- Windows Defender
- ESET Security

Windows Security Center services:

- SecurityHealthService: Running / Manual
- wscsvc: Running / Automatic

## Defender behavior

Previous audit showed Microsoft Defender Antivirus protection components inactive and WinDefend stopped.

The current result explains that state much better: a third-party antivirus product, ESET Security, is registered with Windows Security Center.

`Get-MpPreference` returned HRESULT `0x800106ba`, consistent with the Microsoft Defender Antivirus service not being available/running in the current mode.

Do not treat the inactive Defender engine as a defect until the ESET runtime is verified.

## Current classification

- Windows Security Center: KEEP / healthy signal so far
- Windows Firewall: KEEP / PROTECTED
- ESET Security: ACTIVE-AV-CANDIDATE / VERIFY RUNTIME
- Microsoft Defender Antivirus engine: NOT CURRENTLY PRIMARY / REVIEW
- Defender Tamper Protection: not meaningful to judge in isolation while ESET is the likely primary AV

## Next read-only verification

Verify only:

- ESET service state;
- ESET runtime process presence;
- whether Defender is explicitly in passive/disabled mode where readable.

No APPLY changes.
