# FX11 User Ownership Principle

Status: **OWNER DECISION / NON-NEGOTIABLE**

## Core principle

**The computer and the operating system belong to the user.**

FX11 exists to return practical control over the system to the person who owns and uses the computer.

A vendor, cloud provider, OEM, software publisher or FX11 itself must not become the effective owner of the machine.

## What this means

FX11 should:

- make important system behavior understandable;
- expose meaningful choices instead of hiding them;
- prefer local control where practical;
- let the user decide which optional services, applications, integrations and visual changes are enabled;
- make material changes visible and auditable;
- preserve rollback paths where technically possible;
- avoid unnecessary permanent background components;
- keep privacy/security controls understandable rather than opaque;
- make it possible to remove or disable FX11 components without breaking Windows;
- avoid vendor lock-in and avoid creating FX11 lock-in.

## What FX11 must not do

FX11 must not:

- silently collect or publish private user/device identity;
- force a cloud account where the operating system can function locally;
- silently install optional software;
- silently re-enable optional features the user deliberately disabled;
- silently remove software whose purpose or dependency is unknown;
- make telemetry, advertising, profiling or AI/Recall-style data collection a condition of normal use;
- replace Microsoft/OEM control with an equally opaque FX11 control layer;
- treat the user as an obstacle to be managed.

## Security and freedom are compatible

Returning control to the user does not mean disabling security.

FX11 keeps security mechanisms such as Defender, Firewall, servicing, Recovery and encryption unless there is a deliberate, informed and technically justified reason to change them.

The user should be able to understand:

- what protects the system;
- what data leaves the system;
- what runs in the background;
- what can be disabled;
- what depends on what;
- what a proposed change will do.

## Default posture

For optional capabilities:

**inform -> offer -> let the user choose**

For risky or unknown changes:

**do not modify**

For accepted changes:

**apply -> verify -> log -> allow rollback where possible**

## Project standard

A technically impressive optimization that reduces user agency is not a successful FX11 optimization.

The target is a system that is:

**light + private + secure + stable + serviceable + user-controlled**

FX11 is a tool owned by the user, not a platform that owns the user.
