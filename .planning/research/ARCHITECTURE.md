# Architecture Research

**Domain:** AI agent skill library for distributed .NET development
**Researched:** 2026-03-11
**Confidence:** HIGH

## Standard Architecture

### System Overview

```text
┌──────────────────────────────────────────────────────────────────────┐
│                    Orchestrator / Routing Layer                     │
├──────────────────────────────────────────────────────────────────────┤
│  User Task Intake  │  Skill Selection  │  Cross-Skill Coordination  │
│  Milestone Plan    │  Boundary Checks  │  Escalation / Return       │
└───────────────┬───────────────┬───────────────────────┬─────────────┘
                │               │                       │
┌───────────────┴───────────────┴───────────────────────┴─────────────┐
│                        Specialist Skill Packages                     │
├──────────────────────────────────────────────────────────────────────┤
│  aspire  │  masstransit  │  opentelemetry-dotnet  │  efcore        │
│  api     │  xunit-int    │  frontend-implementation │ future skills │
└───────────────┬───────────────┬───────────────────────┬─────────────┘
                │               │                       │
┌───────────────┴───────────────┴───────────────────────┴─────────────┐
│                         Skill Artifact Layer                         │
├──────────────────────────────────────────────────────────────────────┤
│  SKILL.md  │  references/  │  examples  │  plugin metadata         │
│  scope     │  official docs│  expected  │  runtime discovery        │
│  handoffs  │  constraints  │  outputs   │  trigger information      │
└───────────────┬───────────────┬───────────────────────┬─────────────┘
                │               │                       │
┌───────────────┴───────────────┴───────────────────────┴─────────────┐
│                     Consumer Execution Environments                  │
├──────────────────────────────────────────────────────────────────────┤
│  Codex  │  Claude Code  │  Other agent runtimes  │  Human review   │
└──────────────────────────────────────────────────────────────────────┘
```

### Component Responsibilities

| Component | Responsibility | Typical Implementation |
|-----------|----------------|------------------------|
| Orchestrator | Decide which specialist should act, sequence multi-skill work, and reclaim control when tasks cross domains | Planner or coordinating agent with routing rules and milestone context |
| Skill package | Own one narrow domain and produce reliable outputs within that domain only | One folder per skill with `SKILL.md`, references, examples, and optional plugin metadata |
| Reference set | Anchor skill advice in official docs and stable patterns | Curated markdown files under `references/` |
| Example/output contract | Show the form and quality bar of expected outputs | Small concrete snippets, checklists, and handoff notes inside each skill |
| Plugin metadata | Make the skill discoverable by supported runtimes without changing core content | `.claude-plugin/plugin.json` and related runtime files |
| Research/planning artifacts | Capture project-level decisions that shape future skills consistently | `.planning/PROJECT.md` plus `.planning/research/*.md` |

## Recommended Project Structure

```text
skills/
├── aspire/                       # Existing distributed app orchestration skill
│   ├── SKILL.md
│   ├── references/
│   └── .claude-plugin/
├── masstransit/                  # Existing messaging specialist skill
│   ├── SKILL.md
│   ├── references/
│   └── .claude-plugin/
├── opentelemetry-dotnet/         # Cross-cutting observability skill
│   ├── SKILL.md
│   ├── references/
│   ├── examples/
│   └── .claude-plugin/
├── efcore/                       # Persistence and migrations skill
│   ├── SKILL.md
│   ├── references/
│   ├── examples/
│   └── .claude-plugin/
├── csharp-minimal-api/           # HTTP/API edge skill
│   ├── SKILL.md
│   ├── references/
│   ├── examples/
│   └── .claude-plugin/
├── xunit-integration-testing/    # Verification and test harness skill
│   ├── SKILL.md
│   ├── references/
│   ├── examples/
│   └── .claude-plugin/
└── frontend-implementation/      # UI implementation specialist
    ├── SKILL.md
    ├── references/
    ├── examples/
    └── .claude-plugin/

.planning/
├── PROJECT.md
└── research/
    ├── ARCHITECTURE.md
    └── other research artifacts
```

### Structure Rationale

- **`skills/<skill>/`:** The skill folder is the primary deployable unit. This keeps scope, references, examples, and runtime metadata versioned together.
- **`SKILL.md`:** The real interface contract. It should define domain ownership, decision rules, handoff triggers, examples, and references.
- **`references/`:** Keeps supporting material separated from the main instruction surface so the core skill stays concise but grounded.
- **`examples/`:** Useful for new skills because this repo’s quality bar depends on concrete output shapes, not abstract advice.
- **`.claude-plugin/`:** Runtime-specific packaging should stay isolated so the skill content remains portable across agent environments.
- **`.planning/`:** Project-level architecture and sequencing decisions belong outside individual skills so the library evolves coherently.

