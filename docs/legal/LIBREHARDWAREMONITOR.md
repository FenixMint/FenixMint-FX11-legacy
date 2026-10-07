# LibreHardwareMonitor integration

Status: **APPROVED WITH CONDITIONS**

FX11 Legacy may use LibreHardwareMonitor (LHM) as the hardware telemetry backend for Fenix System, hardware discovery and selected runtime monitoring features.

## Upstream

- Project: LibreHardwareMonitor
- Repository: https://github.com/LibreHardwareMonitor/LibreHardwareMonitor
- License: Mozilla Public License 2.0 (MPL-2.0)
- Official library package: LibreHardwareMonitorLib
- Upstream README states that some sensors require administrator privileges.
- Upstream warns that it is not affiliated with librehardwaremonitor.com; FX11 documentation and tooling must use the GitHub project / official release channels, not that site.

## License compatibility

LibreHardwareMonitor is licensed under MPL-2.0.

FX11 Legacy is also licensed under MPL-2.0. This is a compatible licensing model for the planned integration.

Important MPL-2.0 obligations when FX11 distributes LHM source or executable form include:

1. Preserve license and copyright notices.
2. If Covered Software is distributed in executable form, make the corresponding LHM source available by reasonable means and tell recipients how to obtain it.
3. Modifications to MPL-covered source files remain under MPL-2.0.
4. FX11 code that is separate from LHM source files can remain a separate work; MPL-2.0 is file-level copyleft, not a requirement that every file in a larger work becomes LHM-derived code.
5. Do not imply endorsement by or affiliation with the LibreHardwareMonitor project.

This document is an engineering compliance note, not legal advice.

## Third-party notices

LibreHardwareMonitor's upstream repository contains `THIRD-PARTY-NOTICES.txt`.

At the time this integration decision was recorded, those notices include at least:

- Aga.Controls — BSD license.
- PawnIO.Modules — GNU Lesser General Public License v2.1.

If FX11 redistributes LHM binaries, packages, or dependencies, the corresponding upstream third-party notices and license texts must be preserved in the distribution.

Do not copy only the top-level MPL license and omit upstream third-party notices.

## FX11 integration policy

Preferred order:

### 1. External backend — preferred for the first implementation

Run the official LibreHardwareMonitor application as a separate backend and consume its local API / sensor output from FX11 Fenix System.

Advantages:

- strong separation between FX11 and the upstream program;
- straightforward upgrades of LHM;
- minimal patching of upstream code;
- simpler license compliance;
- the existing FX11 Fenix System prototype already follows this model.

Runtime design:

```
FX11 launcher
  -> start LibreHardwareMonitor
  -> poll local API
  -> API READY
  -> start Fenix System
```

No fixed sleep/delay should be used as the readiness mechanism.

### 2. LibreHardwareMonitorLib — allowed when justified

FX11 may later integrate `LibreHardwareMonitorLib` directly when deeper hardware discovery or lower-latency telemetry makes this worthwhile.

If this path is chosen:

- pin the upstream package version;
- record package/license metadata;
- preserve MPL notices;
- preserve all applicable third-party notices;
- document whether any upstream source files are modified;
- keep FX11-specific code in separate files/modules where practical.

### 3. Forking upstream code — avoid unless necessary

Do not fork or patch LibreHardwareMonitor merely to make integration easier.

If modifications become necessary:

- record the exact upstream commit/version;
- keep modified MPL-covered files under MPL-2.0;
- mark modifications clearly;
- keep the modified source available to recipients of redistributed binaries.

## Hardware scope

LHM is useful for runtime sensor data and supports hardware classes including:

- motherboards;
- Intel and AMD CPUs;
- NVIDIA, AMD and Intel GPUs;
- HDD / SSD / NVMe storage;
- network adapters.

FX11 hardware discovery must not rely on LHM alone.

Static identity and platform classification should also use Windows-native sources such as CIM/WMI, PnP and firmware information. This is required for hardware-aware policy decisions, including vendor adapters such as:

- Lenovo;
- HP;
- Dell;
- ASUS;
- ASRock;
- Generic / custom desktop.

For example, ASRock motherboard identity should come from platform/baseboard discovery; LHM can then augment that identity with available sensors.

## Security rules

- LHM is a telemetry source, not an authority for changing firmware or power policy.
- FX11 must not automatically apply tuning solely because a sensor or motherboard label was detected.
- Administrative elevation must be requested only when a sensor or operation actually requires it.
- Local telemetry endpoints should bind to loopback unless remote access is explicitly designed and secured.
- Any downloaded or bundled LHM build must come from the official GitHub project/release channel and should be integrity-checked.

## Distribution checklist

Before an FX11 release that bundles LibreHardwareMonitor:

- [ ] record exact LHM version / commit;
- [ ] include LHM MPL-2.0 license text;
- [ ] include upstream THIRD-PARTY-NOTICES;
- [ ] provide a source-code location for the exact LHM version distributed;
- [ ] document any FX11 modifications to LHM;
- [ ] verify bundled dependency licenses;
- [ ] verify the local telemetry/API binding configuration;
- [ ] confirm no unofficial LibreHardwareMonitor download source is used.

## Architectural decision

LibreHardwareMonitor is accepted as an FX11 Legacy dependency/backend under the rules above.

Initial implementation choice: **external official LHM backend + FX11-owned Fenix System frontend/consumer**.

Direct `LibreHardwareMonitorLib` integration remains an allowed future capability, not the default first step.
