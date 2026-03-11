# Reference And Validation Policy

## Reference Rules

- Each v1 skill must include focused official references.
- References should be grouped by topic, not collected into one generic dump.
- Version-sensitive guidance should record the relevant version or baseline.
- Dead links are a validation failure until corrected or explicitly replaced.

## Example Rules

- Every v1 skill must ship:
  - one happy-path example
  - one boundary/handoff example
- Pseudo-code is allowed only when clearly labeled.

## Validation Rules

- Every v1 skill must include `validation/checklist.md`.
- Every v1 skill must include `validation/evidence.md`.
- Validation evidence must record:
  - links verified
  - verification date
  - reference owner
  - adapter parity outcome
  - differences found
  - allowed gap, if any
  - limitations

## Completion Standard

A skill is not complete when:
- required examples are missing
- official links have not been checked
- adapter parity has not been reviewed where adapters exist
- validation evidence is absent

