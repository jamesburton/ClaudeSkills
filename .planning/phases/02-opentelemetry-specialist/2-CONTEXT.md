# Phase 2: OpenTelemetry Specialist - Context

**Gathered:** 2026-03-11
**Status:** Ready for planning

<domain>
## Phase Boundary

Add the `opentelemetry-dotnet` skill with realistic distributed-.NET observability guidance and explicit boundaries with `aspire`, `masstransit`, and orchestration. This phase defines the observability specialist itself; it does not absorb hosting, messaging behavior, or backend-specific platform ownership beyond what the skill must hand off or reference.

</domain>

<decisions>
## Implementation Decisions

### Skill ownership boundary
- `opentelemetry-dotnet` primarily owns instrumentation policy: tracing, metrics, logs, correlation, resource naming, semantic conventions, and exporter guidance.
- When observability work also touches Aspire, split by concern: `opentelemetry-dotnet` owns telemetry policy while `aspire` owns AppHost, service-defaults wiring, and dashboard/runtime composition.
- When observability work touches messaging, `opentelemetry-dotnet` owns signal shape and correlation while `masstransit` owns consumers, retries, middleware behavior, and transport decisions.
- Phase 1 handoff rules apply exactly: direct handoff only when there is one clear next owner; otherwise return to the orchestrator.

### Coverage emphasis
- The skill should treat traces, metrics, and logs as first-class rather than making tracing the only primary path.
- Resource naming, service identity, and semantic conventions should have high emphasis.
- Troubleshooting should cover common failures and missing-correlation issues, but should not turn into a deep operations manual.
- Advanced topics such as sampling, collectors, exporters, and context propagation should be covered in meaningful depth rather than only mentioned in passing.

### Example strategy
- The primary happy-path example should be Aspire-based so the skill fits the existing repo baseline.
- The required boundary example should cross between `opentelemetry-dotnet`, `aspire`, and `masstransit`.
- Examples should be realistic but focused: production-shaped enough to be useful, without becoming full tutorials.
- Examples should lead the reader, with references deepening or branching from the example path.

### Exporter and backend neutrality
- The default export path should be OTLP-first.
- Vendor-specific backends should mostly live in references or variants rather than dominating the main skill body.
- The OpenTelemetry Collector should be treated as the recommended production path, but not the only supported shape.
- The skill should keep pragmatic neutrality: the core stays mostly vendor-neutral, but a small number of concrete backend examples in the main flow are acceptable if they materially improve clarity.

### Claude's Discretion
- Exact topic split across `references/` files
- Whether advanced topics live mostly in `SKILL.md` or are balanced between the main body and references
- Which concrete backend examples, if any, are worth naming in the canonical flow versus variant references

</decisions>

<specifics>
## Specific Ideas

- This skill should feel like a real peer to `aspire` and `masstransit`, not a generic OpenTelemetry cheat sheet.
- The most valuable boundary example is telemetry policy crossing into both hosting and messaging concerns.
- OTLP-first guidance should keep the skill portable while still allowing practical backend examples where they sharpen understanding.

</specifics>

<code_context>
## Existing Code Insights

### Reusable Assets
- `skills/_standard/` now defines the required package shape, section contract, boundary rules, templates, and validation expectations that Phase 2 should follow directly.
- `skills/aspire/` now includes examples and validation artifacts, which gives the OpenTelemetry skill a concrete peer package to mirror.
- `skills/masstransit/` now includes examples and validation artifacts, which gives the OpenTelemetry skill a second peer and a natural boundary integration point.

### Established Patterns
- Every new v1 skill should ship as a full package with `SKILL.md`, `references/`, `examples/`, and `validation/`.
- Canonical semantics belong in `SKILL.md`; adapters remain optional thin projections.
- The repo already models direct handoff versus orchestrator return, so Phase 2 should encode observability-specific ownership using the same rule set rather than inventing a new handoff style.

### Integration Points
- `opentelemetry-dotnet` should reference `aspire` when telemetry concerns cross into AppHost/service-defaults composition.
- `opentelemetry-dotnet` should reference `masstransit` when observability work crosses into consumer, transport, or middleware behavior.
- Later skills like `csharp-minimal-api` and `xunit-integration-testing` will likely consume the observability rules defined here, so Phase 2 should make signal ownership and evidence expectations explicit.

</code_context>

<deferred>
## Deferred Ideas

- Deep backend-specific platform guides should stay in references or later variants, not become the main skill body.
- Exporter/backend combinations beyond the OTLP-first default can be expanded later if they start pulling too much weight in the core flow.
- Broader observability operations playbooks are outside this phase unless they are needed to explain common failures in a focused way.

</deferred>

---

*Phase: 02-opentelemetry-specialist*
*Context gathered: 2026-03-11*