## Architectural Patterns

### Pattern 1: Specialist Skill as a Bounded Context

**What:** Each skill owns one narrow technical slice and explicitly refuses adjacent concerns once work crosses that boundary.
**When to use:** Always. This is the core design principle of the repo.
**Trade-offs:** Higher handoff frequency, but much better reliability and less instruction drift than broad full-stack skills.

**Example:**
```text
Task: "Add tracing, metrics, and dashboard wiring to an Aspire service"

1. opentelemetry-dotnet owns instrumentation choices and OTLP guidance
2. aspire owns AppHost resource wiring and dashboard/runtime composition
3. Orchestrator merges both outputs and resolves sequencing
```

### Pattern 2: Artifact-First Skill Packaging

**What:** A skill is not only prose. It is a package of instructions, references, examples, and runtime metadata.
**When to use:** For every new skill added to the library.
**Trade-offs:** Slightly more authoring overhead, but the result is portable and easier to validate across agents.

**Example:**
```text
skill/
├── SKILL.md          # decision rules + handoffs
├── references/       # official-doc excerpts and patterns
├── examples/         # expected output shapes
└── .claude-plugin/   # discovery metadata
```

### Pattern 3: Orchestrator Reclaims Cross-Cutting Work

**What:** Skills should solve local-domain work and immediately return control when coordination, prioritization, or conflict resolution spans domains.
**When to use:** Multi-skill tasks, conflicting recommendations, or any requirement that combines architecture decisions across specialties.
**Trade-offs:** Requires more explicit routing language, but prevents specialist instructions from silently overreaching.

**Example:**
```text
If a Minimal API task requires endpoint design, EF Core persistence,
MassTransit events, and OpenTelemetry instrumentation, the API skill
should define the HTTP contract and hand back for coordination rather
than embedding persistence, messaging, and observability policy itself.
```

## Data Flow

### Request Flow

```text
[User / Planner Request]
    ↓
[Orchestrator identifies domain and expected artifact]
    ↓
[Selected Skill Package]
    ↓
[SKILL.md decision rules]
    ↓
[references/ + examples/ consulted as needed]
    ↓
[Domain-specific output or handoff note]
    ↓
[Orchestrator merges outputs or routes to next skill]
```

### State Management

```text
[PROJECT.md / milestone context]
    ↓
[Research artifacts] ←→ [Skill authoring decisions]
    ↓
[Skill package updates]
    ↓
[Agent runtime consumption]
```

### Key Data Flows

1. **Authoring flow:** Project requirements in `PROJECT.md` drive research artifacts, which define the standard that future skill packages should implement.
2. **Execution flow:** A user request is routed to one specialist skill, which consults references and examples, produces an output, then either completes or hands back to the orchestrator.
3. **Cross-skill handoff flow:** A skill emits a boundary decision such as "continue with Aspire" or "return to orchestrator for sequencing", rather than attempting adjacent work itself.
4. **Portability flow:** Core skill content stays runtime-agnostic, while plugin metadata adapts the same skill for specific agent environments.

## Agent And Orchestrator Handoff Boundaries

| Boundary | Owned By | Handoff Trigger | Expected Handoff Artifact |
|----------|----------|-----------------|---------------------------|
| Task routing | Orchestrator | User request spans multiple specialties or unclear scope | Selected skill list plus ordering |
| Distributed app composition | Aspire skill | Need service topology, AppHost wiring, resource dependencies, dashboard composition | Resource map, startup ordering, AppHost guidance |
| Messaging workflow | MassTransit skill | Need broker topology, consumer patterns, saga/outbox, transport concerns | Message contract and transport recommendations |
| Observability instrumentation | OpenTelemetry skill | Need traces, metrics, logs, OTLP/exporter guidance, semantic conventions | Instrumentation plan and telemetry boundary notes |
| Persistence design | EF Core skill | Need entity mapping, migrations, transaction boundaries, DB patterns | Persistence model and migration guidance |
| HTTP edge design | Minimal API skill | Need endpoint shape, binding, validation, auth, OpenAPI, response patterns | Endpoint contract and API behavior guidance |
| End-to-end verification | xUnit integration skill | Need harness design, fixture setup, containerized dependencies, scenario validation | Test plan and fixture structure |
| UI build execution | Frontend skill | Need client-side implementation details and interaction patterns | Component/task breakdown and integration contract |
| Cross-cutting conflicts | Orchestrator | Recommendations from multiple skills overlap or contradict | Reconciled build sequence and ownership decisions |

## Suggested Build Order

1. **Shared skill standard first**
   Define the reusable structure for `SKILL.md`, required reference style, example expectations, and mandatory handoff sections. This is the schema every later skill depends on.
