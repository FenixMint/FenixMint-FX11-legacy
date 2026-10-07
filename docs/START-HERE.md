# FX11 Legacy — START HERE

## What this repository is

FX11 Legacy is the Windows/PowerShell generation of FX11.

It contains one shared platform for:

1. building an FX11 Windows installation from official Windows 11 media;
2. improving an existing Tiny11 installation;
3. improving an existing stock Windows 11 installation;
4. maintaining accepted policy through FX11 Agent / Guards;
5. running FX11 applications such as Fenix System;
6. applying the FX11 visual/UX capability independently where desired.

This repository intentionally keeps the Windows-native generation together so the Builder, Retrofit paths, Agent, Fenix System and Visual Pack can share one knowledge base and one policy model.

The later, stronger Linux/offline image builder is a separate next-generation line.

## Read order

1. [AGENTS.md](../AGENTS.md)
2. [System architecture](architecture/FX11-SYSTEM-ARCHITECTURE.md)
3. [Variants and convergence](architecture/FX11-VARIANTS-AND-CONVERGENCE.md)
4. [Hardware-adaptive architecture](architecture/FX11-HARDWARE-ADAPTIVE.md)
5. [Visual and UX capability](architecture/FX11-VISUAL-UX.md)
6. [Agent and Fenix System](architecture/FX11-AGENT-AND-FENIX-SYSTEM.md)
7. [Inspiration sources](research/FX11-INSPIRATION-SOURCES.md)
8. [Knowledge preservation checkpoint](checkpoints/FX11-KNOWLEDGE-CP1-2026-10-07.md)
9. [Design history timeline](history/FX11-DESIGN-TIMELINE.md)
10. [Roadmap](roadmap/FX11-LEGACY-ROADMAP.md)
11. [LibreHardwareMonitor licensing/integration](legal/LIBREHARDWAREMONITOR.md)

## Core doctrine

```
DISCOVER
  -> AUDIT
  -> CLASSIFY
  -> PLAN
  -> APPROVE
  -> APPLY
  -> VERIFY
  -> GUARD
```

FX11 is not a process-count contest and not an aggressive debloat challenge.

The desired result is:

**light + private + secure + stable + serviceable**

within the safe limits of the actual starting system and hardware.

## Three variants

### FX11 Native / Windows Builder

Best-quality path.

Some decisions are applied while preparing official Windows 11 media, some during setup/first boot, and some by the runtime agent.

Current implementation direction: PowerShell / Windows servicing, inspired technically by `tiny11maker.ps1`, but not by Tiny11 Core's destructive servicing model.

### FX11 Tiny Retrofit

For an existing Tiny11 installation.

It audits what Tiny11 already removed or changed, preserves good state, repairs missing required capabilities only when justified, and applies FX11 policy without forcing identity with Native.

The ThinkPad L13 Gen 2 AMD installation is the principal historical reference case.

### FX11 Windows Retrofit

For ordinary Windows 11 installations.

This path is deliberately the most conservative because installed Appx, per-user state, OEM software, dependencies and servicing history already exist.

The HP system is the first intended broad READ-ONLY audit/reference for this path.

## Visual capability is independent

The FX11 visual/UX layer is a separate selectable capability:

- baked into FX11 Native when desired;
- optional in Tiny/Windows Retrofit;
- may be applied without system optimization;
- optimization may be applied without changing appearance.

## Hardware-aware design

Policies depend on discovered facts, not vendor-name heuristics.

The architecture explicitly includes motherboard-level detection for custom systems such as ASRock boards, not only whole-system OEMs.

## Historical reference

The detailed checkpoint preserves earlier ThinkPad/Tiny11 work, privacy/security decisions, Office trimming, Lenovo power policy, State/Privacy Guard, Fenix System, visual concepts, EFI lessons and open items.

The design timeline preserves the chronology of major decisions.

Do not delete historical decisions merely because implementation later changes. Mark them SUPERSEDED when needed.
