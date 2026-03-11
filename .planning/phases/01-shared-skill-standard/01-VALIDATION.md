---
phase: 01
slug: shared-skill-standard
status: planned
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-11
---

# Phase 01 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | other — repo/document validation |
| **Config file** | none — plan-local checks define commands |
| **Quick run command** | `git diff --name-only -- .planning skills && Get-ChildItem .planning\\phases\\01-shared-skill-standard,skills -Recurse | Select-Object FullName` |
| **Full suite command** | `Get-ChildItem skills -Directory | ForEach-Object { Get-ChildItem $_.FullName -Recurse }` |
| **Estimated runtime** | ~15 seconds |

---

## Sampling Rate

- **After every task commit:** Run `git diff --name-only -- .planning skills && Get-ChildItem .planning\phases\01-shared-skill-standard,skills -Recurse | Select-Object FullName`
- **After every plan wave:** Run `Get-ChildItem skills -Directory | ForEach-Object { Get-ChildItem $_.FullName -Recurse }`
- **Before `$gsd-verify-work`:** Full suite must be green
- **Max feedback latency:** 30 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 01-01-01 | 01 | 1 | STND-01 | document review | `Test-Path 'skills/_standard/package-standard.md'` | ❌ W0 | ⬜ pending |
| 01-01-02 | 01 | 1 | STND-01 | document review | `Test-Path 'skills/_standard/skill-sections.md'` | ❌ W0 | ⬜ pending |
| 01-01-03 | 01 | 1 | PORT-01 | traceability review | `Select-String -Path 'skills/_standard/requirements-traceability.md' -Pattern 'STND-01|STND-02|STND-03|STND-04|PORT-01|PORT-02'` | ❌ W0 | ⬜ pending |
| 01-02-01 | 02 | 2 | STND-02 | policy review | `Select-String -Path 'skills/_standard/boundary-and-portability.md' -Pattern 'orchestrator|current objective|completed work|unresolved risks|next decision'` | ❌ W0 | ⬜ pending |
| 01-02-02 | 02 | 2 | PORT-02 | parity-template review | `Select-String -Path 'skills/_standard/checklists/adapter-parity-checklist.md','skills/_standard/templates/validation/adapter-parity-evidence.template.md' -Pattern 'semantic drift|parity status|deferred gap|allowed absence'` | ❌ W0 | ⬜ pending |
| 01-02-03 | 02 | 2 | STND-04 | validation-template review | `Select-String -Path 'skills/_standard/templates/validation/checklist.template.md','skills/_standard/templates/validation/evidence.template.md' -Pattern 'official reference|evidence|links verified|limitations'` | ❌ W0 | ⬜ pending |
| 01-03-01 | 03 | 3 | STND-03 | file inspection | `Get-ChildItem 'skills/aspire/examples','skills/masstransit/examples' | Select-Object FullName` | ❌ W0 | ⬜ pending |
| 01-03-02 | 03 | 3 | STND-04 | reference review | `Select-String -Path 'skills/aspire/validation/evidence.md','skills/masstransit/validation/evidence.md','.planning/phases/01-shared-skill-standard/01-03-retrofit-proof.md' -Pattern 'links verified|reference owner|verification date|status'` | ❌ W0 | ⬜ pending |
| 01-03-03 | 03 | 3 | PORT-01 | live adapter surface review | `Test-Path 'skills/aspire/.claude-plugin/plugin.json'; Test-Path 'skills/aspire/skills/aspire/CLAUDE.md'; Test-Path 'skills/masstransit/.claude-plugin/plugin.json'` | ✅ | ⬜ pending |
| 01-03-04 | 03 | 3 | PORT-02 | allowed-absence review | `if (Test-Path 'skills/masstransit/skills/masstransit/CLAUDE.md') { 'present' } else { 'allowed-absence-to-document' }` | ✅ | ⬜ pending |
| 01-03-05 | 03 | 3 | PORT-01 | parity evidence review | `Select-String -Path 'skills/aspire/validation/evidence.md','skills/masstransit/validation/evidence.md','.planning/phases/01-shared-skill-standard/01-03-retrofit-proof.md' -Pattern 'adapter parity|differences found|allowed gap|adapter parity outcome'` | ❌ W0 | ⬜ pending |
| 01-03-06 | 03 | 3 | PORT-02 | retrofit-proof ledger review | `Select-String -Path '.planning/phases/01-shared-skill-standard/01-03-retrofit-proof.md' -Pattern 'PORT-01|PORT-02|STND-04|evidence path|allowed deferred gap|reference verification outcome|allowed absence'` | ❌ W0 | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] `skills/_standard/` base documents and templates — create the canonical package, section, portability, and validation rules before retrofit verification
- [ ] `skills/aspire/examples/` and `skills/masstransit/examples/` — create baseline example locations required by the standard
- [ ] `skills/aspire/validation/` and `skills/masstransit/validation/` — create baseline validation locations required by the standard
- [ ] `.planning/phases/01-shared-skill-standard/01-03-retrofit-proof.md` — create a phase-level retrofit proof ledger for parity and reference verification

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Standard semantics are clear and not self-contradictory | STND-01, STND-02 | Requires judgment across multiple docs | Read the standard docs and confirm required artifacts, section order, and handoff rules are internally consistent |
| Adapter guidance does not drift from canonical intent | PORT-01, PORT-02 | Needs semantic comparison, not just file presence | Review each skill's evidence plus the retrofit-proof ledger, confirm the live adapter files that exist were compared against `SKILL.md`, and confirm any missing adapter surface is recorded as an allowed absence rather than silently ignored |
| Official reference policy is strict enough for future skills | STND-04 | Requires policy review, not only syntax checks | Confirm the standard defines live-link expectations and evidence expectations, then compare that policy to each skill's recorded reference-verification evidence |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 30s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
