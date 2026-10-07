# FX11 knowledge preservation checkpoint CP1 — 2026-10-07

Status: KNOWLEDGE PRESERVATION / MIXED IMPLEMENTED-TESTED-PLANNED

Purpose: preserve the project's known decisions and field experience before new implementation begins in this repository.

This is not a claim that every historical idea is still implemented on every machine.

---

## 1. Project direction

OWNER DECISION:

FX11 evolved from "modify Tiny11" into a distinct Windows 11 system/design philosophy inspired by Tiny11 but governed by FX11 policy.

Current principles:

- light;
- private;
- secure;
- stable;
- serviceable;
- hardware-aware.

Primary workflow:

```
DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD
```

Historical shorter rule:

```
AUDIT -> APPLY -> VERIFY -> POLICY
```

No random debloat scripts.

No blind service/task/package deletion.

One controlled change at a time on live systems.

---

## 2. Three variants

OWNER DECISION:

### FX11 Native / Windows Builder

Best-quality path.

Current builder generation is Windows/PowerShell based, technically inspired by regular `tiny11maker.ps1`.

Some changes happen:

- in ISO/image;
- during setup;
- on First Run;
- later through Agent/Guard.

### FX11 Tiny Retrofit

For existing Tiny11.

Reference machine: Lenovo ThinkPad L13 Gen 2 AMD.

Do not force Tiny Retrofit to become byte-identical to Native.

### FX11 Windows Retrofit

For stock Windows 11.

More conservative.

HP is the first intended broad audit target.

### Global rule

**FX11 Quality > FX11 Uniformity**

Valid results include PARTIAL, NOT_SAFE_TO_CHANGE and UNKNOWN.

---

## 3. ThinkPad reference hardware

HISTORICAL REFERENCE / TESTED ENVIRONMENT:

- Lenovo ThinkPad L13 Gen 2 AMD;
- AMD Ryzen 5 PRO 5650U;
- 16 GB RAM;
- NVMe 256 GB;
- dual boot with Fedora;
- Windows mainly for Excel/Microsoft 365 and Windows-only business software;
- Windows installation started as Tiny11 and then received FX11 refinements.

This machine is a reference case, not a universal template.

---

## 4. Preserve functionality

OWNER DECISION / ACCEPTED:

Keep functional unless a profile explicitly changes the decision:

- Windows Update;
- Defender;
- Firewall;
- activation;
- Recovery;
- PowerShell;
- .NET;
- WebView2;
- printing;
- Excel;
- Word;
- Outlook;
- OneDrive;
- Teams when selected;
- required drivers/runtime components.

Do not remove Edge/WebView runtime blindly when dependencies exist.

Do not disable security just to reduce process count.

---

## 5. Privacy / telemetry reference state

IMPLEMENTED/TESTED on reference system according to project history:

Services:

- `DiagTrack` -> Disabled + Stopped;
- `WSAIFabricSvc` -> Disabled + Stopped.

Selected CEIP tasks disabled, including:

- Consolidator;
- UsbCeip.

Selected Feedback/SIUF tasks disabled:

- DmClient;
- DmClientOnScenarioDownload.

Recall:

- removed as Optional Feature with payload removed;
- no installed Recall/Copilot/WindowsAI/ClickToDo packages in the reference state.

Windows AI policy values used:

- `AllowRecallEnablement=0`;
- `DisableAIDataAnalysis=1`;
- `DisableClickToDo=1`.

Important rule:

Protected AI/system tasks were not taken over or hacked through TaskCache merely to force disablement. Policy is preferred when Windows protects a task.

---

## 6. Privacy Guard

HISTORICAL EVOLUTION:

Early Privacy Guard v0.1 was built/tested and revealed scheduled-task battery-condition issues.

Fixes included:

- `DisallowStartIfOnBatteries=False`;
- `StopIfGoingOnBatteries=False`;
- `StartWhenAvailable=True`.

The consolidated later project summary identifies **FX11 Privacy Guard v0.3** as the current historical baseline concept:

