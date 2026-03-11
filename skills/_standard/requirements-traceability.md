# Phase 1 Requirements Traceability

This matrix maps the shared skill standard to the Phase 1 requirements. It provides structural review evidence before retrofit work begins.

## Artifact index

| Artifact | Role |
|---------|------|
| [`package-standard.md`](./package-standard.md) | Defines the canonical package tree, required artifacts, optional artifacts, supported adapter patterns, and deferred items |
| [`skill-sections.md`](./skill-sections.md) | Defines the fixed canonical `SKILL.md` section order and section intent |

## Traceability matrix

| Requirement | Standard rule | Evidence |
|------------|---------------|----------|
| `STND-01` | Every v1 skill must ship a canonical package with `SKILL.md`, `references/`, `examples/`, and `validation/` artifacts, and `SKILL.md` must follow one fixed section order | `package-standard.md` sections `Canonical package tree` and `Required artifacts`; `skill-sections.md` section `Fixed section order` |
| `STND-02` | Canonical `SKILL.md` must declare scope, in-scope work, out-of-scope work, and boundary and handoff rules that define continue, direct handoff, and return-to-orchestrator decisions | `skill-sections.md` sections `Scope`, `In-scope work`, `Out-of-scope work`, and `Boundary and handoff rules` |
| `STND-03` | Every skill package must include `examples/` as a first-class directory with at least one happy-path example and one boundary or handoff example | `package-standard.md` sections `Canonical package tree`, `Required artifacts`, and `examples/` |
| `STND-04` | Every skill package must include `references/` and `validation/` artifacts so official documentation, verification evidence, link checks, and limitations can be reviewed consistently | `package-standard.md` sections `references/` and `validation/`; `skill-sections.md` sections `References map` and `Validation expectations` |
| `PORT-01` | `SKILL.md` remains the canonical source while adapters are optional thin projections that must preserve semantic parity | `package-standard.md` sections `Package goals`, `Supported adapter patterns`, and `Adapter obligations`; `skill-sections.md` section `Adapter notes` |
| `PORT-02` | Canonical section semantics and package rules stay runtime-neutral so mixed-agent environments can follow the same skill without depending on adapter-only behavior | `skill-sections.md` sections `Workflow` and `Portability guardrails`; `package-standard.md` sections `Package goals` and `Deferred items` |

## Requirement coverage notes

- All six Phase 1 requirements are explicitly covered by central standard artifacts before retrofit begins.
- Package rules point to the canonical `SKILL.md` contract so package shape and section semantics stay aligned.
- Traceability cites package-standard and section-contract clauses only; no requirement depends on runtime-specific adapter behavior.

## Review outcomes to confirm during plan verification

- `STND-01` through `STND-04` appear in this matrix and point to specific clauses.
- `PORT-01` and `PORT-02` appear in this matrix and point to specific clauses.
- The package standard and section contract do not contradict each other.
