# Signals And Conventions

## Purpose

Use this reference when the main question is about what telemetry to emit, how to name it, and how to
keep traces, metrics, and logs correlated across distributed .NET services.

## Current Baseline

- Review date: 2026-03-11
- .NET guidance baseline: Microsoft .NET observability documentation for OpenTelemetry on current supported .NET releases
- OpenTelemetry .NET package baseline: `OpenTelemetry.Extensions.Hosting` 1.12.0
- Convention baseline: OpenTelemetry semantic conventions for .NET and the general OpenTelemetry semantic convention model

Version-sensitive implication:

- When repository code or target runtime is older than the current baseline, keep the semantic model stable
  first, then adapt package APIs to the target framework instead of rewriting naming rules ad hoc.

## Signals Are Equal First-Class Outputs

- Traces answer request and workflow flow through services, dependencies, and background work.
- Metrics answer rate, latency, errors, saturation, and backlog trends.
- Logs answer event detail and local debugging context.
- The skill should not recommend traces alone when the operational question clearly needs metrics or log correlation too.

## Resource Identity

Start with resource identity before instrumenting custom spans or metrics.

Required minimum:

- `service.name` for stable service identity
- `service.namespace` when multiple bounded services share related names
- `service.version` when deployment tracking matters
- deployment environment attribute for stage separation

Useful additions when they materially improve diagnosis:

- service instance identity for replica-level debugging
- cloud or container attributes when workload placement is the actual troubleshooting axis

Avoid:

- changing `service.name` per environment or instance
- encoding transient deployment IDs into the base service identity
- emitting duplicate or contradictory resource attributes from multiple setup paths

## Semantic Conventions

- Prefer official semantic conventions before inventing custom span names or attributes.
- Use server/client/database/messaging/http conventions consistently so backends can group and analyze data correctly.
- Add custom attributes only for domain-specific detail that the conventions do not already cover.
- Keep custom attribute names stable and low-cardinality where possible.

Correlation rule:

- Logs should preserve trace context where the logging provider supports it.
- Messaging and background processing should preserve propagation context, not restart a disconnected trace unless that break is intentional.

## Signal-Specific Guidance

### Traces

- Instrument incoming and outgoing boundaries first: HTTP, gRPC, database, messaging, and notable internal activities.
- Name spans by operation intent, not by UI phrasing or ticket wording.
- Record errors using standard span status and exception recording patterns.

### Metrics

- Prefer counters, histograms, and gauges that answer service-level behavior questions.
- Watch cardinality. Per-user, per-order, or per-request metric dimensions usually create more damage than value.
- Align metric attributes with the same service identity and environment conventions used for traces.

### Logs

- Use structured logs with stable property names.
- Include enough context to explain the event without duplicating entire payloads.
- Preserve trace and span identifiers when supported so logs can pivot from traces cleanly.

## Official Sources

- OpenTelemetry .NET overview: https://opentelemetry.io/docs/languages/dotnet/
- OpenTelemetry .NET getting started: https://opentelemetry.io/docs/languages/dotnet/getting-started/
- OpenTelemetry .NET traces with ASP.NET Core: https://opentelemetry.io/docs/languages/dotnet/traces/getting-started-aspnetcore/
- OpenTelemetry .NET logs with ASP.NET Core: https://opentelemetry.io/docs/languages/dotnet/logs/getting-started-aspnetcore/
- OpenTelemetry .NET semantic conventions: https://opentelemetry.io/docs/specs/semconv/dotnet/
- Microsoft .NET observability with OpenTelemetry: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel
