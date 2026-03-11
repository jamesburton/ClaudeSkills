---
phase: 02
slug: opentelemetry-specialist
status: planned
nyquist_compliant: true
wave_0_complete: false
created: 2026-03-11
---

# Phase 02 — Validation Strategy

> Per-phase validation contract for feedback sampling during execution.

---

## Test Infrastructure

| Property | Value |
|----------|-------|
| **Framework** | other — repo/document validation |
| **Config file** | none — plan-local checks define commands |
| **Quick run command** | `Get-ChildItem 'skills/opentelemetry-dotnet' -Recurse | Select-Object FullName` |
| **Full suite command** | `Get-ChildItem 'skills/opentelemetry-dotnet','skills/aspire','skills/masstransit' -Recurse | Select-Object FullName` |
| **Estimated runtime** | ~15 seconds |

---

## Sampling Rate

- **After every task commit:** Run `Get-ChildItem 'skills/opentelemetry-dotnet' -Recurse | Select-Object FullName`
- **After every plan wave:** Run `Get-ChildItem 'skills/opentelemetry-dotnet','skills/aspire','skills/masstransit' -Recurse | Select-Object FullName`
- **Before `$gsd-verify-work`:** Full suite must be green
- **Max feedback latency:** 30 seconds

---

## Per-Task Verification Map

| Task ID | Plan | Wave | Requirement | Test Type | Automated Command | File Exists | Status |
|---------|------|------|-------------|-----------|-------------------|-------------|--------|
| 02-01-01 | 01 | 1 | OBS-01 | file inspection | `Test-Path 'skills/opentelemetry-dotnet/SKILL.md'` | ❌ W0 | ⬜ pending |
| 02-01-02 | 01 | 1 | OBS-01 | content review | `Select-String -Path 'skills/opentelemetry-dotnet/SKILL.md' -Pattern 'traces|metrics|logs|OTLP|service.name|semantic conventions'` | ❌ W0 | ⬜ pending |
| 02-01-03 | 01 | 1 | OBS-01 | file inspection | `Get-ChildItem 'skills/opentelemetry-dotnet/references','skills/opentelemetry-dotnet/examples' -Recurse | Select-Object FullName` | ❌ W0 | ⬜ pending |
| 02-01-04 | 01 | 1 | OBS-01 | reference quality review | `Select-String -Path 'skills/opentelemetry-dotnet/references/*.md' -Pattern 'https://|baseline|version|OTLP|collector'` | ❌ W0 | ⬜ pending |
| 02-02-01 | 02 | 2 | OBS-02 | boundary review | `Select-String -Path 'skills/opentelemetry-dotnet/SKILL.md','skills/opentelemetry-dotnet/examples/boundary-handoff.md' -Pattern 'aspire|masstransit|orchestrator|next owner'` | ❌ W0 | ⬜ pending |
| 02-02-02 | 02 | 2 | OBS-01 | validation review | `Test-Path 'skills/opentelemetry-dotnet/validation/checklist.md'; Test-Path 'skills/opentelemetry-dotnet/validation/evidence.md'` | ❌ W0 | ⬜ pending |
| 02-02-03 | 02 | 2 | OBS-02 | reference/link review | `Select-String -Path 'skills/opentelemetry-dotnet/validation/evidence.md' -Pattern 'links verified|verification date|reference owner|adapter parity|limitations|baseline'` | ❌ W0 | ⬜ pending |
| 02-02-04 | 02 | 2 | OBS-01 | link liveness review | `$urls = Select-String -Path 'skills/opentelemetry-dotnet/references/*.md' -Pattern 'https://[^ )]+' -AllMatches | ForEach-Object { $_.Matches.Value } | Sort-Object -Unique; foreach ($u in $urls) { try { (Invoke-WebRequest -Uri $u -Method Head -MaximumRedirection 5 -ErrorAction Stop).StatusCode } catch { 'ERROR' } }` | ❌ W0 | ⬜ pending |
| 02-02-05 | 02 | 2 | OBS-02 | adapter optionality review | `if (Test-Path 'skills/opentelemetry-dotnet/.claude-plugin/plugin.json') { 'adapter-present' } else { 'adapter-allowed-absence' }` | ✅ | ⬜ pending |

*Status: ⬜ pending · ✅ green · ❌ red · ⚠️ flaky*

---

## Wave 0 Requirements

- [ ] `skills/opentelemetry-dotnet/` base package — create the canonical skill, references, examples, and validation folders
- [ ] example and boundary files — establish the required example pair before boundary verification
- [ ] validation artifacts — create checklist/evidence files before link and boundary proof can pass

---

## Manual-Only Verifications

| Behavior | Requirement | Why Manual | Test Instructions |
|----------|-------------|------------|-------------------|
| Equal-signal coverage feels balanced | OBS-01 | Requires judgment about emphasis, not just keyword presence | Read `SKILL.md` and references, confirm traces, metrics, and logs are treated as first-class rather than one dominant signal with token coverage of the others |
| Boundary guidance is genuinely sharp | OBS-02 | Needs semantic review, not just named references | Confirm the skill distinguishes telemetry policy from Aspire hosting and MassTransit behavior, and uses orchestrator return when more than one next owner is involved |
| Examples are realistic but focused | OBS-01, OBS-02 | Example quality cannot be fully automated | Review the Aspire happy-path and Aspire+MassTransit boundary examples and confirm they are production-shaped without becoming tutorials |

---

## Validation Sign-Off

- [ ] All tasks have `<automated>` verify or Wave 0 dependencies
- [ ] Sampling continuity: no 3 consecutive tasks without automated verify
- [ ] Wave 0 covers all MISSING references
- [ ] No watch-mode flags
- [ ] Feedback latency < 30s
- [x] `nyquist_compliant: true` set in frontmatter

**Approval:** pending
