# Boundary / Handoff Example

## Scenario

The user asks for one change that includes:

- adding OpenTelemetry coverage for an API and worker
- updating Aspire service defaults and AppHost wiring so local OTLP endpoints are injected correctly
- changing a MassTransit consumer so message retries, headers, and correlation behavior match the telemetry plan

## Expected Decision

- Keep telemetry policy in scope for `opentelemetry-dotnet`.
- Define the observability baseline first: service identity, traces, metrics, logs, propagation, and OTLP path.
- Name `aspire` as a next owner for AppHost and service-defaults wiring.
- Name `masstransit` as a next owner for consumer and retry behavior.
- Return to the orchestrator because more than one next owner remains.

## Handoff Payload

- current objective: align telemetry policy, Aspire runtime wiring, and MassTransit message-flow behavior for one distributed workflow
- completed work: defined resource identity, signal expectations, correlation requirements, and OTLP-first export guidance
- unresolved risks: AppHost composition and messaging behavior still need owner-specific decisions
- next decision or question: should hosting/runtime wiring or messaging behavior be resolved first

## Ownership Split

- `opentelemetry-dotnet` owns telemetry policy and correlation requirements
- `aspire` owns AppHost, service defaults, runtime wiring, and local dashboard composition
- `masstransit` owns consumer behavior, retry/redelivery policy, and transport-facing message flow
- the orchestrator decides sequencing because there is no single clear next owner

## Why The Current Skill Stops

The remaining work crosses two peer specialties. The boundary rule requires direct handoff only when one
next owner is clear; otherwise control returns to the orchestrator.
