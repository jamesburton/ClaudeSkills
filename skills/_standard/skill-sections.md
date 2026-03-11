# Canonical `SKILL.md` Section Contract

This document defines the fixed section order and minimum section intent for all new v1 skills. Maintainers should not rely on examples, adapter files, or implied structure to understand what belongs in a skill.

## Fixed section order

Every canonical `SKILL.md` must follow this order:

1. Front matter
2. Title and summary
3. Scope
4. In-scope work
5. Out-of-scope work
6. Boundary and handoff rules
7. Workflow
8. Output expectations
9. References map
10. Validation expectations
11. Adapter notes

## Section intent

### 1. Front matter

The front matter provides neutral metadata for discovery and routing.

It must communicate:

- Stable skill name
- Short description of when to use the skill
- Allowed tools or equivalent neutral capability metadata already used by the repo

It must not communicate runtime-specific control flow that changes the meaning of the skill.

### 2. Title and summary

The title and summary define the specialty in one compact block.

It must communicate:

- What domain the skill owns
- Why the skill exists
- The level of opinionation or specialization expected

### 3. Scope

The Scope section states the specialty boundary in one short paragraph.

It must communicate:

- The exact work domain the skill is designed to handle
- The expected problem space and artifacts under its ownership
- The viewpoint the agent should maintain while operating within the skill

### 4. In-scope work

The In-scope section makes allowed work explicit.

It must communicate:

- Which tasks the specialist should continue doing without escalation
- Typical artifacts, decisions, or reviews that stay within the specialty
- Clear positive examples of work the skill should own

### 5. Out-of-scope work

The Out-of-scope section makes non-owned work explicit.

It must communicate:

- Adjacent domains this skill must not absorb
- Conditions that require a direct handoff or return to the orchestrator
- Clear negative examples that prevent boundary drift

### 6. Boundary and handoff rules

Boundary and handoff rules are mandatory and must be explicit.

It must communicate:

- When the specialist should continue because the work remains inside the specialty
- When it may hand off directly because exactly one next owner is clear
- When it must stop and return control to the orchestrator because multiple specialties or sequencing decisions are involved
- The required handoff payload: current objective, completed work, unresolved risks, and the exact next decision or question

Boundary and handoff rules are the canonical location for these decisions. Maintainers must not bury them inside workflow examples.

### 7. Workflow

The Workflow section defines how the specialist approaches work inside its boundary.

It must communicate:

- The normal operating sequence for analysis, implementation, and review
- Any domain-specific checkpoints that improve output quality
- When the workflow stops because the work has crossed a boundary

The workflow should stay runtime-neutral and describe intent rather than product-specific mechanics.

### 8. Output expectations

Output expectations define what the specialist returns after acting.

It must communicate:

- What a successful response, artifact, or review should include
- What evidence or rationale must be returned to the caller
- How unresolved issues, limitations, or follow-up questions must be surfaced

### 9. References map

The References map points agents to focused source material.

It must communicate:

- Which files under `references/` cover which topics
- How to choose the relevant reference file without reading everything
- Where official documentation sources are anchored for later verification

The References map must point to topics, not merely say "see references."

### 10. Validation expectations

Validation expectations define what must be true before the skill can claim completion.

It must communicate:

- The minimum validation checklist the skill must satisfy
- The evidence that must be captured in `validation/evidence.md`
- The expectation that official links are verified and limitations are recorded

Validation expectations should make completion criteria reviewable without reading examples alone.

### 11. Adapter notes

Adapter notes document how runtime-specific projections relate to the canonical skill.

It must communicate:

- That `SKILL.md` is canonical
- Which adapter surfaces are optional
- That adapters may restate but must not change the meaning of the canonical skill
- How allowed adapter absence should be treated when an adapter is not present

## Boundary and handoff rules checklist

Use this checklist when reviewing a canonical `SKILL.md`:

- `Scope`, `In-scope work`, and `Out-of-scope work` do not contradict each other.
- `Boundary and handoff rules` names continue, direct handoff, and return-to-orchestrator conditions explicitly.
- `Workflow` does not silently expand ownership beyond the boundary contract.
- `Output expectations` requires a structured handoff or escalation payload when the skill stops.

## Portability guardrails

- Keep section semantics runtime-neutral so mixed-agent environments can follow the same canonical file.
- Do not move boundary logic into adapter-only content.
- Do not rely on front matter or plugin metadata to replace canonical sections.
