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

## Runtime verification

Observed:

- ESET Forwarder: Running / Automatic
- ESET Service (`ekrn`): Running / Automatic
- ESET Firewall Helper: Running / Automatic
- ESET HTTP Server: Stopped / Manual
- ESET kernel/service process: present
- Defender registry:
  - `PassiveMode = 0`
  - `DisableAntiSpyware = 1`

Interpretation:

ESET is actively running and is the effective third-party security product on this system.

The Microsoft Defender Antivirus engine being inactive is therefore expected in this configuration. The registry state shows Defender antivirus explicitly disabled rather than merely passively coexisting.

FX11 must not "repair" Defender by force while ESET remains the chosen antivirus. That could create product conflicts or an unnecessary dual-AV configuration.

Current classification:

- ESET Security: KEEP / ACTIVE SECURITY DEPENDENCY
- Windows Security Center: KEEP / PROTECTED
- Windows Firewall: KEEP / PROTECTED
- Microsoft Defender Antivirus engine: NOT_APPLICABLE as primary AV while ESET is installed and active
- Defender remediation: DO NOT APPLY unless ESET is intentionally removed or its registration/runtime becomes unhealthy

The stopped manual ESET HTTP Server is not treated as a fault from this signal alone.
