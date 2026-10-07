# FX11 Agent, Guards and Fenix System

Status: MIXED — historical implemented prototypes + active architecture

## Separation of responsibilities

### FX11 Agent

Runtime policy maintenance and drift detection.

### Privacy Guard

Privacy-specific self-healing rules.

### State Guard

Broader selected-state verification, including selected power/OEM/Office state.

### Fenix System

User-facing hardware/system monitor.

These may share discovery and logging, but they are not the same application.

## Historical Privacy Guard

A tested prototype evolved through early versions.

Historical responsibilities included:

- enforce Windows AI/Recall privacy policy;
- disable selected telemetry services;
- disable selected CEIP/Feedback tasks;
- local logging.

Known policy values used in the reference setup:

```
AllowRecallEnablement=0
DisableAIDataAnalysis=1
DisableClickToDo=1
```

Reference services:

- DiagTrack -> Disabled / Stopped
- WSAIFabricSvc -> Disabled / Stopped

Reference scheduled tasks included:

- CEIP Consolidator
- UsbCeip
- DmClient
- DmClientOnScenarioDownload

Historical log location:

`C:\ProgramData\FX11\PrivacyGuard.log`

A scheduling bug was discovered and fixed so the Guard was not blocked by battery task conditions:

- DisallowStartIfOnBatteries = False
- StopIfGoingOnBatteries = False
- StartWhenAvailable = True

This is an important design lesson for future Agent scheduling.

## Historical State Guard

State Guard v0.2 concept/reference checked items such as:

- selected Optional Features;
- selected scheduled tasks;
- Lenovo power state;
- charge thresholds 50/80;
- lid/battery policy;
- Privacy Guard presence;
- Office Minimal state.

Future implementation should move from procedural checks toward declarative policy.

## Guard policy

Guard must be idempotent.

Preferred behavior:

```
expected == current
  -> no action

expected != current
  -> record drift
  -> repair only if policy is ENFORCE
  -> otherwise report
```

Do not "repair Windows" continuously without an explicit accepted target.

## Fenix System

LibreHardwareMonitor is the approved initial telemetry backend.

Initial orchestration:

```
Start official LibreHardwareMonitor
  -> poll local API
  -> API READY
  -> start Fenix System
```

No fixed delay.

Reference local endpoint used by the prototype:

`http://127.0.0.1:8085/data.json`

LibreHardwareMonitor historical preferences:

- Start Minimized: ON
- Minimize To Tray: ON
- Run on Windows startup: OFF

FX11 owns the startup orchestration.

## Hardware adaptive requirement

The Fenix System implementation must adapt to available hardware rather than assume a fixed sensor list.

A second HP-oriented hardware-adaptive direction was already discussed/tested as the evolution beyond the original ThinkPad-specific prototype.

## Security

Local telemetry should remain loopback-bound unless a future secured remote design explicitly changes that.

Some sensors require elevation; do not elevate the whole FX11 stack unnecessarily when only a sensor backend needs it.

See `docs/legal/LIBREHARDWAREMONITOR.md`.
