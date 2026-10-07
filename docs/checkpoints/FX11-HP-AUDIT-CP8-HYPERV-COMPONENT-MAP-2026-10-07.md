# FX11 HP audit checkpoint CP8 — Hyper-V component map — 2026-10-07

Status: READ-ONLY DISCOVERY / SANITIZED

No system changes were made.

## Observed Windows optional features

Enabled:

- VirtualMachinePlatform
- Microsoft-Hyper-V-All
- Microsoft-Hyper-V
- Microsoft-Hyper-V-Tools-All
- Microsoft-Hyper-V-Management-PowerShell
- Microsoft-Hyper-V-Hypervisor
- Microsoft-Hyper-V-Services
- Microsoft-Hyper-V-Management-Clients

## Dependency context

Confirmed elsewhere in this audit:

- Fedora is actively running as WSL2.
- AlmaLinux is registered as WSL2 but stopped.
- zero classic Hyper-V virtual machines are registered.
- Windows Sandbox and Containers are disabled.
- WSL2 remains an active dependency.

Microsoft documents that WSL2 uses a subset of Hyper-V architecture supplied through the **Virtual Machine Platform** optional component. Therefore the full Hyper-V role is not required merely because WSL2 is in use.

## Current FX11 classification

- VirtualMachinePlatform: KEEP / REQUIRED
- WSL2 runtime: KEEP / REQUIRED
- Microsoft-Hyper-V-All and its management/services stack: OPTIONAL-CANDIDATE
- Microsoft-Hyper-V-Hypervisor: do not disable independently as a first action; WSL2 still needs the Windows virtualization substrate through VirtualMachinePlatform
- VBS: REVIEW / verify after any role change

## Safe apply principle

If the owner chooses to remove the unused full Hyper-V role:

1. do **not** disable VirtualMachinePlatform;
2. disable the parent Hyper-V role first, not random child components;
3. do not use `bcdedit /set hypervisorlaunchtype off`, because WSL2 still needs the hypervisor architecture;
4. reboot;
5. verify Fedora WSL2 starts;
6. verify `VirtualMachinePlatform` remains enabled;
7. verify full Hyper-V role state;
8. verify VBS/hypervisor state separately.

Rollback path:

`Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V-All -All`

Reboot may be required.

## Architecture lesson

FX11 must distinguish:

- full Hyper-V product/management role;
- the underlying Hyper-V architecture used by WSL2;
- Virtual Machine Platform;
- VBS/security consumers.

These are related but not equivalent capabilities.
