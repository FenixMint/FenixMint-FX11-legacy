# FX11 variants and safe convergence

Status: OWNER DECISION / ACTIVE ARCHITECTURE

## Why three variants exist

The same quality cannot always be achieved safely from different starting states.

FX11 must not force a stock Windows installation to look exactly like a freshly built FX11 image if doing so would reduce stability, serviceability or security.

**FX11 Quality > FX11 Uniformity**

## Variant A — FX11 Native / Windows Builder

Starting point: official Windows 11 installation media.

Current generation: Windows/PowerShell-based image creation and setup customization.

Advantages:

- decisions can be applied before first user profile exists;
- selected Appx can be omitted from provisioning;
- privacy/consumer policy can exist before first sign-in;
- visual baseline may be built into the installation;
- First Run and Agent can start from a known state;
- cleaner provenance than post-install removal.

Current source inspiration for image mechanics: `tiny11maker.ps1`, not Tiny11 Core.

Native must retain the FX11 security/serviceability baseline.

## Variant B — FX11 Tiny Retrofit

Starting point: existing Tiny11.

First principle: discover what already happened.

Do not assume all Tiny11 releases/builds have the same component set.

Possible outcomes:

- already absent and desired => COMPLIANT;
- absent but required by selected profile => evaluate repair;
- present and undesirable => normal FX11 plan;
- irreversible/unsafe difference => PARTIAL or NOT_SAFE_TO_CHANGE.

Reference history: Lenovo ThinkPad L13 Gen 2 AMD, where Tiny11 was subsequently refined with FX11 privacy, security, Office, Lenovo power, guards, EFI and monitoring work.

## Variant C — FX11 Windows Retrofit

Starting point: ordinary installed Windows 11.

Most conservative path.

Reasons:

- per-user Appx state exists;
- provisioned Appx state exists;
- OEM packages/services may be active;
- installed apps may depend on runtimes;
- servicing history may make removal materially different from offline omission;
- user data and associations already exist.

A valid result may explicitly contain:

```
SAFE TO REMOVE
SAFE TO DISABLE
POLICY ONLY
KEEP
UNKNOWN
NOT ACHIEVABLE SAFELY
```

"Not achievable safely" is not a failure.

## Convergence model

FX11 converges toward capability targets, not byte-identical systems.

Example:

```
Target: consumer Clipchamp not wanted

Native:
  omit from provisioned image

Tiny:
  already absent -> COMPLIANT

Stock Windows:
  installed/provisioned -> plan safe removal
  active dependency/problem -> NOT_SAFE_TO_CHANGE
```

All can be correct FX11 outcomes.

## Visual independence

Visual customization is orthogonal to optimization.

Native:
- visual baseline may be integrated in build/setup.

Tiny / Windows Retrofit:
- visual pack is optional;
- may run alone;
- may be applied after optimization;
- may be omitted entirely.

The policy engine must not use visual appearance as an FX11 compliance requirement unless a user explicitly selected that visual profile.
