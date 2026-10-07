# FX11 design history timeline

Status: HISTORICAL RECORD

This timeline preserves major project direction changes and reference decisions recovered from prior FX11 work.

## 2026-09-13 — FX11 identity and First Run direction

Major design ideas established:

- FX11 branding across system/installer;
- FX / FX11 logo direction;
- default wallpaper direction based on Windows XP "Lazur";
- "Idylla" rejected as the default wallpaper direction;
- Start button with FX identity;
- slogan concept: "FX11 OS for you";
- configurable taskbar location concept;
- selectable Start visual families inspired by XP / 7 / 10 / 11;
- normal Windows OOBE creates the user account — no fixed username baked into the image;
- FX11 First Run configures the profile after OOBE;
- optional application catalog suggests software rather than silently installing it;
- catalog ideas included LibreWolf, Firefox, DuckDuckGo Browser, Brave, Thorium, SRWare Iron, Thunderbird, 7-Zip, PeaZip, Driver Booster and VC++ Redistributables;
- privacy target: reduce Microsoft diagnostics/telemetry/error-reporting/profiling while preserving Windows Update, activation and Defender;
- privacy state should be rechecked after Windows updates.

At this stage, a stronger Linux builder was part of the wider architecture vision.

## 2026-10-03 — ThinkPad/Tiny11 operational cleanup checkpoint

The reference machine had evolved from Tiny11 into an FX11-style system.

Key retained decisions:

- battery charging 50/80;
- lid action "do nothing";
- selected Lenovo services retained;
- OneDrive retained;
- Office update retained;
- Defender/Firewall/Windows Update retained;
- Driver Booster manual-only;
- CEIP/Feedback tasks disabled;
- Recall payload removed;
- no blanket debloat;
- classify each item by Office/M365 dependency, hardware need, privacy, unnecessary state or test requirement.

Privacy Guard prototypes were created/tested.

A task-scheduling battery-condition problem was found and corrected.

## 2026-10-04 — Fenix System visual/runtime maturation

Fenix System direction became concrete:

- technical Conky-like panel;
- airy layout;
- thin separators;
- sparklines;
- monospace technical values;
- CPU/GPU/RAM/storage/thermal/battery/network;
- click-through;
- desktop integration;
- persistence with Win+D behavior;
- multi-monitor awareness;
- correct primary-monitor placement;
- WorkingArea-aware layout;
- top taskbar support;
- LibreHardwareMonitor as backend;
- readiness polling preferred over fixed startup delay.

The visual/runtime architecture also moved toward hardware adaptation rather than one fixed ThinkPad layout.

## 2026-10-07 — FX11 architecture formalized

Project direction was explicitly separated into three paths:

1. FX11 Native / Windows Builder;
2. FX11 Tiny Retrofit;
3. FX11 Windows Retrofit.

Key owner decision:

**FX11 Quality > FX11 Uniformity**

Not every starting system can or should reach the same exact component state.

Current builder phase:

- PowerShell/Windows-based;
- inspired by regular `tiny11maker.ps1`;
- not based on Tiny11 Core.

Future phase:

- stronger Linux/offline builder as a separate next-generation line.

Hardware architecture explicitly expanded to include motherboard-level ASRock detection for custom desktops.

Visual/UX was explicitly made independent of optimization:

- Native can bake visuals into the install;
- Retrofit can apply visuals separately;
- optimization does not require visual changes.

LibreHardwareMonitor licensing was reviewed and formally accepted with MPL-2.0 compliance conditions.

The new GitHub repository became the durable source for the Windows/PowerShell FX11 generation, including Builder, Retrofit, Agent, Fenix System and Visual Pack.