- runs as SYSTEM;
- startup/logon/periodic execution;
- verifies privacy policy;
- verifies telemetry service state;
- verifies selected tasks;
- writes local log.

Historical log location:

`C:\ProgramData\FX11\PrivacyGuard.log`

Future implementation should be declarative/idempotent.

---

## 7. State Guard

HISTORICAL IMPLEMENTATION/CONCEPT:

FX11 State Guard v0.2 checked selected state such as:

- disabled Optional Features;
- selected tasks;
- Lenovo power state;
- charge threshold 50/80;
- lid/battery policy;
- Privacy Guard presence;
- Office Minimal state.

Future Agent should absorb this logic as reusable policy rules rather than one-off checks.

---

## 8. Optional Features / system components

REFERENCE CHANGES:

Disabled on the tested setup where not needed:

- WorkFolders-Client;
- SmbDirect;
- WCF-TCP-PortSharing45;
- Microsoft-RemoteDesktopConnection;
- MSRDC-Infrastructure.

Incoming RDP disabled:

- `fDenyTSConnections=1`.

Do not mechanically reproduce Linux service-disable lists in Windows.

Guest/virtualization software such as Spice/QEMU/VMware/VirtualBox guest packages was removable only when unused.

Do not disable printing, Windows Search, Defender or Windows Update without a specific profile reason.

---

## 9. Microsoft 365 / Office reference profile

IMPLEMENTED/TESTED DIRECTION:

Microsoft 365 Business x64, Current channel, pl-PL.

Keep:

- Excel;
- Word;
- Outlook.

Excluded in the minimal Office profile:

- Access;
- PowerPoint;
- Publisher;
- OneNote.

Teams remains separate.

OneDrive remains functional.

ODT historical location:

`C:\ODT\setup.exe`

To reduce unwanted "Files" window behavior, selected Office tasks were disabled:

- Office Actions Server (`ActionsServer.exe availabilitycheck`);
- Office Startup Maintenance (`ActionsServer.exe wacheck`).

Office Automatic Updates and core Office update mechanisms remain enabled.

---

## 10. Startup / scheduled tasks reference cleanup

Disabled when identified as unnecessary:

- Firefox Default Browser Agent;
- Driver Booster Scheduler;
- Driver Booster SkipUAC;
- Driver Booster Update;
- Lenovo SmartStandby Daily analysis;
- Lenovo Power Manager Uninstall;
- selected telemetry/CEIP/Feedback tasks.

Keep:

- OneDrive;
- Windows Security;
- Defender;
- Office Update;
- audio driver components;
- required Lenovo power components.

Driver Booster historical rule:

- optional/manual tool only;
- not a required silent background updater.

Do not bulk-disable all scheduled tasks.

---

## 11. Lenovo OEM reference decisions

Lenovo Vantage is not required for the historical target.

Kept services:

- IBMPMSVC;
- LITSSVC;
- TPHKLOAD.

Disabled on the tested machine:

- LenovoBrightCtrl;
- LenovoSmartStandby.

Battery charging policy without Vantage:

Registry:

`HKLM\SOFTWARE\WOW6432Node\Lenovo\PWRMGRV\Data`

Values:

- `ChargeStartControl=1`;
- `ChargeStopControl=1`;
- `ChargeStartPercentage=50`;
- `ChargeStopPercentage=80`.

Target policy: 50/80.

This is model/context-specific, not a universal Lenovo action.

---

## 12. Laptop power reference

ThinkPad reference policy:

Lid AC/DC:
- Do nothing.

Low battery:
- 30%;
- notification ON;
- action NONE.

Critical battery:
- 20%;
- notification ON;
- action SLEEP.

Reserve:
- 4%.

Hardware uses Modern Standby S0 + Hibernate; no classic S3.

Rule:
- never force S3 merely because another laptop supports it;
- no aggressive Windows "power optimizer" equivalent to TLP.

---

## 13. 24x7 workstation pattern

NEWER ACCEPTED REFERENCE PATTERN from office workstation work:

When profile is explicitly `workstation-24x7` and AC-powered:

