# Pitfalls Research

**Domain:** Specialist AI-agent skill libraries for distributed .NET work
**Researched:** 2026-03-11
**Confidence:** HIGH

## Critical Pitfalls

### Pitfall 1: Boundary Drift Between Specialist Skills

**What goes wrong:**
Skills expand beyond their specialty, start giving partial guidance on adjacent domains, and produce overlapping or contradictory instructions. Agents then keep working inside the wrong skill instead of handing off.

**Why it happens:**
Distributed .NET tasks naturally cross concerns like hosting, messaging, persistence, observability, and testing. Without explicit stop conditions, authors keep adding "helpful" adjacent guidance until specialization collapses.

**How to avoid:**
Require every skill to define entry criteria, explicit non-goals, handoff triggers, and examples of when to return to an orchestrator. Enforce a shared section for "continue here", "hand off", and "escalate" decisions.

**Warning signs:**
Skill descriptions mention multiple major subsystems, examples start spanning end-to-end stacks, or two skills both claim ownership of the same code paths or decisions.

**Phase to address:**
Phase 1 - Shared skill standard and routing rules

---

### Pitfall 2: Agent-Optimized Content Regresses Into Human Tutorial Style

**What goes wrong:**
Skills read well to humans but fail agents because they bury action rules inside prose, omit decision criteria, or lack concrete output patterns.

**Why it happens:**
Documentation habits favor explanation over execution. Authors often assume an agent will infer the correct next step from narrative guidance.

**How to avoid:**
Make skills operational documents first: short decision rules, canonical workflows, expected output shapes, minimal but complete examples, and escalation criteria. Prefer "if X, do Y" over descriptive paragraphs.

**Warning signs:**
Long explanatory sections without action steps, few examples of final outputs, or repeated use of words like "generally" and "consider" without concrete thresholds.

**Phase to address:**
Phase 1 - Shared skill standard and quality bar

---

### Pitfall 3: Portability Claims Hide Runtime-Specific Assumptions

**What goes wrong:**
A skill appears portable across mixed AI agents but silently assumes Codex-style tools, repo layout, or shell behavior. Other agents then fail or hallucinate missing capabilities.

**Why it happens:**
The author writes from one runtime environment and does not separate domain instructions from tool-specific convenience steps.

**How to avoid:**
Split core domain guidance from optional environment notes. Mark tooling-specific commands as examples, not requirements, and keep the main workflow valid with plain file reading, editing, and reasoning.

**Warning signs:**
Instructions depend on one shell, one editor, one tool namespace, or hidden filesystem conventions; failure reports cluster around "could not run" rather than domain decisions.

**Phase to address:**
Phase 1 - Portability requirements in the shared standard

---

### Pitfall 4: Shallow Examples Create False Confidence

**What goes wrong:**
Skills include toy snippets that look correct but do not represent realistic distributed .NET work. Agents copy them into production-shaped tasks and miss configuration, failure handling, or integration edges.

**Why it happens:**
Example creation is faster than scenario design. Authors optimize for brevity and omit the hard parts that actually distinguish specialist work.

**How to avoid:**
Require at least one realistic example per skill that covers the dominant failure path in that domain, such as telemetry correlation gaps, EF Core migration drift, or flaky integration test setup.

**Warning signs:**
Examples fit in a few lines, never show failure handling, omit surrounding configuration, or cannot be mapped to a real repository workflow.

**Phase to address:**
Phase 2 - Domain skill authoring for each specialist area

---

### Pitfall 5: Missing Official Reference Anchors

**What goes wrong:**
Skills become stale, opinionated in the wrong places, or internally inconsistent because they do not point agents back to authoritative .NET, framework, or library documentation.

**Why it happens:**
Authors try to make each skill self-contained and skip source curation. Over time, unsupported claims accumulate and version changes are missed.

**How to avoid:**
Require official doc references for every major subsystem, plus notes on where local opinion intentionally narrows or overrides broader upstream options.

**Warning signs:**
Claims without citations, outdated package names or version assumptions, and no distinction between framework facts and project conventions.

**Phase to address:**
Phase 1 - Shared references standard; Phase 2 - Per-skill source curation

---

