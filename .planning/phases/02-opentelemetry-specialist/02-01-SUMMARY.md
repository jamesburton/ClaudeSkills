---
phase: 02-opentelemetry-specialist
plan: 01
subsystem: infra
tags: [opentelemetry, dotnet, otlp, observability, aspire, masstransit]
requires:
  - phase: 01-shared-skill-standard
    provides: canonical skill package structure, boundary rules, and validation policy
provides:
  - canonical opentelemetry-dotnet skill body
  - focused observability references for conventions, exporters, and troubleshooting
  - realistic happy-path and boundary handoff examples
affects: [phase-02-plan-02, aspire, masstransit, csharp-minimal-api, xunit-integration-testing]
tech-stack:
  added: [OpenTelemetry .NET guidance baseline 1.12.0, OTLP-first reference set]
  patterns: [equal-signal observability ownership, OTLP-first with collector recommendation, peer-boundary handoff model]
key-files:
  created:
    - skills/opentelemetry-dotnet/SKILL.md
    - skills/opentelemetry-dotnet/references/signals-and-conventions.md
    - skills/opentelemetry-dotnet/references/exporters-and-collector.md
    - skills/opentelemetry-dotnet/references/troubleshooting.md
    - skills/opentelemetry-dotnet/examples/happy-path.md
    - skills/opentelemetry-dotnet/examples/boundary-handoff.md
  modified: []
key-decisions:
  - "Treat traces, metrics, and logs as equal first-class signals instead of a tracing-only default."
  - "Keep the canonical export path OTLP-first and recommend the OpenTelemetry Collector as the normal production route."
  - "Define opentelemetry-dotnet as the telemetry-policy owner, with Aspire and MassTransit as peer owners for hosting and messaging behavior."
patterns-established:
  - "Observability skill pattern: lead with service identity and semantic conventions before exporter or backend detail."
  - "Boundary pattern: direct handoff only when one next owner is clear; otherwise return control to the orchestrator."
requirements-completed: [OBS-01, OBS-02]
duration: 7min
completed: 2026-03-11
---

# Phase 2 Plan 1: Author Core OpenTelemetry Guidance, Examples, And References Summary

**Canonical OpenTelemetry .NET skill with equal-signal guidance, OTLP-first references, and multi-owner boundary examples for Aspire and MassTransit**

## Performance

- **Duration:** 7 min
- **Started:** 2026-03-11T17:23:30Z
- **Completed:** 2026-03-11T17:30:30Z
- **Tasks:** 3
- **Files modified:** 6

## Accomplishments

- Added the canonical `opentelemetry-dotnet` skill body with explicit scope, workflow, outputs, and peer boundaries.
- Added three focused reference files covering signals and conventions, OTLP and collector guidance, and troubleshooting with official sources and current baseline notes.
- Added the required example pair showing both the happy-path distributed .NET baseline and the multi-owner handoff model.

## Task Commits

Each task was committed atomically:

1. **Task 1: Create the canonical OpenTelemetry skill body** - `fdd2b4f` (feat)
2. **Task 2: Add focused references for conventions, exporters, and troubleshooting** - `2b55150` (feat)
3. **Task 3: Create the required happy-path and boundary examples** - `7ef98ce` (feat)

## Files Created/Modified

- `skills/opentelemetry-dotnet/SKILL.md` - Canonical observability specialist contract with OTLP-first and peer-boundary rules.
- `skills/opentelemetry-dotnet/references/signals-and-conventions.md` - Signal balance, service identity, semantic conventions, and correlation guidance.
- `skills/opentelemetry-dotnet/references/exporters-and-collector.md` - OTLP-first exporter strategy and collector recommendation path.
- `skills/opentelemetry-dotnet/references/troubleshooting.md` - Common failures this skill owns before escalation to peer owners.
- `skills/opentelemetry-dotnet/examples/happy-path.md` - Aspire-aligned observability baseline example.
- `skills/opentelemetry-dotnet/examples/boundary-handoff.md` - Multi-owner example crossing into `aspire`, `masstransit`, and orchestrator sequencing.

## Decisions Made

- Treated traces, metrics, and logs as equal first-class signals because Phase 2 context explicitly rejected a tracing-only default.
- Recorded a current baseline in the references using current official OpenTelemetry and Microsoft .NET guidance plus OpenTelemetry .NET package version `1.12.0`.
- Kept backend guidance vendor-neutral in the canonical flow and pushed concrete export detail into focused references.

## Deviations from Plan

None - plan executed exactly as written.

## Issues Encountered

- None.

## User Setup Required

None - no external service configuration required.

## Next Phase Readiness

- The repo now has the core `opentelemetry-dotnet` package needed for Phase 2 Plan 2 validation and adapter work.
- Phase 2 Plan 2 can build directly on these files to add validation artifacts, link-evidence recording, and optional adapter material.

## Self-Check: PASSED

- FOUND: `.planning/phases/02-opentelemetry-specialist/02-01-SUMMARY.md`
- FOUND: `fdd2b4f`
- FOUND: `2b55150`
- FOUND: `7ef98ce`

---
*Phase: 02-opentelemetry-specialist*
*Completed: 2026-03-11*
