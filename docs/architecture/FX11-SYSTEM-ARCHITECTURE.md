# FX11 Legacy system architecture

Status: OWNER DECISION / ACTIVE ARCHITECTURE

## Purpose

FX11 Legacy is a hardware-aware Windows 11 build, retrofit and policy platform.

It is not defined by one ISO, one debloat list or one laptop. It is defined by reusable rules, evidence, dependency awareness and verification.

## Platform layers

```
FX11 CORE
├── Discovery
├── Audit
├── Classification
├── Knowledge Base
├── Policy / Target State
├── Planner
├── Apply
├── Verify
├── Rollback
├── Checkpoint / History
└── Guard / Drift Detection

EXECUTION PATHS
├── Native Builder (Windows/PowerShell)
├── Tiny Retrofit
└── Windows 11 Retrofit

HARDWARE
├── Lenovo
├── HP
├── Dell
├── ASUS
├── ASRock
└── Generic / Custom

RUNTIME
├── FX11 Agent
├── Privacy Guard
├── State Guard
└── Fenix System

OPTIONAL CAPABILITIES
├── Visual / UX pack
├── Office profile
├── laptop power profile
├── 24x7 workstation profile
└── application catalog / First Run
```

## Declarative rules

Preferred rule model:

```text
id
description
scope
variant support
hardware prerequisites
software dependencies
risk class
current-state probe
desired state
apply
verify
rollback
guard behavior
source / rationale
last verified Windows build
```

The same rule may have a different execution strategy by variant.

Example:

```
Consumer component policy

Native:
  remove/not provision offline when safe

Tiny Retrofit:
  verify; if already absent => COMPLIANT

Windows Retrofit:
  remove only when safe for current users/dependencies

Agent:
  detect drift / reinstall and report or remediate according to policy
```

## Classification model

A detected component may be classified as:

- PROTECTED
- KEEP
- SAFE
- RECOMMENDED
- CONDITIONAL
- OPTIONAL
- UNKNOWN

A capability result may be:

- COMPLIANT
- PARTIAL
- NOT_APPLICABLE
- NOT_SAFE_TO_CHANGE
- UNAVAILABLE
- UNKNOWN

UNKNOWN never means "probably bloat".

## Protected baseline

Default protection includes servicing/security/runtime elements required by the selected profile:

- Windows Update and servicing stack
- Defender
- Firewall
- activation
- Recovery / WinRE
- PowerShell
- .NET
- VC++ runtimes
- Windows Installer
- App Installer
- Store infrastructure
- WebView2 where dependencies exist
- Office/M365 dependencies
- printing where needed
- required OEM / hardware components

## Checkpoints

Before material live-system changes, capture an audit/checkpoint.

Checkpoint is evidence and rollback support, not a claim that every Windows change can be automatically restored.

## Policy vs implementation

Policy states what should be true.

Execution strategy decides how to reach that state safely for:

- offline image;
- first boot;
- existing Tiny11;
- existing stock Windows;
- ongoing agent enforcement.

This separation prevents duplicate and diverging logic.

## Security doctrine

FX11 optimizes around security, not through it.

Examples:

- do not disable Defender to reduce processes;
- do not disable Windows Update to prevent activity;
- do not disable device encryption by default;
- do not remove Recovery to save disk space;
- do not remove WebView2 blindly;
- do not delete protected scheduled-task definitions if supported policy is sufficient.

## Privacy doctrine

Reduce:

- telemetry;
- diagnostics beyond the chosen minimum;
- feedback prompts;
- consumer experiences;
- advertising identifiers;
- profiling/personalization not requested;
- unwanted Recall / Windows AI / Copilot extras.

But privacy changes must preserve update, activation, security and selected application functionality.

## Application profiles

The system role matters.

Historical Office profile requires at minimum:

- Excel
- Word
- Outlook
- OneDrive
- Teams when selected
- printing
- WebView2/runtime dependencies
- Defender
- Windows Update

The role model must be explicit so an Office workstation and a gaming/custom desktop do not receive identical recommendations.
