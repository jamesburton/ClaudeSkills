# Troubleshooting

## Purpose

Use this reference when telemetry is missing, partially missing, misidentified, or no longer correlated
correctly across a distributed .NET flow.

## Current Baseline

- Review date: 2026-03-11
- Baseline assumption: .NET service uses current OpenTelemetry .NET packages, stable resource identity, and an OTLP-capable export path
- Scope: failures this skill owns directly before the issue becomes broader hosting, transport, or platform operations work

## Missing Signals

Symptoms:

- traces appear but metrics or logs do not
- no telemetry reaches the backend
- only framework-generated spans appear, not app-specific activity

Checks:

- confirm the signal was actually enabled in the .NET setup path
- confirm the relevant instrumentation package or logging integration was added
- confirm the exporter path matches the signal being emitted
- confirm sampling or filtering is not dropping the data unexpectedly

## Broken Correlation

Symptoms:

- logs cannot be pivoted from traces
- background jobs or consumer flows start new unrelated traces
- messaging spans exist but parent-child relationships are missing

Checks:

- confirm propagation is preserved at HTTP, gRPC, and messaging boundaries
- confirm logging enrichment includes trace context where the provider supports it
- confirm messaging work that needs MassTransit behavior changes is handed to `masstransit` after telemetry expectations are defined

## Bad Resource Identity

Symptoms:

- one logical service appears as many unrelated services
- multiple services collapse into one name
- dashboards split data unexpectedly by deployment or replica

Checks:

- confirm `service.name` is stable across replicas of the same logical service
- confirm environment and instance attributes are not being used as the base service name
- confirm multiple setup paths are not overwriting or duplicating resource attributes

## Exporter Or Collector Reachability

Symptoms:

- telemetry is emitted locally but never appears in the backend
- app logs show exporter failures or connection resets
- only some services reach the backend

Checks:

- confirm OTLP endpoint, protocol, and auth settings match the target
- confirm collector or backend is reachable from the service environment
- confirm the failure is not really an Aspire runtime wiring issue; if it is, hand to `aspire` when that is the one clear next owner

## Escalation Boundary

- Stay in this skill when the fix is telemetry configuration, signal setup, resource identity, or propagation policy.
- Hand to `aspire` when the remaining issue is runtime composition, local dashboard wiring, service defaults packaging, or environment injection.
- Hand to `masstransit` when the remaining issue is consumer behavior, middleware ordering, or transport-specific message flow.
- Return to the orchestrator when more than one next owner remains or the exporter path has become a broader platform decision.

## Official Sources

- OpenTelemetry .NET docs: https://opentelemetry.io/docs/languages/dotnet/
- Microsoft .NET observability with OpenTelemetry: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel
- Microsoft .NET OTLP example: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-otlp-example
- OpenTelemetry Collector docs: https://opentelemetry.io/docs/collector/
