# FenixMint FX11 Legacy

Windows/PowerShell generation of FX11, retrofit tooling for existing Tiny11 and Windows 11 installations, and the FX11 runtime agent/application stack.

## Project doctrine

`DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD`

**FX11 Quality > FX11 Uniformity**

FX11 is not a blanket debloat script. The target is a Windows 11 system that is light, private, secure, stable, serviceable and adapted to the actual hardware and role.

## Main tracks

- **FX11 Native / Windows Builder** — builds an FX11 installation from official Windows 11 media using the Windows/PowerShell toolchain.
- **FX11 Tiny Retrofit** — audits and improves an existing Tiny11 installation without forcing it into an unsafe identical state.
- **FX11 Windows Retrofit** — audits and safely optimizes a normal Windows 11 installation.
- **FX11 Agent** — policy/state/privacy guards and drift detection.
- **Fenix System** — hardware-adaptive system monitor and UX layer.
- **FX11 Visual / UX Pack** — independent appearance capability; baked into Native when selected, optional and independently applicable on Retrofit systems.

The later stronger Linux/offline builder remains a separate next-generation line.

## Hardware-aware design

Platform discovery and policy decisions are hardware-aware. Initial OEM/platform adapters include Lenovo, HP and ASRock, with Dell, ASUS and Generic/custom platforms in scope.

Static identity comes from Windows-native discovery such as CIM/WMI/PnP/firmware/baseboard data. LibreHardwareMonitor may augment runtime telemetry.

## Visual independence

Visual customization is not coupled to optimization.

- Native FX11 may integrate the chosen appearance into ISO/setup/first boot.
- Tiny/Windows Retrofit may apply visual changes without running optimization.
- Optimization may be used with completely stock Windows visuals.

Historical visual decisions and Fenix System UX are preserved in the architecture docs.

## Start here

Read in this order:

1. [AGENTS.md](AGENTS.md)
2. [docs/START-HERE.md](docs/START-HERE.md)
3. [System architecture](docs/architecture/FX11-SYSTEM-ARCHITECTURE.md)
4. [Variants and convergence](docs/architecture/FX11-VARIANTS-AND-CONVERGENCE.md)
5. [Hardware-adaptive architecture](docs/architecture/FX11-HARDWARE-ADAPTIVE.md)
6. [Visual / UX capability](docs/architecture/FX11-VISUAL-UX.md)
7. [Agent and Fenix System](docs/architecture/FX11-AGENT-AND-FENIX-SYSTEM.md)
8. [Knowledge preservation checkpoint CP1](docs/checkpoints/FX11-KNOWLEDGE-CP1-2026-10-07.md)
9. [Inspiration sources](docs/research/FX11-INSPIRATION-SOURCES.md)
10. [Roadmap](docs/roadmap/FX11-LEGACY-ROADMAP.md)

## LibreHardwareMonitor

FX11 Fenix System may use LibreHardwareMonitor as its telemetry backend.

LibreHardwareMonitor is MPL-2.0 licensed and has additional third-party notices that must be preserved when redistributed.

See [docs/legal/LIBREHARDWAREMONITOR.md](docs/legal/LIBREHARDWAREMONITOR.md).

## Historical preservation

Older decisions are not deleted simply because the implementation evolves.

Documentation uses explicit status terms such as:

- OWNER DECISION
- IMPLEMENTED
- TESTED
- ACCEPTED
- PLANNED
- OPEN
- SUPERSEDED

This keeps design history available without confusing old prototypes with current architecture.
