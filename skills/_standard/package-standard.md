# Canonical Specialist Skill Package Standard

This document defines the v1 package contract for every specialist skill in this repository. It establishes one canonical package tree, distinguishes required artifacts from optional adapter surfaces, and keeps `SKILL.md` as the source of truth for mixed-agent portability.

## Package goals

- Make `SKILL.md` the canonical semantic source for every skill.
- Keep supporting material in stable, discoverable directories instead of embedding everything in one file.
- Preserve mixed-agent portability by treating runtime adapters as optional projections, not alternate sources of truth.
- Defer generated adapters and runtime-specific canonical logic until after Phase 1.

## Canonical package tree

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
  .claude-plugin/                 # optional adapter surface
    plugin.json
    CLAUDE.md
  skills/<skill>/CLAUDE.md        # optional adapter surface
```

Package rules that define the canonical `SKILL.md` contract are specified in [`skill-sections.md`](./skill-sections.md).

## Required artifacts

Every new v1 skill package must include these artifacts:

| Artifact | Required | Purpose |
|---------|----------|---------|
| `SKILL.md` | Yes | Canonical skill contract and semantics |
| `references/` | Yes | Focused source-backed topic files for official guidance |
| `examples/` | Yes | Canonical examples, including a happy path and a boundary or handoff case |
| `validation/checklist.md` | Yes | Completion checklist used before claiming the skill is done |
| `validation/evidence.md` | Yes | Evidence ledger showing what was verified, what remains limited, and link-check status |

Required means the skill is incomplete without the artifact. A maintainer must not infer required content from examples or adapters alone.

## Optional artifacts

These artifacts are optional in v1:

| Artifact | Required | Purpose |
|---------|----------|---------|
| `.claude-plugin/` | No | Runtime-specific Claude adapter packaging when needed |
| `.claude-plugin/CLAUDE.md` | No | Claude-facing adapter instructions when that adapter surface is used |
| `skills/<skill>/CLAUDE.md` | No | Nested Claude adapter projection when that shape is used by the runtime |

Optional means the artifact is not required for validity. If an optional adapter exists, it must remain semantically aligned with the canonical `SKILL.md`.

## Artifact rules

### `SKILL.md`

- `SKILL.md` is the only canonical semantic source.
- It must use the fixed section order defined in [`skill-sections.md`](./skill-sections.md).
- It must describe the specialty, boundaries, workflow, outputs, references, validation expectations, and adapter notes without relying on runtime-specific behavior.

### `references/`

- Store focused topic files rather than one catch-all reference dump.
- Reference files should support direct lookup by topic such as configuration, testing, or integrations.
- Official documentation sources should be easy to verify and trace from the canonical skill.

### `examples/`

- `examples/` is a first-class directory, not an appendix.
- Every skill must include at least one realistic happy-path example and one boundary or handoff example.
- Examples may be executable or clearly labeled pseudo-code when execution is impractical.

### `validation/`

- `validation/checklist.md` defines the conditions for claiming the skill is complete.
- `validation/evidence.md` records what was actually verified, including link verification outcomes and known limitations.
- Validation content must be separate from examples so reviewers can distinguish expected behavior from verified evidence.

## Supported adapter patterns

Phase 1 explicitly supports the Claude adapter patterns already observed in this repository:

1. `.claude-plugin/` with `plugin.json`
2. `.claude-plugin/` with `CLAUDE.md`
3. Nested `skills/<skill>/CLAUDE.md`

The `aspire` skill demonstrates both `.claude-plugin/` and `skills/aspire/CLAUDE.md`. The `masstransit` skill demonstrates `.claude-plugin/plugin.json` without a nested `CLAUDE.md`. Both are valid package shapes in Phase 1 because adapters are optional thin projections.

## Adapter obligations

- Adapters may reformat, condense, or restate canonical instructions for a runtime.
- Adapters must not add new task ownership, boundary rules, or workflow semantics that are absent from `SKILL.md`.
- Adapter presence is optional; semantic parity is mandatory whenever an adapter exists.
- A missing optional adapter must be treated as an allowed absence, not as a hidden failure.

## Deferred items

The following are intentionally deferred beyond Phase 1:

- Generated adapters or adapter synthesis workflows
- Runtime-specific canonical logic inside `SKILL.md`
- Additional adapter families beyond the Claude-related patterns already observed in the repo
- Any requirement that every skill expose every optional adapter surface

## Review checklist

- One package tree clearly distinguishes required versus optional artifacts.
- `SKILL.md` is explicitly canonical.
- `examples/` and `validation/` are first-class directories.
- Supported adapter patterns match what the repo already uses.
- Deferred items exclude generated adapters and runtime-specific canonical logic.
