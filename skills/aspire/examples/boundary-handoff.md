# Boundary / Handoff Example

## Scenario

The user asks for AppHost wiring plus detailed OpenTelemetry instrumentation and MassTransit consumer retry policy changes.

## Expected Decision

- Keep Aspire resource and AppHost guidance in scope.
- Name `opentelemetry-dotnet` as the next owner for instrumentation.
- Name `masstransit` as the next owner for consumer/retry policy.
- Return to orchestrator because more than one next owner is involved.

## Handoff Payload

- current objective: align the distributed app setup across hosting, telemetry, and messaging
- completed work: defined the Aspire resource graph and AppHost composition guidance
- unresolved risks: instrumentation and messaging behavior remain undefined
- next decision or question: should telemetry or messaging be resolved first

## Why The Current Skill Stops

Instrumentation and broker behavior cross out of Aspire’s bounded specialty.

