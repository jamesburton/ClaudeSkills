# Phase 2 Research: OpenTelemetry Specialist

## Research Goal

Phase 2 should create a focused `opentelemetry-dotnet` skill that behaves like a real peer to `aspire` and `masstransit`, not a generic observability note set. The skill needs to define telemetry policy and boundaries clearly enough that later API, testing, and persistence skills can consume it without re-deciding ownership.

## What The Planner Must Optimize For

- The skill must follow the Phase 1 package standard exactly.
- It should cover traces, metrics, and logs as first-class signals.
- It should emphasize resource naming, service identity, semantic conventions, and correlation.
- It should stay OTLP-first and mostly vendor-neutral in the core flow.
- It should define clear handoff rules to `aspire`, `masstransit`, and the orchestrator.

## Current Repo Baseline

### Reusable standards

- `skills/_standard/package-standard.md` defines the required package tree.
- `skills/_standard/skill-sections.md` defines the fixed `SKILL.md` contract.
- `skills/_standard/boundary-and-portability.md` defines the handoff model.
- `skills/_standard/reference-and-validation-policy.md` defines examples, link verification, and evidence rules.

### Existing integration peers

- `skills/aspire/` is the hosting/runtime peer for AppHost, service-defaults, and dashboard composition.
- `skills/masstransit/` is the messaging/runtime peer for consumer behavior, transports, middleware, and broker-facing concerns.

### Implications

- The OpenTelemetry skill should not absorb hosting composition or messaging behavior.
- The most valuable examples are ones that make those boundaries obvious.
- The skill should be authored as a full package from the start rather than retrofitted later.

## Recommended Skill Shape

Use the Phase 1 package contract directly:

```text
skills/opentelemetry-dotnet/
  SKILL.md
  references/
    signals-and-conventions.md
    exporters-and-collector.md
    troubleshooting.md
  examples/
    happy-path.md
    boundary-handoff.md
  validation/
    checklist.md
    evidence.md
  .claude-plugin/            # optional adapter
    plugin.json
    CLAUDE.md
  skills/opentelemetry-dotnet/CLAUDE.md   # optional adapter
```

## Recommended `SKILL.md` Focus

### Scope

Own:
- telemetry policy for .NET applications
- traces, metrics, and logs
- resource attributes and service identity
- propagation and correlation expectations
- OTLP/exporter guidance
- common telemetry troubleshooting

Do not own:
- Aspire AppHost/service-defaults wiring details
- MassTransit consumer and transport behavior
- backend-specific platform operations beyond concise references

### Boundary Model

- Continue when the work is about instrumentation policy, signal design, or telemetry export shape.
- Hand directly to `aspire` only when the next step is clearly AppHost/service-defaults/dashboard composition.
- Hand directly to `masstransit` only when the next step is clearly messaging behavior or broker-specific runtime policy.
- Return to the orchestrator when telemetry work spans hosting plus messaging, or when exporter/backend choice introduces broader platform decisions.

## Coverage Recommendations

### Signals

Treat traces, metrics, and logs as equal first-class sections instead of making traces the only “real” signal.

### Conventions

High emphasis on:
- `service.name`
- service instance/environment/resource identity
- semantic conventions
- trace/log correlation

These are core to `OBS-01` and `OBS-02`, not optional side notes.

### Troubleshooting

Cover the common failures agents are likely to hit:
- no telemetry emitted
- traces exist but logs are not correlated
- missing resource attributes
- duplicate/incorrect service identity
- exporter configured but signals not reaching backend

Do not turn the skill into a deep collector-operations runbook.

### Advanced Topics

Cover these meaningfully:
- OTLP as the default export path
- collector as recommended production path
- sampling basics and tradeoffs
- context propagation/baggage at a practical level
- backend-specific variants via references, not the canonical core

## Example Strategy

### Happy-path example

Use an Aspire-based distributed .NET service example. It should show:
- traces, metrics, and logs all enabled
- resource naming and identity
- OTLP export path
- where the OpenTelemetry skill stops before hosting-specific wiring becomes the main concern