2. **`opentelemetry-dotnet`**
   Observability is the least domain-specific cross-cutting skill and complements the existing `aspire` and `masstransit` skills immediately. It also establishes the pattern for cross-cutting-but-bounded skills.
3. **`efcore`**
   Persistence is a core backend dependency for distributed .NET systems and informs later guidance for APIs, messaging outbox patterns, and integration testing.
4. **`csharp-minimal-api`**
   After persistence and observability guidance exist, API guidance can reference them without swallowing those domains. This skill becomes the clearest example of precise handoff rules.
5. **`xunit-integration-testing`**
   Testing should land after primary runtime skills exist so its examples can validate realistic combinations like Aspire + EF Core + MassTransit + Minimal API.
6. **`frontend-implementation`**
   Build this last in the milestone because it depends mostly on stable backend/API boundaries rather than shaping them. It should consume contracts produced by the API skill, not invent them.

### Build Order Rationale

- Start with the contract for how skills are authored, or the library will drift stylistically and behaviorally.
- Add backend and cross-cutting specialists before edge skills so downstream handoffs point to real, existing skill packages.
- Add verification after the system under test exists conceptually.
- Add frontend last because it benefits from stable service contracts and is least coupled to the current repo foundation (`aspire` and `masstransit`).

## Scaling Considerations

| Scale | Architecture Adjustments |
|-------|--------------------------|
| 2-6 skills | Manual consistency review is acceptable; shared template and research docs are enough |
| 6-12 skills | Add stronger examples, a standard checklist, and lightweight validation for folder structure and required sections |
| 12+ skills | Introduce index/catalog artifacts, automated linting of skill package shape, and explicit dependency maps between skills |

### Scaling Priorities

1. **First bottleneck:** Inconsistent skill quality and missing handoff rules. Fix with a mandatory authoring standard and reusable review checklist.
2. **Second bottleneck:** Discoverability of overlapping skills. Fix with a central catalog that states scope, triggers, and neighboring skills clearly.

## Anti-Patterns

### Anti-Pattern 1: Monolithic Full-Stack Skill

**What people do:** Create one giant skill that covers API, database, messaging, frontend, observability, and testing.
**Why it's wrong:** It destroys boundary clarity, makes routing meaningless, and produces unreliable outputs because instructions conflict.
**Do this instead:** Keep each skill narrow and require explicit return-to-orchestrator behavior when multiple domains are involved.

### Anti-Pattern 2: Reference-Only Skills

**What people do:** Dump documentation summaries into `SKILL.md` without examples, decision rules, or handoff triggers.
**Why it's wrong:** Agents may know facts but still fail to produce bounded, repeatable outputs.
**Do this instead:** Treat `SKILL.md` as an execution contract with examples, scope limits, and escalation rules.

### Anti-Pattern 3: Runtime-Specific Content Leaking into Core Skill Logic

**What people do:** Bake one agent platform’s metadata or tool assumptions into the core skill instructions.
**Why it's wrong:** Portability is a project constraint, and runtime-specific details age faster than domain guidance.
**Do this instead:** Keep portable domain instructions in `SKILL.md` and isolate runtime packaging in `.claude-plugin/` or equivalent metadata folders.

## Integration Points

### External Services

| Service | Integration Pattern | Notes |
|---------|---------------------|-------|
| Official product documentation | Curated markdown references derived from official docs | Keeps skills grounded without forcing live web access during execution |
| Agent runtimes | Plugin/discovery metadata alongside portable skill text | Runtime-specific packaging should not change domain ownership |
| Future validation tooling | Folder-shape and content checks over `skills/*` | Useful once the library grows beyond manual review |

### Internal Boundaries

| Boundary | Communication | Notes |
|----------|---------------|-------|
| Orchestrator ↔ skill package | Task selection plus handoff note | The orchestrator owns sequencing and conflict resolution |
| Skill package ↔ references | Direct local reads | References support the skill; they do not replace it |
| Skill package ↔ examples | Direct local reads | Examples should demonstrate output quality and boundary decisions |
| Skill package ↔ plugin metadata | Packaging metadata only | Metadata should expose the skill, not redefine its scope |
| Research artifacts ↔ new skill authoring | Human/agent authoring guidance | Research should standardize patterns before new skills are added |

## Sources

- `C:\Development\ClaudeSkills\.planning\PROJECT.md`
- `C:\Development\ClaudeSkills\skills\aspire\SKILL.md`
- `C:\Development\ClaudeSkills\skills\masstransit\SKILL.md`
- `C:\Users\james\.codex\get-shit-done\templates\research-project\ARCHITECTURE.md`

---
*Architecture research for: ClaudeSkills*
*Researched: 2026-03-11*
