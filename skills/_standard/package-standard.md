# Skill Package Standard

## Purpose

This document defines the canonical package shape for specialist skills in this repo. It exists so every new skill can be authored, reviewed, and routed consistently across mixed AI-agent environments.

## Canonical Package Tree

```text
skills/<skill-name>/
  SKILL.md
  references/
    <topic>.md
  examples/
    happy-path.md
    boundary-handoff.md
  validation/
    checklist.md
    evidence.md
  .claude-plugin/                  # optional adapter
    plugin.json
    CLAUDE.md
  skills/<skill-name>/CLAUDE.md    # optional adapter
```

## Required Artifacts

- `SKILL.md`
  The canonical source of truth for skill semantics.
- `references/`
  Focused topic files that support the canonical skill without turning `SKILL.md` into a dump of copied documentation.
- `examples/happy-path.md`
  One realistic example showing the skill operating fully within its own boundary.
- `examples/boundary-handoff.md`
  One realistic example showing the skill stopping, naming the next owner, or returning control to the orchestrator.
- `validation/checklist.md`
  The per-skill completion checklist used during implementation and review.
- `validation/evidence.md`
  A record of what was actually checked, what links were verified, and what limitations remain.

## Optional Artifacts

- `.claude-plugin/`
  Runtime-specific packaging for Claude-related discovery and invocation.
- `skills/<skill-name>/CLAUDE.md`
  Runtime-specific adapter surface when a nested CLAUDE skill representation is needed.

Optional means adapter presence is not required for a skill to be valid. If adapter files exist, they must remain semantically aligned with `SKILL.md`.

## Rules

- `SKILL.md` is canonical. Adapters re-express intent; they do not redefine it.
- Keep runtime-specific logic out of canonical docs.
- Use additive normalization for existing skills. Phase 1 should introduce missing standard artifacts without broad rewrites.
- Keep references focused and topic-based.
- Treat examples and validation as first-class package content, not optional nice-to-haves.

## Deferred Items

- Generated adapters
- Automated conformance tooling
- Additional runtime adapter shapes beyond the currently observed Claude-related patterns

