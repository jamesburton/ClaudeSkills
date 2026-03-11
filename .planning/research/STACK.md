# Stack Research

**Domain:** Portable specialist AI-agent skill library for distributed .NET development
**Researched:** 2026-03-11
**Confidence:** HIGH

## Recommended Stack

### Core Technologies

| Technology | Version | Purpose | Why Recommended |
|------------|---------|---------|-----------------|
| Markdown (GitHub Flavored Markdown / CommonMark) | Current standard | Canonical source format for every skill | Portable across agent runtimes, diff-friendly, readable in Git, and already aligned with the repo's existing `SKILL.md` pattern. |
| YAML front matter | 1.2 | Machine-readable trigger metadata in canonical skill files | Lets each skill declare name, description, and trigger hints without coupling the body to one agent runtime. |
| .NET SDK | 10.0.x LTS | Primary version baseline for examples and validation fixtures | Current LTS gives the longest stable runway; examples can still note `.NET 8/9` compatibility where relevant, but the default baseline should be modern and supported. |
| ASP.NET Core Minimal APIs | 10.0.x | Baseline API style for endpoint-focused skills | Microsoft recommends Minimal APIs for new HTTP API projects, which fits the project's narrow, implementation-oriented skill model. |
| Entity Framework Core | 10.0.x LTS | Persistence baseline for database-oriented skills | EF Core 10 aligns with the current LTS generation and keeps guidance current for migrations, querying, and provider usage. |
| OpenTelemetry for .NET | 1.x | Cross-cutting observability baseline for distributed systems skills | Vendor-neutral telemetry is the right default for distributed .NET work and aligns with Aspire's service-defaults model. |
| xUnit.net | 3.2.x | Validation harness for examples, fixtures, and integration guidance | xUnit v3 is current, .NET-first, and a strong fit for example verification and integration-testing skill content. |

### Supporting Libraries

| Library | Version | Purpose | When to Use |
|---------|---------|---------|-------------|
| `Microsoft.AspNetCore.OpenApi` | 10.0.x | OpenAPI generation for Minimal API examples | Use in the `csharp-minimal-api` skill whenever the example exposes contract metadata or API discovery guidance. |
| `Microsoft.Extensions.Validation` | 10.0.x | Validation APIs for modern .NET examples | Use when showing request/model validation in `.NET 10` examples; note namespace/package differences when documenting older versions. |
| `Microsoft.EntityFrameworkCore.Design` | 10.0.x | Migrations and design-time EF tooling | Use in `efcore` examples that include migrations, schema updates, or scaffolded workflows. |
| `OpenTelemetry.Exporter.OpenTelemetryProtocol` | 1.x | OTLP export in observability examples | Use whenever a skill needs a vendor-neutral export path instead of backend-specific exporters. |
| `Testcontainers` for .NET | 4.x | Real infrastructure in integration test examples | Use in `xunit-integration-testing` examples when the test needs realistic databases, brokers, or other service dependencies. |

### Development Tools

| Tool | Purpose | Notes |
|------|---------|-------|
| `dotnet` SDK 10.0.x | Compile and run code samples, fixtures, and validation smoke tests | Treat this as the primary executable validation toolchain. If a sample cannot be compiled or run, it is documentation-only and should be marked as such. |
| `Node.js` 22 LTS | Run markdown formatting and lint tooling | Keep Node limited to authoring/lint tooling. The skill library itself should stay runtime-agnostic and not require Node for consumption. |
| `markdownlint-cli2` 0.18.x | Markdown structure and style linting | Enforce heading consistency, fenced code block hygiene, and list formatting across all skills. |
| `Prettier` 3.x | Stable markdown formatting | Use for whitespace, wrapping, and fenced block normalization; avoid editing semantics with formatting scripts. |
| `Vale` 3.x | Prose linting for clarity and consistency | Use to catch weak wording, ambiguous instructions, and inconsistent terminology in canonical skill docs. |
| `lychee` 0.18.x | Link validation | Run in CI or as a pre-release validation step to catch dead official-doc references. |

## Recommended Repository Structure

```text
skills/
  <skill-name>/
    SKILL.md
    references/
      <topic>.md
    examples/
      <scenario>.md
    validation/
      checklist.md
      fixtures/
    .claude-plugin/              # optional adapter output
      plugin.json
      CLAUDE.md
    skills/
      <skill-name>/              # optional adapter output
        CLAUDE.md
```

