# Exporters And Collector

## Purpose

Use this reference when the question is how telemetry leaves a .NET service, which exporter path to
prefer, and where the OpenTelemetry Collector fits.

## Current Baseline

- Review date: 2026-03-11
- .NET guidance baseline: Microsoft OTLP example guidance for current supported .NET releases
- OpenTelemetry .NET package baseline: `OpenTelemetry.Exporter.OpenTelemetryProtocol` 1.12.0
- Recommended production path baseline: OTLP-first with collector-mediated fan-out unless the deployment is intentionally simple

## Default Position

- Prefer OTLP as the default export protocol.
- Prefer the OpenTelemetry Collector as the normal production path when telemetry must be routed, transformed,
  sampled centrally, or sent to more than one backend.
- Direct exporter configuration is acceptable for simple local development, narrow proof-of-concept work, or
  intentionally backend-specific deployments with no extra routing needs.

## Collector Guidance

The collector is usually the right choice when any of these are true:

- more than one backend receives telemetry
- sampling or filtering policy should be centralized
- transport credentials or endpoint fan-out should stay out of app code
- multiple services need a uniform export path

The collector is not mandatory when:

- a single service sends directly to one backend in a small local or short-lived environment
- the repo already has an intentionally simple direct OTLP path and no broader routing concerns exist

## Export Strategy

1. Establish resource identity and signal coverage first.
2. Choose OTLP as the primary wire format unless an explicit platform constraint overrides it.
3. Decide whether the app exports directly to a backend or to a collector.
4. If the choice becomes a platform architecture decision involving hosting, secrets, or multi-service rollout,
   return to the orchestrator or hand to `aspire` if hosting/runtime composition is the single clear next owner.

## Common Patterns

### Local Development

- App exports via OTLP directly to Aspire dashboard, local collector, or another local backend.
- Keep configuration simple and visible.

### Production-Like Environment

- App exports OTLP to collector.
- Collector handles fan-out, auth, endpoint routing, and backend-specific export behavior.
- App code remains mostly vendor-neutral.

## Vendor Neutrality

- Keep canonical skill guidance backend-neutral.
- Name concrete backends only when the example materially clarifies the OTLP or collector path.
- Do not let backend-specific setup dominate the main skill body.

## Official Sources

- OpenTelemetry .NET getting started: https://opentelemetry.io/docs/languages/dotnet/getting-started/
- Microsoft .NET OTLP example: https://learn.microsoft.com/en-us/dotnet/core/diagnostics/observability-otlp-example
- OpenTelemetry Collector docs: https://opentelemetry.io/docs/collector/
- OpenTelemetry Collector deployment docs: https://opentelemetry.io/docs/collector/deployment/
