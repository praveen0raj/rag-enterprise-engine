# Features Status

Tracks the lifecycle of all epics and PBIs from specification to implementation.

## Epic Status

| Epic | Title | Phase | Requirements | Diagrams | PBIs | Status |
|------|-------|-------|-------------|----------|------|--------|
<!-- EPICS START -->
| E-001 | Document Ingestion Pipeline | Phase 1 | [ ] | [ ] | [ ] | draft |
| E-002 | Basic Retrieval & Generation | Phase 1 | [ ] | [ ] | [ ] | draft |
| E-003 | API Foundation & Infrastructure | Phase 1 | [ ] | [ ] | [ ] | draft |
| E-004 | Hybrid Retrieval | Phase 2 | [ ] | [ ] | [ ] | draft |
| E-005 | Cross-Encoder Reranking | Phase 2 | [ ] | [ ] | [ ] | draft |
| E-006 | Citation Enforcement | Phase 2 | [ ] | [ ] | [ ] | draft |
| E-007 | Golden Evaluation Dataset | Phase 3 | [ ] | [ ] | [ ] | draft |
| E-008 | RAGAS Evaluation Pipeline | Phase 3 | [ ] | [ ] | [ ] | draft |
| E-009 | CI/CD Quality Gate | Phase 3 | [ ] | [ ] | [ ] | draft |
<!-- EPICS END -->

## PBI Status

| PBI | Epic | AC | Tech Design | Test Design | Impl | Progress | Status |
|-----|------|----|-------------|-------------|------|----------|--------|
<!-- PBIS START -->
<!-- PBIS END -->

---

## Status Legend

### Epic Columns

- **Requirements**: `[ ]` Not started, `[x]` Created and reviewed
- **Diagrams**: `[ ]` Not started, `[x]` Generated
- **PBIs**: `[ ]` Not decomposed, `[x]` All PBIs created

### PBI Columns

- **AC**: `[ ]` Not started, `[x]` Acceptance criteria created and reviewed
- **Tech Design**: `[ ]` Not started, `[x]` Created and reviewed
- **Test Design**: `[ ]` Not started, `[x]` Created and reviewed
- **Impl**: `[ ]` Not started, `[~]` In progress, `[x]` Complete
- **Progress**: AC item completion (e.g., `AC1,AC2/AC3` = AC1,AC2 done, AC3 remaining)

### Status Values

| Status | Meaning |
|--------|---------|
| `draft` | Epic/PBI created, no artifacts yet |
| `specifying` | Requirements analysis or AC in progress |
| `designing` | Tech design or test design in progress |
| `ready` | All design artifacts complete, ready to implement |
| `implementing` | Implementation in progress |
| `done` | All AC items verified, implementation complete |

## Status Transitions

### Epic Lifecycle

```
draft → specifying → designing → ready → implementing → done
  |         |            |         |          |           |
  |    requirements   diagrams    PBIs    PBI impl    all PBIs
  |     created       created    created   started      done
  |
 epic created
 (001-create-epic)
```

### PBI Lifecycle

```
draft → specifying → designing → ready → implementing → done
  |         |            |         |          |           |
  |        AC         tech +     all       TDD         all AC
  |      created     test design  docs     cycle       verified
  |                   created    complete  started
  |
 PBI created
 (from epic kickoff)
```

## Skill to Status Mapping

| Skill | Updates |
|-------|---------|
| 001-create-epic | Epic → `draft`, PBIs → `draft` |
| 002-create-requirements-analysis | Epic → `specifying` |
| 003-review-requirements-analysis | Epic Requirements → `[x]` |
| 004-create-epic-diagrams | Epic Diagrams → `[x]` |
| 005-create-acceptance-criteria | PBI AC → `[x]`, PBI → `specifying` |
| 006-review-acceptance-criteria | PBI AC reviewed |
| 007-create-tech-design | PBI Tech Design → `[x]`, PBI → `designing` |
| 008-review-tech-design | PBI Tech Design reviewed |
| 009-create-test-design | PBI Test Design → `[x]` |
| 010-review-test-design | PBI → `ready` |
| 011-implement-pbi | PBI Impl → `[~]`/`[x]`, PBI → `implementing`/`done` |
| 012-create-tech-docs | Orchestrates 007-010, creates PR |

## Progress Column Format

| Format | Meaning |
|--------|---------|
| `-` | No implementation started |
| `AC1` | AC item 1 complete |
| `AC1,AC2` | AC items 1 and 2 complete |
| `AC1,AC2/AC3` | AC1,AC2 done, AC3 remaining |
| `AC1,AC2,AC3` | All AC items complete (Impl = `[x]`) |
