# ClaudeSkills

## What This Is

ClaudeSkills is a curated library of focused, opinionated AI agent skills for distributed .NET development. It is intended for mixed AI-agent environments, where individual specialist skills should produce reliable outputs inside their own domain, hand off cleanly when work crosses boundaries, and return control to an orchestrator when coordination belongs at the fleet level.

## Core Value

Each skill should reliably drive high-quality work inside a narrow specialty while making explicit, correct handoff decisions at domain boundaries.

## Requirements

### Validated

(None yet — ship to validate)

### Active

- [ ] Add an `opentelemetry-dotnet` skill with practical guidance, examples, and references for observability in distributed .NET systems.
- [ ] Add an `efcore` skill with opinionated guidance for persistence, migrations, and patterns commonly needed in distributed application work.
- [ ] Add an `xunit-integration-testing` skill covering realistic integration testing patterns for distributed .NET services and infrastructure.
- [ ] Add a `csharp-minimal-api` skill focused on endpoint design, binding, validation, authentication, OpenAPI, and reliable response patterns.
- [ ] Add a `frontend-implementation` skill focused on UI implementation quality, practical frontend delivery, and clear boundaries with backend and design-oriented work.
- [ ] Establish a consistent skill standard for examples, official-doc references, handoff rules, and output reliability guidance across all skills in the repo.

### Out of Scope

- Broad, generic programming skills — this project is for narrow specialist skills with clear boundaries.
- Human-first tutorial content — these skills should optimize for agent execution first, even if humans can still read them.
- One giant full-stack skill — specialization and explicit routing are preferred over a monolithic instruction set.

## Context

The repository already contains two distributed-.NET oriented skills: `aspire` and `masstransit`. The next milestone extends that foundation into adjacent specialties that commonly appear in the same systems: observability, persistence, integration testing, API endpoint implementation, and frontend delivery.

The intended operating model is agentic rather than purely documentary. Each skill should be opinionated enough to reduce ambiguity, include examples that anchor expected outputs, cite known documentation sources, and define when to continue, when to hand off to another specialist, and when to escalate back to an orchestrator managing multiple agents.

The audience is mixed AI agents rather than Codex alone, so portability matters. Instructions should avoid toolchain assumptions where possible while still being concrete enough to produce reliable outputs in practice.

## Constraints

- **Scope**: Skills must stay narrowly focused — broad skills blur responsibility and weaken handoff quality.
- **Reliability**: Each skill must include examples, references, and explicit guidance — the goal is predictable output quality, not loose advice.
- **Portability**: Skills should work across mixed AI-agent environments — avoid unnecessary runtime-specific assumptions.
- **Consistency**: New skills should follow a shared structure and quality bar — this repo should feel like one system, not unrelated documents.
- **Architecture**: Skills must define when to hand off to another specialist or orchestrator — distributed agent work is a first-class use case.

## Key Decisions

| Decision | Rationale | Outcome |
|----------|-----------|---------|
| Build a specialist skill library for distributed .NET work | The existing repo already centers on this domain, and the proposed additions naturally extend it | — Pending |
| Optimize for mixed AI agents instead of a single runtime | The team will use these skills across agent environments, so portability matters | — Pending |
| Keep skills focused and opinionated | Narrow scopes and strong guidance improve reliability and delegation quality | — Pending |
| Require handoff guidance in each skill | Skills should know when work has crossed into another specialty or back to orchestration | — Pending |
| Include examples and known-doc references in every skill | Concrete examples and source-backed guidance increase output consistency | — Pending |

---
*Last updated: 2026-03-11 after initialization*
