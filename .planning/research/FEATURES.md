# Feature Research

**Domain:** Mixed-agent distributed .NET skill pack
**Researched:** 2026-03-11
**Confidence:** HIGH

## Feature Landscape

### Table Stakes (Users Expect These)

Features users assume exist. Missing these = product feels incomplete.

| Feature | Why Expected | Complexity | Notes |
|---------|--------------|------------|-------|
| Narrow specialist scope per skill | The project promise is focused specialist skills, not generic coding advice | MEDIUM | Each skill needs a crisp boundary, domain vocabulary, and clear non-goals so agents do not overreach |
| Opinionated implementation guidance | Agents need concrete direction to produce repeatable output in .NET delivery work | MEDIUM | Guidance should prefer known patterns, name common pitfalls, and remove ambiguous “it depends” branches |
| Canonical examples and references | Without examples and official-doc anchors, skills drift into unreliable paraphrase | MEDIUM | Every skill should include small, realistic snippets and a short official-doc reference set |
| Explicit handoff and escalation rules | Mixed-agent environments fail when a specialist keeps working past its boundary | HIGH | Each skill should state when to continue, when to hand off to another skill, and when to return to the orchestrator |
| Shared skill structure and quality bar | Users will expect all skills in the repo to feel consistent once more than two exist | MEDIUM | A standard section order, example style, and dependency language is needed before scaling the pack |
| Coverage of core distributed-.NET adjacent domains | The roadmap already points to observability, persistence, testing, API, and frontend as expected complements to Aspire and MassTransit | HIGH | These are the table-stakes adjacent specialties for real distributed application work, not optional extras |

### Differentiators (Competitive Advantage)

Features that set the product apart. Not required, but valuable.

| Feature | Value Proposition | Complexity | Notes |
|---------|-------------------|------------|-------|
| Mixed-agent portability | Skills remain useful across Codex, Claude, and other agent runtimes instead of assuming one execution model | HIGH | Requires runtime-neutral wording, avoiding vendor-only tooling assumptions, and documenting outputs rather than UI-specific behavior |
| Boundary-aware handoff design | The pack can behave like a distributed specialist team instead of isolated documents | HIGH | Strongest differentiator if every skill names upstream/downstream peers such as `aspire`, `masstransit`, `efcore`, and `opentelemetry-dotnet` |
| Distributed-system-first examples | Examples tuned to AppHost wiring, messaging, persistence, telemetry, and integration testing make the skills materially more useful than generic framework docs | HIGH | The examples need to show interaction patterns across service boundaries, not just single-project snippets |
| Reliability-focused output guidance | Skills that specify expected deliverables, validation checks, and common failure modes reduce agent variance | MEDIUM | This turns the repo from “reference notes” into an execution system for agents |
| Cross-skill consistency across backend and frontend work | Including frontend implementation as a bounded specialist prevents “backend-only” blind spots in distributed product delivery | MEDIUM | Valuable because UI work is often the weak link in otherwise solid distributed .NET stacks |

### Anti-Features (Commonly Requested, Often Problematic)

Features that seem good but create problems.

| Feature | Why Requested | Why Problematic | Alternative |
|---------|---------------|-----------------|-------------|
| One giant full-stack skill | Feels simpler to ask one skill to handle everything | Destroys specialist boundaries, weakens handoffs, and makes outputs generic and inconsistent | Keep skills narrow and route through orchestrator-level coordination |
| Human-first tutorial prose | Seems easier to read and share | Bloats tokens, hides decision points, and reduces execution clarity for agents | Favor concise execution guidance with examples and references |
| Runtime-specific workflow assumptions | Appears convenient if the author uses one agent platform | Makes the pack less portable and undermines the stated mixed-agent goal | Describe expected inputs, outputs, and decisions in runtime-neutral language |
| Exhaustive framework coverage inside one skill | Users often ask for a “complete reference” | Encourages shallow breadth over reliable depth and creates maintenance drag | Cover the high-signal patterns for one specialty and hand off the rest |
| Auto-orchestration hidden inside specialist skills | Feels efficient if a skill silently coordinates other domains | Confuses ownership and makes failure recovery opaque | Make orchestration an explicit handoff to the calling agent or orchestrator |

## Feature Dependencies

```text
[Shared skill standard]
    └──requires──> [Consistent section template]
                       └──requires──> [Examples + official references]

[Boundary-aware handoff design]
    └──requires──> [Narrow specialist scope per skill]
                       └──requires──> [Domain-specific terminology]

[Distributed-system-first examples]
    └──requires──> [Existing distributed-.NET baseline]
                       └──requires──> [Aspire skill + MassTransit skill]

[Mixed-agent portability] ──enhances──> [Shared skill standard]
[Reliability-focused output guidance] ──enhances──> [Boundary-aware handoff design]
[One giant full-stack skill] ──conflicts──> [Narrow specialist scope per skill]
[Runtime-specific workflow assumptions] ──conflicts──> [Mixed-agent portability]
```

### Dependency Notes

