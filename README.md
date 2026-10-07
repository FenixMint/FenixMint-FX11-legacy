# FenixMint FX11 Legacy

Windows/PowerShell generation of FX11, retrofit tooling for existing Tiny11 and Windows 11 installations, and the FX11 runtime agent/application stack.

## Main tracks

- **FX11 Native / Windows Builder** — builds an FX11 installation from official Windows 11 media using the Windows/PowerShell toolchain.
- **FX11 Tiny Retrofit** — audits and improves an existing Tiny11 installation without forcing it into an unsafe identical state.
- **FX11 Windows Retrofit** — audits and safely optimizes a normal Windows 11 installation.
- **FX11 Agent** — policy/state/privacy guards and drift detection.
- **Fenix System** — hardware-adaptive system monitor and UX layer.

Shared doctrine:

`DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD`

Quality, security and serviceability take priority over making every starting system identical.

## Hardware-aware design

Platform discovery and policy decisions are hardware-aware. Initial OEM/platform adapters include Lenovo, HP and ASRock, with Dell, ASUS and Generic/custom platforms in scope.

## LibreHardwareMonitor

FX11 Fenix System may use LibreHardwareMonitor as its telemetry backend. LibreHardwareMonitor is MPL-2.0 licensed and has additional third-party notices that must be preserved when redistributed.

See [docs/legal/LIBREHARDWAREMONITOR.md](docs/legal/LIBREHARDWAREMONITOR.md) for the integration and license policy.
