# Boundary / Handoff Example

## Scenario

The user asks for MassTransit consumer setup, EF Core transaction design, and AppHost resource composition in one change.

## Expected Decision

- Keep MassTransit transport, consumer, middleware, and saga guidance in scope.
- Name `efcore` as the next owner for transaction and persistence design.
- Name `aspire` as the next owner for AppHost/resource composition.
- Return to orchestrator because more than one next owner is involved.

## Handoff Payload

- current objective: align the messaging workflow with persistence and hosting
- completed work: defined the messaging responsibilities, transport guidance, and middleware expectations
- unresolved risks: persistence/outbox details and hosting composition are still open
- next decision or question: should persistence or hosting be resolved first

## Why The Current Skill Stops

Persistence and hosting ownership extend beyond MassTransit’s specialty.

