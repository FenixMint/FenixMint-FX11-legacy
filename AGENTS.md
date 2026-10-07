# AGENTS.md — FX11 Legacy execution guardrails

This repository contains the Windows/PowerShell generation and retrofit era of FX11.

## Source-of-truth order

Read and follow in this order:

1. `AGENTS.md`
2. `docs/START-HERE.md`
3. current architecture documents
4. current checkpoint(s)
5. current roadmap / task document
6. code and tests

Do not reconstruct architecture from memory when the repository already contains a newer decision.

## Core doctrine

FX11 is not a blanket debloat script.

Primary workflow:

```
DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD
```

Earlier operational shorthand remains valid inside this model:

```
AUDIT -> APPLY -> VERIFY -> POLICY
```

## Quality rule

**FX11 Quality > FX11 Uniformity**

There are three supported starting paths:

- FX11 Native / Windows Builder
- FX11 Tiny Retrofit
- FX11 Windows Retrofit

They do not need to converge to an identical package/service/process count.

Per-capability results may legitimately be:

- COMPLIANT
- PARTIAL
- NOT_APPLICABLE
- NOT_SAFE_TO_CHANGE
- UNAVAILABLE
- UNKNOWN

Never force a system into an unsafe state merely to make variants look identical.

## Safety / serviceability

Preserve by default unless a current, reviewed rule explicitly says otherwise:

- Windows Update servicing
- Microsoft Defender
- Windows Firewall
- Windows activation
- Recovery / WinRE
- PowerShell
- .NET
- Visual C++ runtimes
- Windows Installer
- Microsoft Edge WebView2 runtime when dependencies exist
- Microsoft Store infrastructure
- App Installer
- printing when the selected profile needs it
- Office / Microsoft 365 dependencies
- hardware / firmware / OEM components whose role is not understood

Unknown means **do not modify**.

Do not use `DISM /ResetBase` in FX11 Legacy baseline cleanup.

Do not take ownership of protected system tasks or edit TaskCache just to force a change. Prefer supported policy/task mechanisms. If Windows blocks a nonessential change, record it rather than breaking servicing.

## One change at a time

For live-system changes:

1. show current state;
2. show intended state and reason;
3. apply one controlled change;
4. verify immediately;
5. retain rollback path;
6. log the outcome.

Avoid giant opaque debloat scripts.

## Hardware awareness

Hardware policy must be based on discovery, not assumptions.

Initial platform/vendor scope includes:

- Lenovo
- HP
- Dell
- ASUS
- ASRock
- Generic / custom desktop

Use Windows-native identity sources (CIM/WMI/PnP/firmware/baseboard) for static classification. LibreHardwareMonitor may augment runtime telemetry but must not be the sole authority for identity or policy.

OEM service != bloat.

## Visual layer

Visual/UX customization is a separate capability from optimization.

- FX11 Native may bake the selected visual baseline into the ISO/install path.
- Tiny/Windows Retrofit may apply visual changes independently of optimization.
- A user must be able to run visual customization without running privacy/debloat/system tuning.
- Likewise, optimization must not require visual customization.

## Builder scope

Current native build phase is Windows/PowerShell based and may be inspired by `tiny11maker.ps1`.

The stronger Linux/offline builder is a later phase and belongs to the separate FX11 Builder line.

Do not import the destructive Tiny11 Core approach into this repository.

## Privacy

Goal: reduce diagnostics, telemetry, profiling, advertising/consumer experiences and unwanted AI/Recall features while preserving security, servicing and required applications.

Privacy guards must be idempotent and auditable.

## Data minimization / no private traces

This repository and all shareable FX11 artifacts must contain **no real private or uniquely identifying deployment data**.

Do not commit or publish real:

- hardware or storage serial identifiers;
- usernames, account addresses, tenant identifiers or user security identifiers;
- computer or host names;
- network identity such as adapter addresses, IP addresses or Wi-Fi names;
- authentication or recovery secrets;
- exact user-profile or document paths;
- raw event-log payloads containing identity-bearing data;
- unique device identifiers that are not required for a public technical artifact.

Hardware-aware decisions use model, class and capability facts, not unique device identity.

Audit/checkpoint schemas must exclude sensitive fields by design and apply a final privacy/redaction preflight before export.

Tests and documentation use synthetic identities only.

Full policy:

`docs/security/FX11-DATA-MINIMIZATION-AND-REDACTION.md`

## Security

Optimization never takes priority over security.

Defender, Firewall, servicing, recovery and encryption capabilities must be audited before modification.

Do not disable device encryption by default.

## Licensing

LibreHardwareMonitor integration rules are documented in:

`docs/legal/LIBREHARDWAREMONITOR.md`

Preserve upstream and third-party license notices when redistributing dependencies.

## Status language

Use explicit status words in documentation:

- OWNER DECISION
- IMPLEMENTED
- TESTED
- ACCEPTED
- PLANNED
- OPEN
- SUPERSEDED

Do not turn an old idea into an implementation claim.