### Pitfall 6: Handoff Rules Stop at Domain Borders But Ignore Orchestrator Control

**What goes wrong:**
A skill knows when it is out of scope but does not say whether to hand off directly to another specialist or return control to a coordinating agent. Multi-agent work then loops, duplicates effort, or deadlocks.

**Why it happens:**
Skill authors think in pairwise handoffs rather than fleet-level coordination. Distributed .NET tasks often require sequencing across multiple specialists, not just one transfer.

**How to avoid:**
Add explicit orchestration guidance: when the specialist may continue, when it must suggest the next skill, and when it must stop and report status back for coordination.

**Warning signs:**
Instructions say "use another skill" without naming ownership transfer rules, or agents repeatedly revisit the same files across specialties.

**Phase to address:**
Phase 1 - Orchestrator interaction rules

---

### Pitfall 7: Domain Skills Encode Conflicting Architectural Defaults

**What goes wrong:**
Different skills recommend incompatible patterns for the same distributed system, such as inconsistent naming, resilience assumptions, telemetry conventions, or test boundaries.

**Why it happens:**
Each skill is authored in isolation. Good local advice becomes bad system advice when the library lacks a common architectural spine.

**How to avoid:**
Define cross-skill defaults once: terminology, example solution shape, configuration philosophy, validation expectations, and what "production-ready" means in this repo.

**Warning signs:**
One skill assumes Aspire-managed startup while another assumes ad hoc wiring, or testing guidance contradicts persistence and messaging expectations.

**Phase to address:**
Phase 1 - Shared standard; Phase 3 - Cross-skill consistency review

---

### Pitfall 8: Testability Guidance Arrives Too Late

**What goes wrong:**
Skills teach implementation detail without telling agents how to prove the result works in a distributed environment. The library then produces code that looks plausible but is hard to verify.

**Why it happens:**
Testing is treated as a separate concern instead of a design constraint. In distributed .NET systems, observability, persistence, APIs, and messaging all need verifiable seams.

**How to avoid:**
Make verification a required section in every skill: what to test, what to stub, what must run against real infrastructure, and what evidence should be returned.

**Warning signs:**
Implementation examples end at "write the code", no validation checklist is included, or integration testing only appears in the dedicated testing skill.

**Phase to address:**
Phase 2 - Per-skill verification guidance; Phase 4 - xUnit integration testing skill

---

### Pitfall 9: Frontend and API Skills Blur Responsibility Around Contracts

**What goes wrong:**
Frontend and minimal API skills each make assumptions about validation, error shapes, auth, and OpenAPI ownership. Agents then ship mismatched contracts and patch around them downstream.

**Why it happens:**
UI and API work meet at the boundary where ambiguity is easiest to ignore. Specialist skills often focus inward on their own code rather than the contract surface.

**How to avoid:**
State contract ownership rules explicitly. The API skill should define canonical response and validation patterns; the frontend skill should consume those contracts and escalate when they are missing or unstable.

**Warning signs:**
Frontend examples invent API responses, API examples omit error contracts, or both skills provide different rules for validation and authentication flows.

**Phase to address:**
Phase 5 - C# minimal API skill; Phase 6 - Frontend implementation skill

---

### Pitfall 10: "Opinionated" Guidance Hardens Into Unsupported Dogma

**What goes wrong:**
A skill forbids valid alternatives or pushes one stack pattern as universally correct, causing bad recommendations when teams have different infrastructure, compliance, or operational constraints.

**Why it happens:**
Opinionation is useful for reliability, but without scope qualifiers it becomes cargo-cult architecture.

**How to avoid:**
Require each opinion to declare its context, tradeoff, and escape hatch. Keep the recommendation strong, but name when another path is reasonable and when the orchestrator should decide.

**Warning signs:**
Absolute wording without rationale, missing tradeoffs, or repeated cases where users must override the skill to fit their environment.

**Phase to address:**
Phase 1 - Opinionation rules in the standard; Phase 3 - Cross-skill review

## Technical Debt Patterns

Shortcuts that seem reasonable but create long-term problems.

