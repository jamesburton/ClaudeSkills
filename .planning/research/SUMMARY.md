# Project Research Summary

**Project:** ClaudeSkills
**Domain:** Portable specialist AI-agent skill library for distributed .NET development
**Researched:** 2026-03-11
**Confidence:** HIGH

## Executive Summary

Research converges on a clear direction: this project should behave like a distributed specialist team, not a loose collection of prompt notes. The right approach is a docs-first, portable skill library where each skill owns one bounded domain, carries a consistent execution contract, includes realistic examples and official references, and hands work back when the task crosses into another specialist area or requires orchestrator-level coordination.

The most important launch requirement is not just adding more topics. The foundation is a shared skill standard that defines structure, boundaries, examples, validation, references, and handoff behavior across the entire repo. On top of that standard, the strongest first-wave skills are `opentelemetry-dotnet`, `efcore`, `csharp-minimal-api`, and `xunit-integration-testing`, all aligned to the existing `aspire` and `masstransit` baseline.

The biggest risks are boundary drift, human-first prose, hidden runtime assumptions, and inconsistent architectural defaults across skills. The roadmap should therefore start with a cross-skill standard before individual skill authoring, then sequence domain skills in dependency order, and reserve frontend specialization for a later phase unless the user explicitly wants it in v1.

## Key Findings

### Recommended Stack

The canonical source format should remain `SKILL.md` in Markdown with YAML front matter, with runtime-specific adapters kept thin and derivative. New examples and validation fixtures should standardize on `.NET 10` LTS, `EF Core 10`, `OpenTelemetry .NET 1.x`, `xUnit.net v3`, and Minimal APIs as the default HTTP edge style.

**Core technologies:**
- Markdown + YAML front matter: canonical portable skill format — readable, diffable, and runtime-neutral
- .NET 10 / EF Core 10 / OpenTelemetry 1.x / xUnit v3: example and validation baseline — modern distributed-.NET stack alignment
- `references/`, `examples/`, `validation/`: required support artifacts — make skills operational instead of purely descriptive

### Expected Features

Research says users will expect every skill to have narrow ownership, opinionated guidance, realistic examples, official-doc anchors, and explicit handoff rules. The real differentiator is mixed-agent portability plus boundary-aware routing, so the library acts coherently across different agent runtimes instead of assuming one execution environment.

**Must have (table stakes):**
- Shared skill standard with examples, references, validation, and handoff sections — users will expect consistency across the repo
- `opentelemetry-dotnet`, `efcore`, `csharp-minimal-api`, and `xunit-integration-testing` — core adjacent specialties for distributed .NET delivery
- Cross-skill boundary definitions tied back to `aspire`, `masstransit`, peer skills, and orchestrator escalation — required for the mixed-agent operating model

**Should have (competitive):**
- Mixed-agent portability language — keeps the skill pack usable beyond one runtime
- Distributed-system-first examples — makes the skills materially more useful than generic framework summaries

**Defer (v2+):**
- `frontend-implementation` if strict MVP scope is needed
- Additional infrastructure specialists such as identity, deployment, or caching
- Automated conformance tooling beyond the initial manual quality bar

### Architecture Approach

The skill package is the primary architectural unit. Each package should contain canonical instructions, references, examples, validation guidance, and optional runtime metadata, while the orchestrator owns sequencing, conflict resolution, and cross-skill coordination. Project-level planning artifacts should define the shared standard before more skill folders are added so that all future skills align to the same operating model.

**Major components:**
1. Shared skill standard — defines structure, boundaries, validation, references, and handoff rules
2. Specialist skill packages — one bounded domain per folder with aligned artifact structure
3. Orchestrator interaction model — explicit rules for returning control or suggesting the next specialist

### Critical Pitfalls

1. **Boundary drift between skills** — require strict scope, non-goals, and handoff triggers in every skill
2. **Human-first tutorial writing** — make skills operational documents with decision rules and validation, not prose-heavy explanations
3. **Hidden runtime assumptions** — separate portable domain guidance from runtime-specific adapters and convenience steps
4. **Shallow examples** — require realistic distributed-.NET examples, not toy snippets
5. **Conflicting defaults across skills** — define the shared architectural spine first, then author domain skills against it

## Implications for Roadmap

Based on research, suggested phase structure:

