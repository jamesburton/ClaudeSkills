# Phase 1 Research: Shared Skill Standard

## Research Goal

Phase 1 should define one canonical package model for specialist AI-agent skills in this repo, then apply it in a way that keeps current `aspire` and `masstransit` skills usable while making future skills predictable to author, validate, and hand off between.

This phase is about standard definition and light normalization, not deep content expansion for every existing skill.

## What The Planner Must Optimize For

- `SKILL.md` remains the canonical source of truth.
- Skills stay specialist, not full-stack.
- Boundary decisions must be explicit enough that an agent can stop instead of improvising across domains.
- Runtime portability must be additive: adapters may restate, not redefine.
- Validation must be concrete enough that later v1 skills can be judged complete with consistent evidence.

## Current Repo Baseline

### Observed package shape

- `skills/aspire/` has `SKILL.md`, `references/`, `.claude-plugin/`, and nested `skills/aspire/CLAUDE.md`.
- `skills/masstransit/` has `SKILL.md`, `references/`, and `.claude-plugin/plugin.json`.
- Neither skill has `examples/`.
- Neither skill has `validation/`.
- Claude adapter coverage is inconsistent: `aspire` has both observed adapter patterns; `masstransit` has only plugin metadata.

### Implications

- The repo already supports `SKILL.md` + `references/` as the stable core.
- Phase 1 should normalize by adding missing standard artifacts and documenting adapter rules, not by redesigning the repo around a new format.
- Existing skills should be upgraded by addition first, then by tightening structure in later cleanup if needed.

## Recommended Canonical Skill Package

Each v1 skill package should use this shape:

```text
skills/<skill>/
  SKILL.md
  references/
    <topic>.md
  examples/
    happy-path.md
    boundary-handoff.md
  validation/
    checklist.md
    evidence.md
  .claude-plugin/                 # optional adapter
    plugin.json
    CLAUDE.md
  skills/<skill>/CLAUDE.md        # optional adapter
```

### Required artifacts

- `SKILL.md`
- `references/` with focused topic files
- `examples/` with at least one happy-path and one boundary/handoff example
- `validation/checklist.md`
- `validation/evidence.md` or equivalent evidence record

### Optional artifacts

- `.claude-plugin/`
- nested `skills/<skill>/CLAUDE.md`

Optional means "not required for validity". If present, they must conform to adapter rules.

## Recommended `SKILL.md` Standard

Use a fixed section order so agents and maintainers can scan every skill the same way:

1. Front matter
2. Title / summary
3. Scope
4. In-scope work
5. Out-of-scope work
6. Boundary and handoff rules
7. Working method / workflow
8. Output expectations
9. References map
10. Validation expectations
11. Adapter notes

### Front matter should standardize

- `name`
- `description`
- `allowed-tools`

Avoid runtime-specific semantics in front matter beyond neutral metadata already used by the repo.

### Section content expectations

- Scope: define the specialty in one tight paragraph.
- In-scope / out-of-scope: explicit bullets, not implied by examples.
- Boundary and handoff rules: define when the skill continues, when it hands to one named specialist, and when it returns to orchestrator.
- Workflow: operational steps for how the specialist approaches work.
- Output expectations: what evidence or summary the specialist returns.
- References map: point to `references/*.md` by topic, not generic "see references".
- Validation expectations: tell the agent what must be true before it claims success.
- Adapter notes: state that adapters are thin projections of `SKILL.md`.

## Architecture Patterns

### Pattern 1: Canonical core plus thin adapters

- Keep all semantics in `SKILL.md`.
- Put detail-heavy source material in `references/`.
- Put realistic execution examples in `examples/`.
- Put completion criteria and evidence rules in `validation/`.
- Treat runtime adapters as packaging layers over the same semantics.

This pattern is the cleanest fit for `PORT-01` and `PORT-02`.

### Pattern 2: Explicit boundary ownership

Each skill should express boundary logic in a small, repeatable rule set:

- Continue when the task remains inside the specialty.
- Hand directly only when exactly one next owner is clear.
- Return to orchestrator when multiple specialties or sequencing decisions are involved.
- Include handoff payload: objective, completed work, unresolved risks, next question/decision.

This rule set should be part of the standard, not repeated ad hoc per skill.

### Pattern 3: Validation as package content, not planner memory

The standard should make validation first-class:

- examples prove behavior shape
- checklist defines required completeness
- evidence record captures what was actually verified

Do not rely on maintainers remembering what "done" means across skills.

## How To Normalize Current Skills Without Breaking Them

Use an additive migration path:

1. Define the standard in central phase artifacts first.
2. Align existing `SKILL.md` files to required section order only where low-risk.
3. Add missing `examples/` and `validation/` directories to `aspire` and `masstransit`.
4. Document that current Claude adapter patterns are both supported.
5. Require semantic parity checks between adapters and canonical `SKILL.md`.

### Do not do in Phase 1

- Do not rewrite `aspire` and `masstransit` content into a brand-new authoring model.
- Do not introduce generated adapters.
- Do not require every existing adapter pattern to be present for every skill.
- Do not add runtime-specific logic to canonical docs.

### Normalization target for current repo

- `aspire`: keep both current Claude adapter forms, add missing examples and validation artifacts, and tighten `SKILL.md` sectioning if needed.
- `masstransit`: keep existing plugin metadata, decide whether to add `CLAUDE.md` now or explicitly allow missing adapter content until a later cleanup step, and add examples and validation artifacts.

The safer planning assumption is that adapter parity rules are defined in Phase 1, while broad adapter backfill across all existing skills stays minimal.

## What Phase 1 Should Create Or Update

Phase 1 should likely produce these artifacts:

