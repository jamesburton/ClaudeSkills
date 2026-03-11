---
name: opentelemetry-dotnet
description: >
  Use when the user needs .NET OpenTelemetry guidance for traces, metrics, logs, resource identity,
  semantic conventions, propagation, OTLP export, or telemetry troubleshooting in distributed services.
  Trigger when the work is primarily about instrumentation policy, signal coverage, correlation, or
  collector/exporter shape rather than AppHost composition or messaging runtime behavior.
allowed-tools: Read, Glob, Grep, Bash(*), Write, Edit
---

# OpenTelemetry .NET Skill

`opentelemetry-dotnet` is the observability specialist for distributed .NET services. It owns telemetry
policy for traces, metrics, and logs, with equal emphasis on service identity, semantic conventions,
correlation, and OTLP-first export guidance.

## Scope

Own the observability layer for .NET services: signal design, resource identity, instrumentation policy,
propagation, exporter shape, and focused troubleshooting. This skill is a peer to `aspire` and
`masstransit`, not a replacement for hosting composition or messaging runtime ownership.

## In-Scope Work

- Define traces, metrics, and logs as first-class signals for a .NET service or service set.
- Set resource identity with `service.name`, `service.namespace`, `service.version`, deployment
  environment, and instance-level attributes when they materially affect correlation.
- Choose and explain semantic conventions, activity naming, span attributes, metric naming, and log
  enrichment expectations.
- Configure OpenTelemetry SDK usage in .NET applications, including propagation and OTLP-first export.
- Recommend when to use the OpenTelemetry Collector as the production path versus direct exporter wiring.
- Review missing telemetry, broken correlation, duplicate service identity, and common exporter-path issues.
- Document observability decisions, evidence, and the exact follow-up owner when the work leaves this boundary.

## Out-Of-Scope Work

- Aspire AppHost composition, service defaults package ownership, dashboard lifecycle, and resource graph design.
- MassTransit consumer behavior, retry/redelivery policy, transport topology, saga behavior, and broker-specific middleware.
- Backend-specific monitoring platform operations beyond concise exporter or collector guidance.
- Product-level SLO design, alert routing, or organization-specific observability governance outside the immediate .NET telemetry change.
- Cross-domain sequencing decisions where both hosting and messaging owners need coordinated next steps.

## Boundary And Handoff Rules

- Continue when the work is about traces, metrics, logs, propagation, resource identity, semantic
  conventions, OTLP, collector usage, or focused telemetry troubleshooting.
- Hand directly to `aspire` only when the observability policy is already defined and the one clear next
  owner is AppHost, service-defaults, dashboard, or runtime composition work.
- Hand directly to `masstransit` only when telemetry expectations are already defined and the one clear
  next owner is consumer behavior, middleware behavior, transport settings, or broker-facing messaging policy.
- Return to the orchestrator when the next step requires more than one owner, such as telemetry policy plus
  Aspire runtime composition plus MassTransit messaging behavior.
- Return to the orchestrator when exporter or backend decisions widen into platform tradeoffs that exceed a
  bounded .NET telemetry recommendation.

Required handoff payload:

- current objective
- completed work
- unresolved risks
- next decision or question

Boundary shorthand:

- `opentelemetry-dotnet` owns telemetry policy and correlation.
- `aspire` owns hosting composition and runtime wiring.
- `masstransit` owns messaging behavior and transport policy.
- The orchestrator owns multi-specialty sequencing when no single next owner is clear.

## Working Method / Workflow

1. Confirm the telemetry objective, target services, and whether the request is about traces, metrics,
   logs, or all three.
2. Establish resource identity first. Require a stable `service.name` and name any additional resource
   attributes needed for environment, version, or instance-level clarity.
3. Treat traces, metrics, and logs as equal first-class signals. Do not produce a tracing-only answer when
   the request affects operational visibility more broadly.
4. Apply semantic conventions before inventing custom names. Use custom attributes only where conventions do
   not cover the domain signal.
5. Keep the default export path OTLP-first. Prefer the OpenTelemetry Collector for production fan-out,
   policy centralization, and backend neutrality unless the change is intentionally small and direct export
   is sufficient.
6. Preserve correlation across HTTP, gRPC, background workers, and messaging handoffs. If the flow crosses
   into MassTransit or Aspire ownership, stop after defining telemetry expectations and hand off explicitly.
7. Validate the likely failure modes: no telemetry emitted, partial signal coverage, missing resource
   attributes, broken trace/log correlation, or exporter reachability issues.

## Output Expectations

- A concise observability plan or change set covering traces, metrics, logs, service identity, and export path.
- Explicit resource and semantic-convention decisions, including any custom attributes that remain necessary.
- OTLP and collector guidance that states whether the collector is recommended, optional, or intentionally deferred.
- Boundary notes naming `aspire`, `masstransit`, or the orchestrator when work leaves this skill's scope.
- Verification notes that state how the agent checked signal presence, correlation, and exporter health.

## References Map

- `references/signals-and-conventions.md`
  OpenTelemetry .NET signal coverage, resource identity, semantic conventions, and correlation guidance.
- `references/exporters-and-collector.md`
  OTLP-first exporter strategy, collector guidance, and backend-neutral production patterns.
- `references/troubleshooting.md`
  Common failures this skill owns, including missing telemetry, resource identity mistakes, and broken correlation.
- `examples/happy-path.md`
  Aspire-aligned example showing a realistic distributed .NET telemetry baseline.
- `examples/boundary-handoff.md`
  Multi-owner example showing where `opentelemetry-dotnet` stops and when control returns to the orchestrator.

## Validation Expectations

- `SKILL.md` must clearly treat traces, metrics, and logs as first-class rather than leaving metrics or logs
  as token afterthoughts.
- The canonical guidance must name `service.name`, semantic conventions, OTLP, and the collector recommendation path.
- Boundary rules must explicitly distinguish telemetry ownership from `aspire` and `masstransit`, and must
  preserve the repo rule that direct handoff happens only when one next owner is clear; otherwise return to
  the orchestrator.
- References must point to focused official sources, and any version-sensitive guidance must record a current
  baseline so later maintainers can tell what the recommendation assumed.
- Examples must be realistic enough to fit this repo's distributed-.NET baseline instead of reading like generic snippets.

## Adapter Notes

Runtime adapters are optional thin projections of this canonical `SKILL.md`. If adapter files are added
later, they must preserve the same scope, equal-signal ownership, OTLP-first default, and boundary rules
without introducing semantic drift.
