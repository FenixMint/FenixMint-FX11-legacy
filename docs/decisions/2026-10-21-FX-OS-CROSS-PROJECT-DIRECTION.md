# Cross-project owner direction — FX OS / FX11 Builder (2026-10-21)

Status: **OWNER DECISION (direction)** / **PLANNED (future work)**. Documentation-only addition; no prior knowledge is removed, superseded implicitly or rewritten.

## Owner decision

- FX OS has evolved from an improved Tiny11 concept toward a wider **Linux + Windows** system-customization family.
- Windows track: **develop our own FX11 Builder**. Preserve the distinction between historical Windows/PowerShell FX11 Legacy and the separate next-generation Linux-native/offline FX11 Builder.
- Linux track: for now develop modification/configuration layers on **existing distributions**, starting from **Fedora KDE** and potentially **LMDE** later, rather than maintaining an independent distribution.
- Windows major system generations change less frequently than Linux distribution releases. Use that difference when planning independent compatibility, release and validation cycles; both tracks still require updates and testing.
- Preserve prior GitHub records, prototypes and decisions.

## Boundaries / existing work preserved

This record **does not** authorize replacing the current Windows/PowerShell FX11 Native Builder, Tiny Retrofit, Windows Retrofit, Agent, Fenix System, privacy/guard policy or Visual Pack.

The current separate `FenixMint/FX11-Builder` Linux-native image-building work is also preserved. Whether/how those two builder generations ultimately converge is **OPEN**; there is no automatic repository merge, destructive refactor or renaming.

Keep existing FX11 philosophy:
- `DISCOVER -> AUDIT -> CLASSIFY -> PLAN -> APPROVE -> APPLY -> VERIFY -> GUARD`
- `FX11 Quality > FX11 Uniformity`
- security, servicing, recovery, user choice, explainable change and independent visual customization.

FX NEXUS remains a separate project. No installed-system, repository-history, provider or user-data changes are authorized here.

## Authoritative cross-reference

The fuller dated family-level decision record is:
[FX OS family and FX11 Builder direction](https://github.com/FenixMint/FX11-Builder/blob/main/docs/decisions/2026-10-21-FX-OS-FAMILY-AND-FX11-BUILDER.md).

Earlier documents (including `docs/checkpoints/FX11-KNOWLEDGE-CP1-2026-10-07.md` and `docs/roadmap/FX11-LEGACY-ROADMAP.md`) remain valid historical/current references. Changes to particular roadmap items require deliberate follow-up decisions, not silent reinterpretation of this note.

**Documentation checkpoint only; implementation and acceptance tests not performed.**
