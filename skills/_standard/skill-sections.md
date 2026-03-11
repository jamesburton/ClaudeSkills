# SKILL.md Section Contract

## Required Order

Every new v1 skill should use this fixed section order so maintainers and agents can scan skills predictably.

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

## Section Intent

### Front matter

Required keys:
- `name`
- `description`
- `allowed-tools` when tool restrictions matter

This metadata should stay runtime-neutral and describe when the skill should trigger.

### Title / summary

One short heading and a concise framing paragraph that states the specialty clearly.

### Scope

Define the skill's bounded specialty in one tight paragraph. This is the primary ownership statement.

### In-scope work

List the kinds of tasks the specialist should continue working on directly.

### Out-of-scope work

List adjacent concerns the skill must not absorb. These bullets should be explicit, not implied by omission.

### Boundary and Handoff Rules

This section must define:
- when the skill continues
- when it hands directly to one named next owner
- when it returns to the orchestrator
- the required handoff payload:
  - current objective
  - completed work
  - unresolved risks
  - exact next decision or question

### Working Method / Workflow

Describe the operational steps the specialist follows when handling in-scope work.

### Output Expectations

State what the skill should return: artifacts, summaries, decisions, validation evidence, or handoff notes.

### References Map

Point to focused `references/*.md` files by topic, so the reader knows what to open for which concern.

### Validation Expectations

Describe what must be true before the skill can claim success. This must align with the validation templates and reference policy.

### Adapter Notes

State that runtime adapters are optional thin projections of the canonical `SKILL.md` and must not introduce semantic drift.