### Structure Rules

- `SKILL.md` is the only canonical source of truth for the skill.
- `references/` contains condensed, official-doc-backed notes, not copied documentation dumps.
- `examples/` contains short, realistic examples with an explicit target stack and expected outcome.
- `validation/` contains repeatable checks, sample prompts, fixture code, and failure modes.
- Runtime-specific files such as `.claude-plugin/` or `skills/<name>/CLAUDE.md` are derivative adapters only. They must not diverge semantically from `SKILL.md`.
- Keep each skill self-contained. Do not create a shared mega-reference folder that forces cross-skill coupling.

## Skill Content Standard

Each `SKILL.md` should include these sections in this order:

1. Front matter: `name`, concise `description`, and trigger clues.
2. Purpose and scope: what the skill owns.
3. Boundaries: what the skill does not own.
4. Inputs expected from the caller.
5. Workflow: ordered operating steps for the specialist agent.
6. Examples: at least one happy-path example and one boundary/handoff example.
7. References: official docs first, secondary sources only when clearly labeled.
8. Validation: how to check outputs, what evidence to produce, and what failures mean.
9. Handoff rules: when to continue, when to hand off, and what context to transfer.

## Validation Standard

- Every example must be tagged with a target version such as `.NET 10`, `EF Core 10`, or `xUnit v3`.
- Prefer executable examples over pseudo-code. If pseudo-code is unavoidable, label it clearly.
- Require a validation checklist per skill that answers:
  - Did the output stay inside the skill boundary?
  - Did the examples compile or match current official API shapes?
  - Are all official links live?
  - Does the handoff guidance name the next likely specialist explicitly?
- For code-oriented skills, include at least one fixture or snippet that can be compiled or test-run.
- Do not mark work "done" without validation evidence or an explicit statement that execution could not be performed.

## Handoff Rules

- A specialist skill continues only while the requested work remains inside its declared boundary.
- A skill must hand off when responsibility crosses into another specialist area, for example:
  - `csharp-minimal-api` -> `efcore` for schema/migration design
  - `csharp-minimal-api` -> `opentelemetry-dotnet` for telemetry architecture
  - `csharp-minimal-api` -> `frontend-implementation` for UI behavior and rendering
  - `efcore` -> `masstransit` for outbox/event delivery architecture
  - Any specialist -> orchestrator when the task spans multiple specialists or requires sequencing
- Handoffs must transfer:
  - current objective
  - completed work
  - unresolved risks
  - exact next question or decision
- Do not let a skill absorb orchestration duties. Multi-skill coordination belongs above the specialist layer.

## Installation

```bash
# .NET validation baseline
dotnet --info
dotnet new install xunit.v3.templates

# Markdown authoring tools
npm install -D markdownlint-cli2 prettier

# Optional validation tools
# Install Vale and lychee through your preferred package manager in CI/dev shells
```

## Alternatives Considered

| Recommended | Alternative | When to Use Alternative |
|-------------|-------------|-------------------------|
| Canonical `SKILL.md` plus thin runtime adapters | Separate per-runtime skill files as peers | Only use peer runtime files if a platform has unavoidable metadata or packaging requirements that cannot be generated from the canonical doc. |
| `.NET 10` LTS as the default example baseline | `.NET 8` as the default baseline | Use `.NET 8` when documenting teams that are locked to the current older LTS and the guidance differs materially. |
| `xUnit.net v3` for validation fixtures | NUnit or MSTest | Use an alternative only when the target ecosystem already mandates it or when interop with an existing test harness matters more than library consistency. |
| Official-doc-first references | Blog-post-first references | Use secondary sources only for gaps, gotchas, or comparative commentary after the official reference has been established. |

## What NOT to Use

