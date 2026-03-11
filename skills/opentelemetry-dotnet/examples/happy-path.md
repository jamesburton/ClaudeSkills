# Happy-Path Example

## Scenario

The user has an Aspire AppHost running an API service, a worker, and supporting infrastructure. They want
the shared observability baseline to emit traces, metrics, and logs with stable service identity and an
OTLP-first export path.

## Expected Specialist Behavior

- Define telemetry policy before diving into AppHost composition details.
- Require each service to emit traces, metrics, and logs with a stable `service.name`.
- Keep resource identity aligned across the API and worker so cross-service traces are readable.
- Recommend OTLP export as the default path and call out the collector as the preferred production route.
- Stop once the remaining work is purely Aspire runtime wiring or dashboard composition.

## Example Output

1. Service identity
   - API uses `service.name=orders-api`
   - Worker uses `service.name=orders-worker`
   - Both services share environment and version resource attributes

2. Signal baseline
   - traces enabled for inbound HTTP, outbound dependencies, and background work
   - metrics enabled for request latency, failure counts, and worker throughput
   - logs enriched for correlation with active trace context

3. Export path
   - local/default path: OTLP to Aspire dashboard or another local OTLP receiver
   - production-shaped path: OTLP to OpenTelemetry Collector, then fan-out to the chosen backend

4. Boundary note
   - `opentelemetry-dotnet` owns the telemetry policy above
   - if the next change is AppHost resource wiring, service defaults packaging, or dashboard lifecycle,
     hand directly to `aspire`

## Why This Is In Scope

The work is still about observability policy and signal coverage for a distributed .NET baseline. Aspire is
present, but the active decision is telemetry shape rather than hosting composition.