- sleep AC = never;
- hibernate AC = never / hibernation may be disabled for that role;
- disk idle AC = never;
- USB selective suspend AC may be disabled;
- Wi-Fi AC = maximum performance;
- display remains user choice;
- machine should not auto-shutdown/restart unexpectedly.

Windows Update policy used on the office workstation:

- `NoAutoUpdate=1`;
- `NoAutoRebootWithLoggedOnUsers=1`.

This is a role/profile decision, not a universal laptop baseline.

A real controlled restart/shutdown investigation confirmed why long-running FX work needs explicit restart policy and checkpoint/resume behavior.

---

## 14. Defender / security reference

KEEP Defender active.

Verified historical state included:

- AntivirusEnabled=True;
- RealTimeProtection=True;
- BehaviorMonitor=True;
- IoavProtection=True;
- Antispyware=True;
- NIS=True;
- TamperProtection=True.

PUA protection enabled.

Controlled Folder Access:
- AuditMode, not aggressive blocking.

OPEN / REVERIFY BEFORE FREEZE:

- Network Protection final state;
- `SubmitSamplesConsent` final state.

simplewall:

- installed/considered as optional open-source firewall control layer;
- full policy not frozen.

Security always wins over process-count optimization.

---

## 15. Component store / cleanup

ACCEPTED:

Safe cleanup used:

`DISM.exe /Online /Cleanup-Image /StartComponentCleanup`

Historical result: approximately 4 GB recovered.

FORBIDDEN BASELINE:

Do not use `/ResetBase` in FX11 Legacy live-system cleanup because it harms update rollback/serviceability.

This is also a deliberate divergence from current regular `tiny11maker.ps1`, which uses ResetBase during image creation.

---

## 16. EFI / dual boot

HISTORICAL INCIDENT + ACCEPTED RULE:

Fedora has its own EFI System Partition (FAT32).

Windows accidentally assigned that ESP drive letter `D:`.

The drive letter was removed using `Remove-PartitionAccessPath`.

ESP remained:

- GPT type `c12a7328-f81f-11d2-ba4b-00a0c93ec93b`;
- Hidden: Yes;
- no drive letter.

Rule:

- detect foreign/additional ESPs;
- do not assign letters;
- do not modify their content during optimization;
- audit partitions/EFI read-only unless a dedicated boot task explicitly authorizes a change.

---

## 17. Fenix System / LibreHardwareMonitor

HISTORICAL IMPLEMENTED/TESTED DIRECTION:

LibreHardwareMonitor backend.

Reference API:

`http://127.0.0.1:8085/data.json`

LHM preferences:

- Start Minimized ON;
- Minimize To Tray ON;
- Run on Windows startup OFF.

FX11 controls startup.

Required orchestration:

```
Start LHM
-> poll API 8085
-> API READY
-> Start Fenix System
```

No fixed sleeps.

Fenix System reference functions:

- CPU;
- GPU;
- RAM;
- SSD;
- thermal;
- battery;
- network;
- charts;
- Battery Health;
- uptime.

Window behavior:

- click-through;
- desktop integration;
- should survive Win+D behavior as designed;
- multi-monitor aware;
- current primary monitor;
- respects WorkingArea;
- handles top taskbar.

Visual direction:

- light/airy;
- technical;
- Conky-like;
- thin section rules;
- monospace technical data;
- subtle separators;
- sparklines;
- compact utilization bars;
- optional TOP CPU process area.

A hardware-adaptive direction was already tested/discussed beyond the original ThinkPad, including HP.

LibreHardwareMonitor licensing/integration is now formally documented in this repository.

---

## 18. Visual FX11 history

OWNER PREFERENCE / DESIGN HISTORY:

FX11 visual identity should be available independently of optimization.

For Native:
- visual baseline may be baked into ISO/install.

For Retrofit:
- visual pack is optional and independently applicable.

Historical concepts:

- FX/FX11 branding;
- default wallpaper direction based on Windows XP "Lazur";
- "Idylla" rejected;
- Start button with FX identity;
- slogan concept: `FX11 OS for you`;
- taskbar position historically desired to support bottom/top/left/right where technically feasible;
- top taskbar preference used/tested;
- selectable Start styles inspired by XP / 7 / 10 / 11;
- future FX11 Control Center can expose choices.

