# FX11 hardware-adaptive architecture

Status: OWNER DECISION / ACTIVE ARCHITECTURE

## Rule

Never optimize from brand assumptions alone.

`OEM service == bloat` is forbidden reasoning.

## Static discovery sources

Use Windows-native sources for identity:

- Win32_ComputerSystem
- Win32_BaseBoard
- Win32_BIOS
- Win32_Processor
- PnP devices / drivers
- storage/NVMe controller data
- battery classes
- network adapters
- audio devices
- TPM
- Secure Boot
- firmware/UEFI facts
- power capability output such as `powercfg /a`

LibreHardwareMonitor may add runtime sensor data, but is not the only identity source.

## Required hardware facts

At minimum:

- system manufacturer;
- system model/family/SKU;
- baseboard manufacturer/product/version;
- BIOS manufacturer/version/date;
- CPU vendor/model;
- GPU vendor/model(s);
- memory;
- storage devices/controllers;
- laptop/desktop classification;
- battery presence/capabilities;
- network adapters;
- audio stack;
- Bluetooth;
- camera;
- Modern Standby / S3 capability;
- TPM / Secure Boot;
- WSL/Hyper-V/hypervisor facts;
- display topology.

## Adapter model

```
Hardware/
├── Lenovo
├── HP
├── Dell
├── ASUS
├── ASRock
└── Generic
```

An adapter may:

- identify vendor utilities;
- identify known required services;
- identify optional vendor agents;
- expose safe recommendations;
- provide verification probes.

It must not silently remove unknown OEM software.

## Lenovo historical reference

ThinkPad L13 Gen 2 AMD reference experience:

Kept:

- IBMPMSVC
- LITSSVC
- TPHKLOAD

Disabled when unused/problematic in the tested setup:

- LenovoBrightCtrl
- LenovoSmartStandby

Battery threshold policy used Lenovo Power Manager registry data:

```
HKLM\SOFTWARE\WOW6432Node\Lenovo\PWRMGRV\Data

ChargeStartControl=1
ChargeStopControl=1
ChargeStartPercentage=50
ChargeStopPercentage=80
```

This is a historical tested reference, not a universal Lenovo rule.

## HP

HP is the first planned broad stock-Windows Retrofit audit target.

Initial HP phase must be READ-ONLY:

- discover exact model;
- discover baseboard;
- inventory HP software/services/tasks;
- classify only known items;
- mark unknown items UNKNOWN;
- produce plan, not changes.

## ASRock / custom desktop

ASRock must be a first-class **motherboard** adapter because custom desktops may report a generic system OEM.

Example identity:

```
System OEM: Generic / Custom
BaseBoard: ASRock
Board model: LiveMixer B650
Platform: AMD AM5
GPU: NVIDIA GeForce RTX 4060
```

ASRock discovery should look for, but not blindly disable:

- App Shop;
- A-Tuning;
- Polychrome RGB;
- Nahimic/audio extensions;
- Realtek LAN/audio components;
- AMD/Intel chipset components;
- firmware/update utilities;
- vendor services/startup items.

Likely classification examples:

- chipset driver: PROTECTED/KEEP;
- audio/LAN drivers: PROTECTED/KEEP;
- RGB control: OPTIONAL;
- tuning utility: CONDITIONAL.

Actual classification must be based on discovered role and dependencies.

## Power policy is hardware-dependent

Laptop rules may include:

- lid action;
- battery thresholds;
- low/critical/reserve actions;
- Modern Standby behavior.

Desktop rules may instead focus on:

- PCIe ASPM;
- USB selective suspend;
- CPU minimum/maximum policy;
- NVMe/storage power;
- wake timers;
- Wake-on-LAN;
- 24x7 profile.

Never force S3 when hardware reports only Modern Standby S0.