### Boundary example

Use an Aspire + MassTransit crossover example. It should show:
- `opentelemetry-dotnet` defining telemetry expectations for a service and messaging flow
- handoff to `aspire` for runtime/service-defaults composition
- handoff to `masstransit` for consumer/retry/transport behavior
- orchestrator return when more than one next owner is needed

## Recommended References Split

### `signals-and-conventions.md`

Should cover:
- traces, metrics, logs overview
- resource identity and semantic conventions
- correlation guidance

### `exporters-and-collector.md`

Should cover:
- OTLP-first export path
- collector as preferred production path
- vendor/backend variants as secondary paths

### `troubleshooting.md`

Should cover:
- missing signals
- broken correlation
- resource naming mistakes
- exporter/reachability sanity checks

## Current Official Source Signals

The official OpenTelemetry .NET docs present traces, metrics, and logs as stable major signals, and the Microsoft .NET observability docs position OTLP and vendor-neutral telemetry as the general path for APM integration. Official getting-started examples use ASP.NET Core/Minimal API style apps, which fits this repo’s decision to keep the OpenTelemetry skill practical and .NET-native.

## Practical Rollout Order Inside This Phase

### 1. Author the canonical skill package

Create:
- `SKILL.md`
- focused references
- happy-path and boundary examples

This is the main delivery for `OBS-01`.

### 2. Add validation and adapters

Create:
- validation checklist/evidence
- optional adapter surfaces if chosen
- explicit boundary validation against `aspire` and `masstransit`

This is where `OBS-02` is proven.

## Common Pitfalls

### Boundary drift

- Letting the skill own Aspire wiring instead of telemetry policy
- Letting the skill prescribe MassTransit behavior instead of telemetry implications

### Signal imbalance

- Overweighting tracing and leaving metrics/logs shallow, which would conflict with the Phase 2 discussion decisions

### Backend lock-in

- Turning the core body into vendor-specific setup instead of keeping OTLP-first and variant-friendly

### Weak examples

- Generic ASP.NET-only snippets that ignore the repo’s distributed-.NET baseline

## Validation Architecture

Phase 2 validation should have three layers:

### Layer 1: Structural conformance

Validate that the new skill package includes:
- canonical `SKILL.md`
- focused `references/`
- `examples/happy-path.md`
- `examples/boundary-handoff.md`
- `validation/checklist.md`
- `validation/evidence.md`

### Layer 2: Requirement proof

Validate:
- `OBS-01` by showing the skill covers traces, metrics, logs, conventions, correlation, and OTLP/export guidance
- `OBS-02` by showing the skill explicitly distinguishes ownership versus `aspire`, `masstransit`, and orchestrator return

### Layer 3: Reference and evidence quality

Validate:
- official references exist and are live
- examples are realistic and boundary-aware
- evidence file records link checks and boundary validation outcomes

### Suggested Nyquist checks

- file-tree inspection for required package artifacts
- content inspection for traces/metrics/logs and conventions coverage
- content inspection for named handoff rules to `aspire` and `masstransit`
- link verification for official references
- spot review of examples for happy-path plus multi-owner boundary behavior

## Sources

- https://opentelemetry.io/docs/languages/dotnet/
- https://opentelemetry.io/docs/languages/dotnet/getting-started/
- https://opentelemetry.io/docs/languages/dotnet/traces/getting-started-aspnetcore/
- https://opentelemetry.io/docs/languages/dotnet/logs/getting-started-aspnetcore/
- https://opentelemetry.io/docs/specs/semconv/dotnet/
- https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel
- https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-otlp-example

## Recommended Planner Assumptions

- Two plans are enough for this phase if the first builds the core skill package and the second adds validation/adapters/evidence.
- The skill should be production-shaped but still concise; references carry the deeper variants.
- Adapter presence is optional; the planner does not need to force every adapter surface if the canonical skill is complete and the absence is documented.
