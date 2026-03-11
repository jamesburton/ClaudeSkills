# Requirements: ClaudeSkills

**Defined:** 2026-03-11
**Core Value:** Each skill should reliably drive high-quality work inside a narrow specialty while making explicit, correct handoff decisions at domain boundaries.

## v1 Requirements

### Skill Standard

- [ ] **STND-01**: Maintainer can create a new skill using a canonical structure that includes scope, boundaries, workflow, examples, references, validation, and handoff rules.
- [ ] **STND-02**: Agent can determine from each skill what work is in scope, what is out of scope, and when control must return to an orchestrator.
- [ ] **STND-03**: Agent can find at least one realistic example and one boundary or handoff example in each new v1 skill.
- [ ] **STND-04**: Maintainer can follow a consistent policy for official-doc references, version stamping, and validation evidence across all v1 skills.

### Observability

- [ ] **OBS-01**: Agent can use `opentelemetry-dotnet` to add or review tracing, metrics, and logging guidance for distributed .NET services.
- [ ] **OBS-02**: Agent can identify when observability work belongs in `opentelemetry-dotnet` versus `aspire`, `masstransit`, or the orchestrator.

### Persistence

- [ ] **DATA-01**: Agent can use `efcore` to guide DbContext design, migrations, querying, and persistence patterns for distributed .NET services.
- [ ] **DATA-02**: Agent can identify when persistence work should hand off from `efcore` to `masstransit`, `csharp-minimal-api`, or the orchestrator.

### API

- [ ] **API-01**: Agent can use `csharp-minimal-api` to guide endpoint design, binding, validation, authentication, OpenAPI, and response patterns for ASP.NET Core Minimal APIs.
- [ ] **API-02**: Agent can identify when API tasks require handoff to `efcore`, `opentelemetry-dotnet`, `frontend-implementation`, or the orchestrator.

### Testing

- [ ] **TEST-01**: Agent can use `xunit-integration-testing` to design or review realistic integration tests for distributed .NET services and infrastructure.
- [ ] **TEST-02**: Agent can determine what validation evidence a specialist skill should return and when real infrastructure or fixtures are required.

### Portability

- [ ] **PORT-01**: Maintainer can keep `SKILL.md` as the canonical source while providing thin manual runtime-specific adapter files without changing the skill’s meaning.
- [ ] **PORT-02**: Agent can follow v1 skills in mixed-agent environments without depending on Codex-only workflow assumptions.

## v2 Requirements

### Frontend

- **UI-01**: Agent can use `frontend-implementation` to build and refine UI behavior while respecting API contract ownership and backend specialist boundaries.
- **UI-02**: Agent can determine when frontend work should escalate because the API contract, validation behavior, or auth flow is unstable or missing.

### Automation

- **AUTO-01**: Maintainer can generate runtime-specific adapter files from canonical skill docs.
- **AUTO-02**: Maintainer can run automated conformance checks across all skills for structure, links, validation notes, and handoff completeness.

## Out of Scope

| Feature | Reason |
|---------|--------|
| Monolithic full-stack skill | Conflicts with the core value of bounded specialist ownership |
| Human-first tutorial-style docs | Reduces agent execution clarity and output reliability |
| Runtime-specific logic in canonical skill docs | Undermines the mixed-agent portability goal |
| Additional specialist domains beyond current v1 set | Keep the first milestone focused on the shared standard and four core adjacent backend skills |

## Traceability

| Requirement | Phase | Status |
|-------------|-------|--------|
| STND-01 | Phase 1 | Pending |
| STND-02 | Phase 1 | Pending |
| STND-03 | Phase 1 | Pending |
| STND-04 | Phase 1 | Pending |
| OBS-01 | Phase 2 | Pending |
| OBS-02 | Phase 2 | Pending |
| DATA-01 | Phase 3 | Pending |
| DATA-02 | Phase 3 | Pending |
| API-01 | Phase 4 | Pending |
| API-02 | Phase 4 | Pending |
| TEST-01 | Phase 5 | Pending |
| TEST-02 | Phase 5 | Pending |
| PORT-01 | Phase 1 | Pending |
| PORT-02 | Phase 1 | Pending |

**Coverage:**
- v1 requirements: 14 total
- Mapped to phases: 14
- Unmapped: 0 ✓

---
*Requirements defined: 2026-03-11*
*Last updated: 2026-03-11 after initial definition*