- **Shared skill standard requires consistent section template:** The pack cannot scale cleanly unless each skill presents prerequisites, concepts, examples, references, and handoff rules in the same order.
- **Consistent section template requires examples + official references:** Structure alone is not enough; reliability depends on anchored examples and trusted source material.
- **Boundary-aware handoff design requires narrow specialist scope per skill:** Handoffs only work when each skill has a clear stopping point.
- **Narrow specialist scope per skill requires domain-specific terminology:** Skills need precise vocabulary to recognize when work is in or out of scope.
- **Distributed-system-first examples require existing distributed-.NET baseline:** The current `aspire` and `masstransit` skills are the concrete foundation the next skills should align with.
- **Mixed-agent portability enhances shared skill standard:** Portability pressure forces the standard to describe decisions and deliverables clearly instead of relying on tool-specific behavior.
- **Reliability-focused output guidance enhances boundary-aware handoff design:** A skill that names what “done” looks like can hand off cleanly with less ambiguity.
- **One giant full-stack skill conflicts with narrow specialist scope per skill:** Combining domains would directly violate the project’s core value.
- **Runtime-specific workflow assumptions conflict with mixed-agent portability:** Agent-specific instructions reduce reuse across the intended environments.

## MVP Definition

### Launch With (v1)

Minimum viable product — what's needed to validate the concept.

- [ ] Shared skill standard for structure, examples, references, and handoff rules — validates that the repo can scale coherently beyond the first two skills
- [ ] `opentelemetry-dotnet` skill — essential for observability, which is a core concern in distributed systems
- [ ] `efcore` skill — essential for persistence and migrations, which most service-based systems need immediately
- [ ] `xunit-integration-testing` skill — essential for validating multi-service behavior, not just isolated code units
- [ ] `csharp-minimal-api` skill — essential for the common API delivery layer around distributed services
- [ ] Boundary definitions linking all skills back to `aspire`, `masstransit`, peers, and orchestrator escalation — essential for the mixed-agent operating model

### Add After Validation (v1.x)

Features to add once core is working.

- [ ] `frontend-implementation` skill — add once backend-specialist patterns are stable and the repo needs stronger UI delivery coverage
- [ ] Cross-skill dependency map or routing matrix — add once enough skills exist that handoff paths become non-trivial
- [ ] Skill quality checklist or linting workflow — add once maintenance overhead appears across multiple contributors or agents

### Future Consideration (v2+)

Features to defer until product-market fit is established.

- [ ] Additional infrastructure specialists such as identity, caching, or deployment pipeline skills — defer until the first adjacent set proves the specialization model
- [ ] Generated test suites or automated skill conformance validation — defer until the manual quality bar is established
- [ ] Multi-language or non-.NET distributed skill packs — defer because the project’s current differentiation is depth in distributed .NET work

## Feature Prioritization Matrix

| Feature | User Value | Implementation Cost | Priority |
|---------|------------|---------------------|----------|
| Shared skill standard with examples, references, and handoff rules | HIGH | MEDIUM | P1 |
| `opentelemetry-dotnet` skill | HIGH | MEDIUM | P1 |
| `efcore` skill | HIGH | MEDIUM | P1 |
| `xunit-integration-testing` skill | HIGH | MEDIUM | P1 |
| `csharp-minimal-api` skill | HIGH | MEDIUM | P1 |
| Mixed-agent portability guidelines | HIGH | HIGH | P1 |
| `frontend-implementation` skill | MEDIUM | MEDIUM | P2 |
| Cross-skill routing matrix | MEDIUM | MEDIUM | P2 |
| Automated skill quality validation | MEDIUM | HIGH | P3 |

**Priority key:**
- P1: Must have for launch
- P2: Should have, add when possible
- P3: Nice to have, future consideration

## Competitor Feature Analysis

| Feature | Competitor A | Competitor B | Our Approach |
|---------|--------------|--------------|--------------|
| Specialist guidance depth | Official framework docs are authoritative but broad and human-oriented | Generic agent prompts/rules are flexible but often shallow and inconsistent | Provide narrow, opinionated specialist skills with concrete examples and practical defaults |
| Cross-domain coordination | Framework docs rarely define when to hand off to another specialist domain | Single-agent prompt packs often encourage one model to do everything | Treat handoff and orchestrator escalation as first-class behavior |
| Portability across agent runtimes | Vendor-specific docs assume their own ecosystem and workflows | Many community prompt packs are tuned to one agent product | Keep instructions runtime-neutral so the same skill can travel across mixed-agent environments |
| Distributed-system realism | Many references teach isolated library usage | Generic coding assistants produce examples divorced from AppHost, messaging, persistence, and telemetry interactions | Center examples on real distributed .NET service interactions built around the existing repo baseline |

## Sources

- `C:\Development\ClaudeSkills\.planning\PROJECT.md`
- `C:\Development\ClaudeSkills\skills\aspire\SKILL.md`
- `C:\Development\ClaudeSkills\skills\masstransit\SKILL.md`
- `C:\Users\james\.codex\get-shit-done\templates\research-project\FEATURES.md`
- Observed repository structure in `C:\Development\ClaudeSkills`

---
*Feature research for: Mixed-agent distributed .NET skill pack*
*Researched: 2026-03-11*