| Avoid | Why | Use Instead |
|-------|-----|-------------|
| Monolithic "full-stack .NET" skills | They blur ownership, weaken delegation, and create bad handoff behavior | Narrow specialist skills with explicit boundaries |
| Runtime-specific instructions in canonical `SKILL.md` | They reduce portability and force one agent platform's assumptions onto all consumers | Keep runtime packaging in derivative adapter files |
| Long pasted vendor docs in `references/` | They age badly, bloat the repo, and hide the actionable guidance | Condensed notes with links to official docs |
| Versionless code examples | They become ambiguous as APIs move across `.NET`, `EF Core`, and `xUnit` releases | Version-stamped examples and fixtures |
| Validation claims without executable checks or explicit caveats | They create false confidence for downstream agents | Checklists plus compile/test/link evidence |
| Tool-specific orchestration logic inside specialist skills | It couples the library to one runner and breaks portability | Neutral handoff rules that any orchestrator can consume |

## Stack Patterns by Variant

**If the skill is framework-specialist (`aspire`, `masstransit`, `efcore`, `opentelemetry-dotnet`):**
- Keep the workflow focused on framework decisions, common patterns, integration points, and failure modes.
- Add a `references/` file per major topic rather than one giant catch-all reference.
- Include at least one example that shows where the framework should not be used and who should own the next step.

**If the skill is implementation-specialist (`csharp-minimal-api`, `frontend-implementation`, `xunit-integration-testing`):**
- Require concrete inputs and expected outputs.
- Include "definition of done" criteria and validation evidence expectations.
- Show how to defer adjacent concerns instead of partially solving them.

**If the skill needs agent-runtime packaging:**
- Keep the portable `SKILL.md` canonical.
- Generate or manually maintain thin adapters that re-express the same intent for the target runtime.
- Never let adapter files contain extra business guidance that the canonical skill does not include.

## Version Compatibility

| Package A | Compatible With | Notes |
|-----------|-----------------|-------|
| `.NET SDK 10.0.x` | `.NET 10`, `.NET 9`, `.NET 8` sample verification | Use `.NET 10` as the authoring baseline, but note older target frameworks explicitly when examples are intended for them. |
| `EF Core 10.0.x` | `.NET 10` | Preferred LTS pairing for new persistence examples. |
| `EF Core 9.0.x` | `.NET 8` and `.NET 9` | Use only when the skill is documenting currently deployed STS/LTS estates that have not moved to EF 10 yet. |
| `xUnit.net v3 3.2.x` | `.NET 8+` test projects | Current v3 line is appropriate for new validation fixtures and integration-testing examples. |
| `OpenTelemetry .NET 1.x` | Supported modern .NET runtimes | Keep API and instrumentation package families aligned; prefer OTLP examples over backend-specific exporters. |

## Sources

- [Official .NET support policy](https://dotnet.microsoft.com/en-us/platform/support/policy) — verified current supported `.NET 8`, `.NET 9`, and `.NET 10` timelines and LTS/STS status
- [Aspire overview](https://learn.microsoft.com/en-us/dotnet/aspire/get-started/aspire-overview) — verified Aspire's code-first distributed app model and OpenTelemetry-oriented service defaults
- [Minimal APIs overview](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/overview?view=aspnetcore-9.0) — verified that Minimal APIs are the recommended starting point for new ASP.NET Core HTTP APIs
- [Minimal APIs quick reference (.NET 10)](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis?view=aspnetcore-10.0) — verified current API organization, middleware, and validation guidance
- [What's new in ASP.NET Core in .NET 10](https://learn.microsoft.com/en-us/aspnet/core/release-notes/aspnetcore-10.0?view=aspnetcore-10.0) — verified `.NET 10` validation package changes relevant to skill examples
- [Supported .NET implementations for EF Core](https://learn.microsoft.com/en-us/ef/core/platforms) — verified EF Core version/runtime compatibility
- [What's new in EF Core 10](https://learn.microsoft.com/en-us/ef/core/what-is-new/ef-core-10.0/whatsnew) — verified EF Core 10 LTS status for new examples
- [OpenTelemetry .NET docs](https://opentelemetry.io/docs/languages/dotnet/) — verified current language-specific guidance baseline
- [OpenTelemetry .NET getting started](https://opentelemetry.io/docs/languages/dotnet/getting-started/) — verified modern `.NET 8+` manual instrumentation baseline
- [xUnit.net getting started with v3](https://xunit.net/docs/getting-started/v3/getting-started) — verified current v3 onboarding and sample assumptions
- [xUnit.net releases](https://xunit.net/releases/) — verified current v3 release line

---
*Stack research for: portable specialist AI-agent skill library for distributed .NET development*
*Researched: 2026-03-11*