| Shortcut | Immediate Benefit | Long-term Cost | When Acceptable |
|----------|-------------------|----------------|-----------------|
| Copying one skill's structure into another without reworking boundaries | Fast authoring | Propagates the wrong scope, stale examples, and misleading handoff rules | Only as a starting skeleton before domain-specific rewrite |
| Using repository-local conventions without labeling them as local opinion | Faster writing | Mixed-agent portability degrades and agents overfit to this repo | Never |
| Deferring verification guidance to a future testing skill | Quicker first draft | Skills produce untestable outputs and hidden regressions | Only for an explicitly marked draft, not for a published skill |
| Omitting version and package assumptions | Less maintenance upfront | References drift and examples silently rot | Never |
| Treating handoff rules as optional polish | Smaller initial document | Cross-agent loops and ownership confusion become systemic | Never |

## Integration Gotchas

Common mistakes when connecting to external services.

| Integration | Common Mistake | Correct Approach |
|-------------|----------------|------------------|
| Official .NET / framework docs | Linking only homepage docs and not the exact operational page | Cite the specific authoritative page for the concept, plus a short note on how the skill narrows it |
| Mixed agent runtimes | Depending on one agent's tool model in the core workflow | Keep domain instructions tool-agnostic and isolate runtime-specific notes as optional examples |
| Existing repo skills | Adding new skills without reconciling terms, examples, and handoff wording with `aspire` and `masstransit` | Run a consistency pass against existing skills before publishing a new one |
| Real infrastructure in examples | Showing local-only examples for distributed concerns like brokers, telemetry collectors, or database migrations | Include at least one example that reflects realistic service boundaries and infrastructure interaction |

## Performance Traps

Patterns that work at small scale but fail as usage grows.

| Trap | Symptoms | Prevention | When It Breaks |
|------|----------|------------|----------------|
| Oversized skills that try to cover multiple specialties | Agents miss rules, choose the wrong section, or produce inconsistent outputs | Keep skills narrow and route aggressively at boundaries | Breaks once a skill is used across more than 2-3 adjacent domains |
| Reference sprawl inside a skill | Maintenance cost rises and agents anchor on stale details | Curate a small set of authoritative links and examples per skill | Breaks when updates require touching many loosely related sections |
| Cross-skill inconsistency debt | New skills take longer because each one must rediscover repo conventions | Create a single standard and review checklist before expanding the library | Breaks sharply around the 4th or 5th skill, which is this project's current stage |
| Verification gaps hidden by plausible prose | Skills seem complete until downstream agents start reporting low-confidence outputs | Require "evidence of completion" sections and demo-to-production checklists | Breaks as soon as multiple agents chain outputs without human correction |

## Security Mistakes

Domain-specific security issues beyond general web security.

| Mistake | Risk | Prevention |
|---------|------|------------|
| Examples include inline secrets, connection strings, or fake-but-realistic credentials | Agents may reproduce insecure patterns in generated code and docs | Use placeholders, secret-provider patterns, and explicit "never hardcode" language in every infrastructure-facing skill |
| Skills normalize broad dev-only settings as default practice | Insecure transport, weak auth, or disabled validation leaks into production guidance | Label development-only shortcuts clearly and pair them with production-safe alternatives |
| Handoff guidance skips auth and data-boundary ownership | Agents add endpoints or integrations without clarifying who owns access control or sensitive data flow | Require each skill to state when auth, secrets, or PII concerns force escalation or specialist handoff |
| References lag behind current framework security guidance | Agents follow deprecated middleware or package patterns | Review official security-sensitive references during each skill update cycle |

## UX Pitfalls

Common user experience mistakes in this domain.

| Pitfall | User Impact | Better Approach |
|---------|-------------|-----------------|
| Skills force the user to infer which specialist owns a task | Users receive conflicting or partial outputs and lose trust in delegation | Put trigger conditions and handoff rules at the top of each skill |
| Skills return advice without a clear artifact shape | Users cannot tell whether the agent finished, paused, or needs orchestration | Define expected deliverables, status language, and completion evidence |
| Skills use repo jargon without explaining it | New contributors and non-Codex agents misapply conventions | Standardize terminology and define it once in the shared standard |
| Examples look polished but do not mirror real project constraints | Users accept recommendations that later fail in distributed environments | Prefer realistic examples with explicit limitations over idealized snippets |

