# FX11 HP audit checkpoint CP7 — WSL correction — 2026-10-07

Status: OWNER CORRECTION / READ-ONLY DISCOVERY / SANITIZED

## Correction

The HP system **does have WSL in active use**.

Owner-confirmed state:

- Fedora is currently used under WSL.
- AlmaLinux is also installed under WSL but is currently unused.
- The long-term direction is native AlmaLinux on a dedicated partition.
- Until that migration is complete, Fedora WSL remains a real dependency.

No private distro filesystem contents, usernames, paths or guest data are recorded here.

## Why the earlier audit misclassified WSL

The earlier command queried this Windows Optional Feature:

`Microsoft-Windows-Subsystem-Linux`

and it returned:

`Disabled`

That result was incorrectly interpreted as "WSL disabled".

With current Microsoft Store-delivered WSL, that optional component is not a sufficient detector for WSL 2 usage. The Store WSL package and WSL 2 can exist and run while the legacy/inbox optional feature used for WSL 1 is disabled.

Therefore FX11 must **never infer WSL absence from that Optional Feature alone**.

## Correct detection model

FX11 Discovery must use the WSL runtime itself:

- `wsl --status`
- `wsl --version`
- `wsl --list --verbose`

For privacy-safe persisted audit output, retain only:

- WSL present: yes/no
- WSL runtime/package version
- number of distributions
- number running/stopped
- WSL generation per distro (1/2) as aggregate where possible

Distro names may be shown interactively to the owner, but normal public fixtures/checkpoints should not require them.

Do not inspect guest filesystems or user content.

## Dependency correction

Current classification:

- Virtual Machine Platform: **KEEP / ACTIVE DEPENDENCY** while Fedora WSL 2 remains in use.
- Full Hyper-V role: **CONDITIONAL / REVIEW**. WSL 2 uses the virtualization architecture exposed through Virtual Machine Platform and does not require the full Hyper-V management role for ordinary WSL 2 use.
- VBS: REVIEW / active.
- WSL Store/runtime: KEEP while Fedora WSL is used.
- AlmaLinux WSL distro: owner says installed but unused; removal is a separate user choice, not an optimization side effect.

## Architectural lesson

This is exactly why FX11 uses:

`DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE`

and does not apply after a single signal.

A Windows Optional Feature can represent only one implementation layer while the actual capability is delivered through a newer package/runtime.

The Discovery engine must use **capability-aware probes**, not only registry/feature-name heuristics.

## Next read-only WSL probe

Use:

```powershell
wsl --status; wsl --version; wsl --list --verbose
```

This is read-only and should be the canonical WSL discovery path for this machine.
