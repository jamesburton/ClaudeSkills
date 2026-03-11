---
name: "<skill-name>"
description: "Use when ..."
allowed-tools: Read, Write, Edit
---

# <Skill Name>

## Summary

One short paragraph describing the specialty.

## Scope

Define the bounded specialty in one tight paragraph.

## In-Scope Work

- Task class 1
- Task class 2

## Out-of-Scope Work

- Adjacent concern 1
- Adjacent concern 2

## Boundary And Handoff Rules

- Continue when the task stays inside this specialty.
- Hand off directly only when exactly one next owner is clear.
- Return to the orchestrator when multiple specialties or sequencing decisions are involved.

Required handoff payload:
- current objective
- completed work
- unresolved risks
- next decision or question

## Workflow

1. Read the relevant inputs.
2. Apply the specialist rules.
3. Validate the output or produce a handoff.

## Output Expectations

- What artifacts or decisions the skill should return
- What evidence or checks should accompany completion

## References Map

- `references/topic-a.md` — what it covers
- `references/topic-b.md` — what it covers

## Validation Expectations

- Two canonical examples exist
- Official references are checked
- Validation evidence is recorded

## Adapter Notes

Adapters are optional thin projections of `SKILL.md` and must not introduce semantic drift.

