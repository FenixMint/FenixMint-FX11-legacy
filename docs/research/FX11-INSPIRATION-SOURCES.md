# FX11 inspiration sources and deliberate divergences

Status: RESEARCH / OWNER DECISION

This document records projects that inform FX11 Legacy. Inspiration is not wholesale adoption.

## 1. tiny11builder — regular tiny11maker.ps1

Upstream:
- https://github.com/ntdevlabs/tiny11builder
- regular script: `tiny11maker.ps1`

FX11 explicitly studies the **regular** builder, not Tiny11 Core.

### Useful patterns

The current regular script demonstrates a Windows/PowerShell offline image workflow using Microsoft servicing tools:

- copy official Windows media;
- convert ESD to WIM when needed;
- select image index;
- mount install.wim;
- inspect architecture/language;
- remove selected provisioned Appx;
- load offline registry hives;
- apply default-user and machine policy;
- modify boot.wim/setup policy;
- create an unattended path;
- export/compress the image;
- build an ISO with oscdimg.

That general engineering shape is relevant to the current FX11 Native Windows Builder phase.

### Surfaces worth auditing

The current script touches/removes/configures areas that FX11 should understand:

- Clipchamp;
- News / Weather;
- Xbox components;
- Get Help / Get Started;
- Office Hub;
- Solitaire;
- People;
- Power Automate;
- To Do;
- Feedback Hub;
- Maps;
- Phone Link;
- media apps;
- Quick Assist;
- Dev Home;
- new Outlook;
- Teams consumer/new Teams;
- Copilot;
- consumer content / sponsored apps;
- Advertising ID;
- tailored experiences;
- online speech/privacy/personalization;
- ContentDeliveryManager;
- telemetry policy;
- dmwappushservice;
- search-box suggestions;
- OOBE/local-account policy;
- Reserved Storage;
- setup hardware requirement bypasses;
- Compatibility Appraiser / CEIP / WER related tasks.

These are a **knowledge inventory**, not a default delete list.

### Deliberate FX11 divergences

Current `tiny11maker.ps1` also performs actions that FX11 must not blindly copy:

- removes Edge;
- removes Microsoft Edge WebView;
- removes OneDrive setup;
- blocks OneDrive sync;
- disables automatic Device Encryption via `PreventDeviceEncryption=1`;
- deletes scheduled-task definition files directly;
- uses `DISM /StartComponentCleanup /ResetBase`;
- applies unsupported-hardware bypasses universally.

FX11 policy:

- WebView2 is protected when application dependencies exist;
- OneDrive is retained in the historical Office profile;
- encryption is not disabled by default;
- supported Task Scheduler/policy paths are preferred to deleting protected task definitions;
- FX11 Legacy baseline does **not** use `/ResetBase`;
- unsupported-hardware bypass is a separate conditional capability, not a universal baseline.

### Why not Tiny11 Core

Tiny11 Core is explicitly designed as a more destructive development/testbed path and sacrifices serviceability.

FX11 Legacy targets regular use, security, Windows Update, Recovery and maintainability.

Therefore Tiny11 Core is **not** the model for FX11.

## 2. Destroy Windows Spying

Reference studied:
- Destroy Windows 10 Spying / reviewed fork lineage.

Useful as a historical map of privacy surfaces:

- telemetry/spying services;
- Metro/consumer applications;
- Office telemetry;
- host/firewall blocking;
- privacy-related system components.

FX11 does not inherit the old project's "remove/disable broadly" posture.

DWS is inspiration for **what to audit**, not authority for modern Windows 11 policy.

## 3. Existing FX11 field work

The strongest source is empirical work on real systems:

- Lenovo ThinkPad L13 Gen 2 AMD / Tiny11;
- HP stock-Windows test target;
- custom desktop direction including ASRock LiveMixer B650 and NVIDIA GeForce RTX 4060.

Observed dependencies and regressions are promoted into the FX11 Knowledge Base only after classification and verification.

## 4. LibreHardwareMonitor

Used as an approved hardware telemetry backend/source, subject to the license and distribution rules in:

`docs/legal/LIBREHARDWAREMONITOR.md`

It does not replace Windows-native hardware identity discovery.

## 5. General source rule

Every external project contributes one or more of:

- a surface to audit;
- an implementation technique;
- a warning/anti-pattern;
- a dependency lesson.

No external project defines FX11 policy by itself.