### Phase 1: Shared Skill Standard
**Rationale:** Prevents inconsistency and boundary drift before more skills are added
**Delivers:** Shared structure, validation expectations, reference policy, and handoff/orchestrator rules
**Addresses:** Table-stakes consistency and routing requirements
**Avoids:** Boundary drift, tutorial-style skills, conflicting defaults

### Phase 2: Observability and Persistence Skills
**Rationale:** `opentelemetry-dotnet` and `efcore` are foundational adjacent domains for the existing distributed-.NET baseline
**Delivers:** Two high-value specialist skills with realistic examples and references
**Uses:** `.NET 10`, `EF Core 10`, `OpenTelemetry .NET 1.x`
**Implements:** Core backend specialist package pattern

### Phase 3: API Skill
**Rationale:** Minimal API guidance should inherit the standard and align with persistence/telemetry boundaries
**Delivers:** `csharp-minimal-api` skill with explicit contract and handoff rules
**Uses:** ASP.NET Core Minimal APIs and OpenAPI baseline
**Implements:** HTTP edge skill and API contract ownership

### Phase 4: Integration Testing Skill
**Rationale:** Verification guidance should land after core backend specialist patterns exist so it can reference them coherently
**Delivers:** `xunit-integration-testing` skill with realistic infrastructure-backed test patterns
**Uses:** xUnit v3 and Testcontainers
**Implements:** Cross-cutting verification layer

### Phase 5: Frontend Implementation Skill
**Rationale:** UI work depends on clear API contract ownership and should arrive after backend specialist boundaries are stable
**Delivers:** `frontend-implementation` skill aligned to backend contracts and escalation rules
**Uses:** Existing project frontend stack or a portable UI implementation pattern
**Implements:** UI specialist package with contract-consumption rules

### Phase Ordering Rationale

- The shared standard must come first because every later skill depends on it for consistency and routing behavior.
- Observability and persistence are the strongest immediate complements to the existing `aspire` and `masstransit` skills.
- API guidance should follow once backend domain boundaries are explicit.
- Integration testing should verify the combined backend patterns rather than define them in isolation.
- Frontend should follow API contract ownership unless product pressure makes it part of MVP.

### Research Flags

Phases likely needing deeper research during planning:
- **Phase 2:** Confirm current OpenTelemetry and EF Core API/version guidance stays aligned with official docs
- **Phase 5:** Frontend implementation details depend on whether the repo wants stack-specific or stack-neutral UI guidance

Phases with standard patterns:
- **Phase 1:** Shared documentation/quality standard is largely repo-internal and straightforward to define
- **Phase 3:** Minimal API patterns are well-established once project boundaries are fixed
- **Phase 4:** xUnit/Testcontainers-based integration testing has standard modern patterns

## Confidence Assessment

| Area | Confidence | Notes |
|------|------------|-------|
| Stack | HIGH | Strong convergence on portable docs-first structure and modern distributed-.NET example baseline |
| Features | HIGH | User goals and repo context align with the table-stakes/differentiator split |
| Architecture | HIGH | The repo already behaves like a skill library and research sharpened the specialist/orchestrator boundaries |
| Pitfalls | HIGH | Risks are concrete, domain-specific, and directly actionable in roadmap sequencing |

**Overall confidence:** HIGH

### Gaps to Address

- Frontend scope: decide whether `frontend-implementation` is part of v1 or a post-validation addition
- Adapter strategy: decide whether runtime-specific adapter files will be manually maintained or generated from canonical skill docs

## Sources

### Primary (HIGH confidence)
- [C:\Development\ClaudeSkills\.planning\research\STACK.md](C:\Development\ClaudeSkills\.planning\research\STACK.md)
- [C:\Development\ClaudeSkills\.planning\research\FEATURES.md](C:\Development\ClaudeSkills\.planning\research\FEATURES.md)
- [C:\Development\ClaudeSkills\.planning\research\ARCHITECTURE.md](C:\Development\ClaudeSkills\.planning\research\ARCHITECTURE.md)
- [C:\Development\ClaudeSkills\.planning\research\PITFALLS.md](C:\Development\ClaudeSkills\.planning\research\PITFALLS.md)

### Secondary (MEDIUM confidence)
- [C:\Development\ClaudeSkills\skills\aspire\SKILL.md](C:\Development\ClaudeSkills\skills\aspire\SKILL.md)
- [C:\Development\ClaudeSkills\skills\masstransit\SKILL.md](C:\Development\ClaudeSkills\skills\masstransit\SKILL.md)

---
*Research completed: 2026-03-11*
*Ready for roadmap: yes*