On tested system:

- display 1680x1050;
- top taskbar;
- WorkingArea Y=48.

Rule:
- respect WorkingArea;
- do not create global positioning hacks because one app (e.g. Snipping Tool) misbehaves.

---

## 19. OOBE / First Run / users

OWNER DECISION:

FX11 must not bake a fixed username/user account into ISO.

Normal Windows OOBE creates the account.

FX11 First Run then configures the selected FX11 profile.

Optional apps are suggested, not silently installed.

Historical application catalog concept included:

Browsers:
- LibreWolf;
- Firefox;
- DuckDuckGo Browser;
- Brave;
- Thorium;
- SRWare Iron.

Other:
- Thunderbird;
- 7-Zip;
- PeaZip;
- Driver Booster (optional/manual-only);
- Visual C++ Redistributable x86/x64.

This is historical catalog design, not a permanent mandatory bundle.

The catalog was intended to remain available later through a future FX11 Control Center.

---

## 20. Tiny11 inspiration boundary

OWNER DECISION:

Use regular `tiny11maker.ps1` as a major implementation reference for the current Windows/PowerShell Native Builder.

Do **not** model FX11 on Tiny11 Core.

Tiny11maker surfaces are useful, but current upstream actions such as:

- Edge/WebView removal;
- OneDrive removal;
- Device Encryption prevention;
- direct task-file deletion;
- ResetBase;

must be independently evaluated and are not automatic FX11 policy.

See `docs/research/FX11-INSPIRATION-SOURCES.md`.

---

## 21. Hardware vendors/platforms

OWNER DECISION:

Initial explicit hardware architecture includes:

- Lenovo;
- HP;
- Dell;
- ASUS;
- ASRock;
- Generic/custom desktop.

ASRock must be detected at motherboard/baseboard level, not only whole-system OEM.

Known custom desktop reference hardware discussed:

- ASRock LiveMixer B650;
- NVIDIA GeForce RTX 4060;
- AMD AM5 platform context.

Hardware policy must use discovery and dependencies.

---

## 22. Repository split / generation boundary

CURRENT DECISION:

`FenixMint/FenixMint-FX11-legacy` contains the Windows/PowerShell generation:

- Native Builder on Windows;
- Tiny Retrofit;
- Windows Retrofit;
- Agent/Guards;
- Fenix System;
- Visual pack;
- shared Knowledge Base / policy.

The stronger Linux/offline builder is a later generation and should remain a separate builder line.

The future builder should reuse/export FX11 policy knowledge rather than repeat all historical discovery from scratch.

---

## 23. Open items that must not be forgotten

OPEN:

- build Discovery/Audit v0.1 READ-ONLY;
- run first broad stock Windows audit on HP;
- formalize declarative rule schema;
- dependency graph for Appx/runtimes/Office/OEM;
- freeze Defender Network Protection policy after verification;
- freeze SubmitSamplesConsent after verification;
- decide exact simplewall integration level;
- extract/migrate existing Privacy Guard/State Guard code into repo;
- extract/migrate Fenix System code into repo;
- define visual pack package boundaries;
- design First Run / Control Center;
- implement safe Appx handling for existing users vs provisioned packages;
- define unsupported-hardware bypass as optional/conditional;
- create build manifest and reproducibility logs;
- add tests for hardware classifiers;
- add ASRock adapter tests;
- add HP audit fixture/reference;
- preserve exact upstream dependency versions/licenses for bundled components.

---

## 24. Non-negotiable summary

- no blanket debloat;
- no "OEM == bloat";
- no forced identical result across variants;
- no sacrificing security/serviceability for process count;
- no unknown component removal;
- no fixed user baked into ISO;
- no silent optional app installation;
- no ResetBase in live FX11 cleanup;
- visuals independent from optimization in Retrofit;
- Native may integrate visuals into install;
- audit before apply;
- verify after every material change;
- guards are explicit/idempotent;
- preserve history and mark superseded decisions instead of deleting them.
