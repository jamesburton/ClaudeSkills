# Boundary And Portability Policy

## Continue, Hand Off, Or Return

Every specialist skill must follow the same decision model:

- Continue when the requested work remains inside the declared specialty.
- Hand off directly only when exactly one next owner is obvious.
- Return to the orchestrator when multiple specialties are involved or sequencing decisions are needed.

## Required Handoff Payload

Every handoff must include:

- current objective
- completed work
- unresolved risks
- next decision or question

## Direct Handoff Rule

Direct handoff is allowed only when:

- the next owner is singular and explicit
- the current skill has completed its in-scope work
- no orchestration or prioritization decision remains

Otherwise the skill must stop and return control to the orchestrator.

## Portability Rules

- `SKILL.md` is the canonical source of meaning.
- Adapters are optional projections of canonical meaning.
- Adapters must not introduce semantic drift.
- Runtime-specific conveniences belong in adapter surfaces, not in canonical skill semantics.

## Allowed Adapter Absence

If a skill does not need a specific adapter surface, that absence is allowed. The absence must be documented explicitly in validation evidence when adapter parity is reviewed.

