# Phase 1: Shared Skill Standard - Context

**Gathered:** 2026-03-11
**Status:** Ready for planning

<domain>
## Phase Boundary

Establish the canonical skill package structure, validation expectations, reference policy, portability rules, and orchestrator handoff model used by all v1 skills. This phase defines the standard that later specialist skills must follow; it does not add new specialist domains beyond that standard.

</domain>

<decisions>
## Implementation Decisions

### Canonical skill package shape
- Every new v1 skill must ship as a full package: `SKILL.md`, `references/`, `examples/`, and `validation/`.
- `SKILL.md` uses a fixed section order so agents can rely on predictable structure across the repo.
- Reference material should be split into focused topic files under `references/`, not collapsed into one large catch-all file.
- Canonical examples should live in `examples/` so `SKILL.md` stays concise and operational.

### Handoff and orchestration rules
- A specialist may hand directly to another specialist only when there is exactly one clear next owner.
- If multiple specialties are implicated, the specialist must stop and return control to the orchestrator instead of sequencing work itself.
- Every handoff must name the exact next owner or explicitly say to return to the orchestrator.
- Every handoff must include a structured summary: current objective, completed work, unresolved risks, and the exact next decision or question.

### Validation standard
- Every new v1 skill must include at least two canonical examples: one happy-path example and one boundary or handoff example.
- Validation should prefer executable examples where practical, but clearly labeled pseudo-code is acceptable when execution is not practical.
- A skill is not considered complete without a validation checklist plus explicit evidence such as compile/test/link checks or a stated limitation.
- Official documentation links are part of validation and dead links should be treated as a validation failure.

### Adapter and portability rules
- `SKILL.md` is the canonical source of truth for each skill.
- Runtime-specific adapters are optional but should follow a standardized shape when present.
- Phase 1 should explicitly recognize `.claude-plugin/` and nested `skills/<skill>/CLAUDE.md` as supported adapter patterns.
- Adapters may reformat or restate content for a runtime, but they must not introduce semantic drift from the canonical `SKILL.md`.
- A skill without adapters is still valid if its canonical `SKILL.md` is complete.

### Claude's Discretion
- Exact file naming conventions inside `examples/` and `validation/`
- Whether the standard is captured as one central document, per-skill checklist, or both
- How strongly to distinguish mandatory versus recommended subsections within the fixed `SKILL.md` order

</decisions>

<specifics>
## Specific Ideas

- Treat the repo like a distributed specialist team, not a set of generic prompt docs.
- The standard should optimize for mixed AI-agent portability rather than Codex-only behavior.
- Keep the standard focused and opinionated so downstream skills produce reliable outputs and know when to stop.

</specifics>

<code_context>
## Existing Code Insights

### Reusable Assets
- Existing `SKILL.md` pattern in `aspire` and `masstransit`: strong starting point for canonical file shape and front matter usage.
- Existing `references/` directories in both skills: can be normalized into the Phase 1 standard rather than introduced from scratch.
- Existing `.claude-plugin/` assets: provide a concrete runtime-adapter pattern that the standard can formalize.

### Established Patterns
- `aspire` already includes both `.claude-plugin/` and nested `skills/aspire/CLAUDE.md`, which shows one fuller adapter pattern already present in the repo.
- `masstransit` includes `.claude-plugin/` but no nested CLAUDE skill folder, which shows current adapter inconsistency that Phase 1 should resolve.
- Neither current skill has dedicated `examples/` or `validation/` directories, so Phase 1 will likely introduce new package expectations instead of only documenting existing practice.

### Integration Points
- Phase 1 should set rules that later phases apply directly when creating `opentelemetry-dotnet`, `efcore`, `csharp-minimal-api`, and `xunit-integration-testing`.
- The standard must remain compatible with existing `aspire` and `masstransit` skills so future cleanup can align them without rewriting the project model.

</code_context>

<deferred>
## Deferred Ideas

- `frontend-implementation` remains a v1.x phase, not part of Phase 1.
- Generated adapters and automated conformance tooling are deferred beyond this phase.
- Additional runtime adapter shapes beyond the currently observed Claude-related patterns can be added later if needed.

</deferred>

---

*Phase: 01-shared-skill-standard*
*Context gathered: 2026-03-11*