- One central standard document for canonical skill package rules
- One central adapter/portability policy document
- One central validation standard document
- One reusable per-skill checklist or template for new v1 skills
- Updates to `aspire` and `masstransit` only where needed to prove the standard is usable

### Likely repo changes

- `.planning/` docs describing the standard in planner-friendly form
- shared templates or checklists under a repo-level standard area
- `skills/aspire/examples/`
- `skills/aspire/validation/`
- `skills/masstransit/examples/`
- `skills/masstransit/validation/`
- targeted updates to existing `SKILL.md` files if required to match the standard

The planner should separate "define the standard" from "retrofit current skills enough to validate the standard".

## Don't Hand-Roll

- Custom per-skill section ordering
- Implicit handoff behavior inferred only from examples
- Runtime-specific canonical instructions
- Unstructured validation notes buried inside `SKILL.md`
- Adapter-specific semantic differences that are not traceable back to canonical docs

These directly create drift, which is the main Phase 1 quality risk.

## Common Pitfalls

### Boundary rules

- "Do adjacent work" language lets specialists absorb other domains.
- Multi-step workflows can hide orchestrator decisions unless return conditions are explicit.
- A handoff rule without a structured payload produces low-quality transfers.

### Adapters

- Adapters can become stale summaries if there is no parity check against `SKILL.md`.
- Supporting two Claude adapter patterns can turn into duplication unless both are treated as optional projections.
- Plugin metadata may look complete while missing operational guidance.

### References

- Large undifferentiated reference files reduce agent precision.
- Version-free reference sets can silently drift from the skill's assumptions.
- Dead official-doc links undermine `STND-04` and should count as validation failure.

### Validation

- Example-only validation is insufficient; examples need checklist and evidence.
- Pseudo-code examples can look valid without proving execution viability; they must be clearly labeled.
- Retrofitting old skills may tempt shallow checklist creation without actual evidence expectations.

## Practical Rollout Order Inside This Phase

### 1. Lock the standard contract

Define:

- canonical package tree
- required vs optional artifacts
- fixed `SKILL.md` section order
- boundary/handoff rule model
- adapter parity rules
- validation evidence model

This supports Plan `01-01` and most of `01-02`.

### 2. Define repo-level templates and checklists

Create the reusable artifacts maintainers will copy:

- package template
- example template
- validation checklist template
- evidence template
- adapter parity checklist

This reduces ambiguity before touching existing skills.

### 3. Retrofit current skills minimally

Apply the new structure to `aspire` and `masstransit` only enough to prove:

- the package model fits real skills
- adapters remain optional/thin
- examples and validation can be added without restructuring references

### 4. Verify requirements coverage

Check explicit mapping to:

- `STND-01`
- `STND-02`
- `STND-03`
- `STND-04`
- `PORT-01`
- `PORT-02`

### 5. Capture deferred follow-up

Anything beyond minimal retrofit should be recorded for later cleanup or v2 automation, not absorbed into this phase.

## Planning Implications By Roadmap Plan

### Plan 01-01: canonical structure

Should define:

- package tree
- required artifacts
- `SKILL.md` section contract
- template/checklist assets

### Plan 01-02: boundaries, handoff, portability

Should define:

- decision rules for continue / handoff / return-to-orchestrator
- structured handoff payload
- adapter support matrix
- semantic parity policy

### Plan 01-03: examples, references, validation

Should define:

- example minimums
- reference quality rules
- validation checklist and evidence expectations
- minimal retrofit of existing skills to prove the standard works

## Validation Architecture

Phase 1 validation should be split into three layers so a Nyquist validation file can check both documents and real repo state.

### Layer 1: Structural conformance

Validate that the standard explicitly defines:

- required package artifacts
- required `SKILL.md` sections
- optional adapter artifacts
- minimum examples and validation files
- handoff payload fields

This can be checked by reviewing the central standard documents and any template/checklist files created in the phase.

### Layer 2: Retrofit proof on existing skills

Validate that `aspire` and `masstransit` demonstrate the standard in practice:

- both still have canonical `SKILL.md`
- both retain existing usable adapter assets
- both gain `examples/` and `validation/` artifacts or an explicitly documented equivalent accepted by the standard
- references remain focused topic files

This proves the standard is implementable without breaking current repo patterns.

### Layer 3: Semantic integrity

Validate that:

- adapter text does not introduce instructions absent from `SKILL.md`
- handoff rules name one next owner or orchestrator return
- examples include one happy-path and one boundary/handoff case
- validation artifacts require evidence, not only a checklist
- official reference links resolve or are explicitly verified during review

### Suggested Nyquist validation checks

- Document review against `STND-01` through `STND-04`, `PORT-01`, and `PORT-02`
- File-tree inspection for required new standard artifacts
- File-tree inspection for `aspire` and `masstransit` example/validation additions
- Spot comparison of adapter files against canonical `SKILL.md`
- Link verification for official references named by the standard

### Expected evidence outputs from this phase

- checklist showing each standard requirement is defined
- checklist showing each existing skill meets the Phase 1 retrofit bar
- notes on any consciously deferred adapter gaps
- link-check or manual verification note for official docs

## Recommended Planner Assumptions

- The phase should end with a documented standard plus proof-of-fit on current skills.
- Minimal retrofit is enough; full backfill and automation are not required in Phase 1.
- Adapter inconsistency should be tolerated only where the policy explicitly allows optional adapters.
- The standard must be strict on semantics and flexible on adapter presence.

## Open Decisions To Resolve During Planning

- Where the canonical standard lives: one document or a small set of focused standard docs
- Whether `masstransit` must gain a `CLAUDE.md` adapter in this phase or only be documented as adapter-incomplete but still valid
- How much `SKILL.md` reordering to require immediately for existing skills versus for new skills only
- Whether version stamps belong in each reference file, validation evidence file, or both
