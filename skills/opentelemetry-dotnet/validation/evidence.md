# Validation Evidence

## Links Verified

- URL: https://opentelemetry.io/docs/languages/dotnet/
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md
- URL: https://opentelemetry.io/docs/languages/dotnet/getting-started/
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md, exporters-and-collector.md
- URL: https://opentelemetry.io/docs/languages/dotnet/traces/getting-started-aspnetcore/
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md
- URL: https://opentelemetry.io/docs/languages/dotnet/logs/getting-started-aspnetcore/
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md
- URL: https://opentelemetry.io/docs/specs/semconv/dotnet/
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md
- URL: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-with-otel
  status: live
  verification date: 2026-03-11
  reference owner: signals-and-conventions.md, troubleshooting.md
- URL: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-otlp-example
  status: live
  verification date: 2026-03-11
  reference owner: exporters-and-collector.md, troubleshooting.md
- URL: https://opentelemetry.io/docs/collector/
  status: live
  verification date: 2026-03-11
  reference owner: exporters-and-collector.md, troubleshooting.md
- URL: https://opentelemetry.io/docs/collector/deployment/
  status: live via redirect to `/docs/collector/deploy/`
  verification date: 2026-03-11
  reference owner: exporters-and-collector.md

## Coverage Evidence

- traces: `SKILL.md` and `references/signals-and-conventions.md` define traces as first-class output and require inbound, outbound, and background instrumentation coverage.
- metrics: the canonical skill explicitly rejects tracing-only guidance and requires metrics for latency, failures, throughput, and saturation where relevant.
- logs: the skill treats logs as a first-class signal and requires structured enrichment with trace context when supported.
- OTLP: `SKILL.md` and `references/exporters-and-collector.md` make OTLP the default export protocol.
- conventions: `service.name`, semantic conventions, and stable custom-attribute rules are explicit in canonical guidance.
- common failures: `references/troubleshooting.md` covers missing telemetry, broken correlation, bad resource identity, and exporter or Collector reachability.

## Boundary Verification

- aspire: the skill stops once the remaining work is AppHost composition, service defaults ownership, dashboard lifecycle, runtime wiring, or environment injection.
- masstransit: the skill stops once the remaining work is consumer behavior, retry/redelivery policy, middleware ordering, transport topology, or broker-facing messaging policy.
- orchestrator: the skill returns to the orchestrator when more than one next owner remains, including mixed telemetry, Aspire, and MassTransit sequencing work.
- handoff payload: the canonical skill requires current objective, completed work, unresolved risks, and next decision or question.

## Adapter Parity

- adapter path:
  parity outcome: pending review
  differences found: not yet assessed
  allowed gap: adapter surfaces are optional until reviewed

## Limitations

- Adapter parity outcome will be finalized after optional Claude-facing surfaces are either added as thin projections or recorded as allowed absences.
