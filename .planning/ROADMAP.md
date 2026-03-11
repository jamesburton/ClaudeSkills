# Roadmap: ClaudeSkills

## Overview

This roadmap turns ClaudeSkills from a promising pair of distributed-.NET skills into a coherent specialist skill library. The sequence starts by locking the shared operating model, then adds one bounded specialist skill per phase so each addition inherits the same structure, validation bar, and handoff behavior.

## Phases

**Phase Numbering:**
- Integer phases (1, 2, 3): Planned milestone work
- Decimal phases (2.1, 2.2): Urgent insertions if needed later

- [x] **Phase 1: Shared Skill Standard** - Define the canonical structure, validation rules, and handoff model for all skills (completed 2026-03-11)
- [x] **Phase 2: OpenTelemetry Specialist** - Add the observability skill aligned to the standard and existing distributed-.NET baseline (completed 2026-03-11)
- [ ] **Phase 3: EF Core Specialist** - Add the persistence skill with clear boundaries against API, messaging, and orchestration concerns
- [ ] **Phase 4: Minimal API Specialist** - Add the endpoint-focused API skill with explicit contract and handoff rules
- [ ] **Phase 5: Integration Testing Specialist** - Add the verification skill for realistic distributed-.NET integration testing

## Phase Details

### Phase 1: Shared Skill Standard
**Goal**: Establish the canonical skill package structure, validation expectations, reference policy, portability rules, and orchestrator handoff model used by all v1 skills.
**Depends on**: Nothing (first phase)
**Requirements**: [STND-01, STND-02, STND-03, STND-04, PORT-01, PORT-02]
**Success Criteria** (what must be TRUE):
  1. Maintainer can follow one documented standard to create a new specialist skill with scope, boundaries, workflow, examples, references, validation, and handoff sections.
  2. Agent can determine from the standard when a specialist should continue, when it should hand off, and when it should return control to an orchestrator.
  3. The repo defines a canonical `SKILL.md` source plus a thin manual adapter strategy that preserves mixed-agent portability.
  4. The standard specifies what validation evidence and official references each v1 skill must include before it is considered complete.
**Plans**: 3 plans

Plans:
- [x] 01-01: Define the canonical specialist skill structure and required sections
- [x] 01-02: Define boundary, handoff, portability, and adapter rules
- [x] 01-03: Define examples, references, and validation standards for all v1 skills

### Phase 2: OpenTelemetry Specialist
**Goal**: Add the `opentelemetry-dotnet` skill with realistic distributed-.NET observability guidance and explicit boundaries with `aspire`, `masstransit`, and orchestration.
**Depends on**: Phase 1
**Requirements**: [OBS-01, OBS-02]
**Success Criteria** (what must be TRUE):
  1. Agent can use `opentelemetry-dotnet` to guide tracing, metrics, and logging work for distributed .NET services.
  2. The skill includes realistic examples, official references, and validation guidance aligned to the shared standard.
  3. Agent can tell when observability wiring belongs in `opentelemetry-dotnet` versus `aspire`, `masstransit`, or the orchestrator.
**Plans**: 2 plans

Plans:
- [x] 02-01: Author core OpenTelemetry guidance, examples, and references
- [x] 02-02: Add boundary, validation, and adapter material for the observability skill

### Phase 3: EF Core Specialist
**Goal**: Add the `efcore` skill for persistence design, migrations, and data access patterns in distributed .NET systems.
**Depends on**: Phase 2
**Requirements**: [DATA-01, DATA-02]
**Success Criteria** (what must be TRUE):
  1. Agent can use `efcore` to guide DbContext design, migrations, and persistence patterns for distributed .NET services.
  2. The skill includes realistic examples, official references, and validation guidance aligned to the shared standard.
  3. Agent can tell when persistence work should hand off to `masstransit`, `csharp-minimal-api`, or the orchestrator.
**Plans**: 2 plans

Plans:
- [ ] 03-01: Author EF Core workflow, examples, and reference set
- [ ] 03-02: Add boundary, validation, and adapter material for the persistence skill

### Phase 4: Minimal API Specialist
**Goal**: Add the `csharp-minimal-api` skill for endpoint implementation and HTTP contract ownership without absorbing persistence, telemetry, or UI concerns.
**Depends on**: Phase 3
**Requirements**: [API-01, API-02]
**Success Criteria** (what must be TRUE):
  1. Agent can use `csharp-minimal-api` to guide endpoint design, binding, validation, authentication, OpenAPI, and response patterns.
  2. The skill includes realistic examples, official references, and validation guidance aligned to the shared standard.
  3. Agent can tell when API work should hand off to `efcore`, `opentelemetry-dotnet`, `frontend-implementation`, or the orchestrator.
**Plans**: 2 plans

Plans:
- [ ] 04-01: Author Minimal API workflow, examples, and reference set
- [ ] 04-02: Add boundary, validation, and adapter material for the API skill

### Phase 5: Integration Testing Specialist
**Goal**: Add the `xunit-integration-testing` skill for realistic validation of distributed .NET services and infrastructure-backed workflows.
**Depends on**: Phase 4
**Requirements**: [TEST-01, TEST-02]
**Success Criteria** (what must be TRUE):
  1. Agent can use `xunit-integration-testing` to design or review distributed-.NET integration tests with realistic infrastructure expectations.
  2. The skill includes realistic examples, official references, and validation guidance aligned to the shared standard.
  3. Agent can tell what evidence specialist skills should return and when real infrastructure or fixtures are required.
**Plans**: 2 plans

Plans:
- [ ] 05-01: Author integration testing workflow, examples, and reference set
- [ ] 05-02: Add validation, evidence, boundary, and adapter material for the testing skill

## Progress

**Execution Order:**
Phases execute in numeric order: 1 → 2 → 3 → 4 → 5

| Phase | Plans Complete | Status | Completed |
|-------|----------------|--------|-----------|
| 1. Shared Skill Standard | 3/3 | Complete | 2026-03-11 |
| 2. OpenTelemetry Specialist | 2/2 | Complete | 2026-03-11 |
| 3. EF Core Specialist | 0/2 | Not started | - |
| 4. Minimal API Specialist | 0/2 | Not started | - |
| 5. Integration Testing Specialist | 0/2 | Not started | - |
