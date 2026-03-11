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

- adapter parity status: complete
- adapter path: `skills/opentelemetry-dotnet/.claude-plugin/plugin.json`
  parity outcome: aligned
  differences found: none; adapter adds package metadata only and does not restate or narrow canonical OpenTelemetry guidance.
  allowed gap: none
- adapter path: `skills/opentelemetry-dotnet/.claude-plugin/CLAUDE.md`
  parity outcome: aligned
  differences found: none; file remains a claude-mem placeholder and does not introduce runtime-specific OpenTelemetry, aspire, masstransit, or orchestrator semantics.
  allowed gap: none
- adapter path: `skills/opentelemetry-dotnet/skills/opentelemetry-dotnet/CLAUDE.md`
  parity outcome: aligned
  differences found: none; file remains a claude-mem placeholder and leaves canonical scope, OTLP-first guidance, and boundary rules in `SKILL.md`.
  allowed gap: none

## Baseline And Status

- verification date: 2026-03-11
- baseline: canonical skill verified against OpenTelemetry .NET package baseline `1.12.0`, current Microsoft .NET observability guidance, and current OpenTelemetry Collector docs
- status: audit-ready
- reference quality: focused official references are grouped by signals/conventions, exporters/Collector, and troubleshooting rather than bundled into one generic list
- completion proof: validation evidence covers links verified, boundary proof, adapter parity, baseline notes, and known limitations in one place

## Limitations

- Adapter files are intentionally thin and do not duplicate canonical semantics, so reviewers must use `SKILL.md` as the source of truth for scope and handoff rules.
- Link verification confirms liveness on 2026-03-11, but future OpenTelemetry or Microsoft doc reorganizations may require URL refreshes during later validation passes.