## "Looks Done But Isn't" Checklist

- [ ] **Shared skill standard:** Often missing explicit handoff and stop conditions — verify each skill says when to continue, hand off, and escalate
- [ ] **References:** Often missing authoritative links and version assumptions — verify every major claim is anchored to official docs or clearly marked repo opinion
- [ ] **Examples:** Often missing realistic distributed failure paths — verify each skill includes at least one production-shaped example, not only toy snippets
- [ ] **Verification guidance:** Often missing evidence requirements — verify each skill states how an agent should prove the work is correct
- [ ] **Portability:** Often missing runtime-neutral instructions — verify the main workflow does not depend on a single agent environment
- [ ] **Cross-skill consistency:** Often missing shared terminology and defaults — verify new skills align with existing `aspire` and `masstransit` conventions or document the divergence

## Recovery Strategies

| Pitfall | Recovery Cost | Recovery Steps |
|---------|---------------|----------------|
| Boundary drift between specialist skills | MEDIUM | Freeze new skill authoring, define ownership matrices, trim overlapping sections, and add direct handoff language before expanding further |
| Human-tutorial style instead of agent-operational guidance | MEDIUM | Rewrite around decision rules, output patterns, and failure handling; keep explanatory prose only where it changes behavior |
| Portability hidden behind runtime assumptions | MEDIUM | Split core instructions from environment notes, test the skill mentally against a second agent model, and remove nonessential tool coupling |
| Conflicting architectural defaults across skills | HIGH | Create a shared conventions document, reconcile existing skills first, then update planned skills against that baseline |
| Verification guidance missing from implementation skills | HIGH | Add a mandatory validation section per skill and backfill an integration-testing strategy that exercises the highest-risk seams |

## Pitfall-to-Phase Mapping

How roadmap phases should address these pitfalls.

| Pitfall | Prevention Phase | Verification |
|---------|------------------|--------------|
| Boundary drift between specialist skills | Phase 1 - Shared skill standard and routing rules | Review each skill for explicit in-scope, out-of-scope, handoff, and escalate sections |
| Human-tutorial style instead of agent-operational guidance | Phase 1 - Shared skill standard and quality bar | Sample a task prompt per skill and confirm the skill yields a concrete action path without extra interpretation |
| Portability hidden behind runtime assumptions | Phase 1 - Portability requirements in the standard | Check that core instructions remain valid without Codex-specific tools or shell assumptions |
| Shallow examples create false confidence | Phase 2 - Domain skill authoring | Confirm each skill has at least one realistic distributed .NET scenario with failure-aware guidance |
| Missing official reference anchors | Phase 1 and Phase 2 | Verify every major section cites authoritative documentation and labels local opinion separately |
| Handoff rules ignore orchestrator control | Phase 1 - Orchestrator interaction rules | Test whether a task crossing two domains results in a clean stop or routed handoff instead of overlap |
| Conflicting architectural defaults across skills | Phase 3 - Cross-skill consistency review | Compare terminology, example assumptions, and production-readiness rules across all skills |
| Testability guidance arrives too late | Phase 4 - xUnit integration testing skill plus per-skill verification sections | Ensure each skill defines validation expectations and the testing skill covers system-level proof paths |
| Frontend and API skills blur contract ownership | Phase 5 and Phase 6 - Minimal API then frontend skills | Verify API skill defines canonical contracts and frontend skill consumes rather than invents them |
| Opinionated guidance hardens into unsupported dogma | Phase 3 - Cross-skill review | Check that strong recommendations include rationale, scope, and allowed exceptions |

## Sources

- Existing project brief in [PROJECT.md](C:\Development\ClaudeSkills\.planning\PROJECT.md)
- Existing skill implementations in `skills/aspire/SKILL.md` and `skills/masstransit/SKILL.md`
- Known failure patterns from multi-agent prompt libraries: boundary ambiguity, portability drift, and missing verification contracts
- Distributed .NET practice expectations implied by the planned skill set: observability, EF Core, integration testing, minimal APIs, and frontend contract consumption

---
*Pitfalls research for: specialist AI-agent skill libraries for distributed .NET work*
*Researched: 2026-03-11*
