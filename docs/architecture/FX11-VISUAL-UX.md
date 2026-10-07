# FX11 Visual / UX capability

Status: OWNER DECISION / HISTORICAL DESIGN + ACTIVE ARCHITECTURE

## Architectural rule

Visual changes are an **independent optional capability**.

They are not coupled to privacy/debloat/optimization.

### FX11 Native

Selected visual baseline may be integrated into the ISO/install/first-run path.

### Tiny / Windows Retrofit

Visual changes are an optional package that can be:

- applied independently;
- skipped entirely;
- changed later without re-running optimization.

Optimization must also work with the stock Windows appearance.

## Historical FX11 visual direction

Previously discussed/accepted concepts include:

- FX / FX11 branding throughout installer/system;
- default wallpaper based on Windows XP **Lazur**;
- "Idylla" was explicitly rejected as the default direction;
- Start button using FX branding;
- slogan concept: **FX11 OS for you**;
- taskbar placement configurable on supported implementations;
- historical preference for an XP-like taskbar placed at the top;
- selectable Start/menu visual families inspired by XP / 7 / 10 / 11;
- visual choices exposed through a future FX11 Control Center rather than irreversibly hard-coded.

These are preserved as design history. Implementation feasibility can differ by Windows build.

## Taskbar / WorkingArea lesson

On the tested Windows setup, a top taskbar resulted in a correct Windows WorkingArea offset.

Rule:

- respect Windows-reported WorkingArea;
- do not globally hack window coordinates because one application fails to respect the taskbar;
- application-specific layout bugs should not justify system-wide positioning hacks.

## Fenix System visual language

Accepted direction:

- lightweight;
- lots of visual air;
- technical / Conky-like;
- thin section header rules;
- monospace for technical data;
- subtle separators;
- sparklines;
- compact utilization bars;
- CPU/GPU/RAM/storage/thermal/battery/network sections;
- minimal uptime footer;
- optional small TOP CPU process view.

Historical v1.0 target included:

- CPU;
- GPU;
- RAM;
- SSD;
- Battery;
- Network;
- CPU/network charts;
- Battery Health;
- uptime;
- approximately 920 px height in the tested layout.

## Fenix System window behavior

Historical implemented/tested requirements:

- click-through;
- desktop-integrated behavior;
- should not disappear after Win+D;
- multi-monitor aware;
- position on current primary monitor;
- use `WindowStartupLocation=Manual`;
- correct final position after source/window initialization;
- honor `WorkingArea`, including a top taskbar.

## Native ISO visual path

For Native FX11, visual assets may be integrated at build/install time so the system is visually coherent from first boot.

Future Linux/offline builder may later provide stronger control over installer/boot visuals, but that is outside the immediate PowerShell-builder milestone.

## Retrofit visual package

Retrofit visual tooling should be versioned separately from optimizer policy.

Suggested conceptual CLI/API separation:

```
fx11 audit
fx11 plan
fx11 apply

fx11 visual audit
fx11 visual plan
fx11 visual apply
```

Exact command names are not yet binding.

## Historical installer/boot concept

Earlier design exploration included a clean FX11-branded boot/installer experience and an eventual menu capable of exposing installation/maintenance entries. Preserve the concept, but do not claim it is implemented in the Windows PowerShell builder yet.
