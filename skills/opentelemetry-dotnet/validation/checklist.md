# Validation Checklist

- [x] `SKILL.md` follows the fixed section order
- [x] `examples/happy-path.md` exists
- [x] `examples/boundary-handoff.md` exists
- [x] Official references are identified and reviewed
- [x] Adapter parity reviewed where adapters exist
- [x] Validation evidence recorded

## OpenTelemetry Skill Completion Checks

- [x] Canonical guidance treats traces, metrics, and logs as equal first-class signals
- [x] Resource identity guidance requires `service.name` and documents related resource attributes
- [x] OTLP-first export guidance and Collector recommendation path are explicit
- [x] Common failures cover missing telemetry, broken correlation, exporter reachability, and resource identity mistakes
- [x] Boundary rules explicitly distinguish `aspire`, `masstransit`, and orchestrator return behavior
- [x] Official references are grouped by signal/convention, exporter/collector, and troubleshooting topics
- [x] Validation evidence records official references, links verified, baseline notes, and boundary verification
