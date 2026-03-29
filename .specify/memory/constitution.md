<!--
Sync Impact Report
===================
- Version change: N/A → 1.0.0 (initial ratification)
- Added principles:
  - I. Code Quality
  - II. Testing Standards
  - III. User Experience Consistency
  - IV. Performance Requirements
- Added sections:
  - Development Workflow
  - Governance
- Removed sections: none
- Templates requiring updates:
  - .specify/templates/plan-template.md ✅ no changes needed (Constitution Check is generic)
  - .specify/templates/spec-template.md ✅ no changes needed (success criteria already support performance/UX metrics)
  - .specify/templates/tasks-template.md ✅ no changes needed (test phases and polish phase align with principles)
- Follow-up TODOs: none
-->

# RAG Application Constitution

## Core Principles

### I. Code Quality

All code MUST be clear, maintainable, and reviewable. Specific rules:

- Functions MUST have a single responsibility and be no longer than
  50 lines (excluding declarations and closing braces). Split when
  exceeded.
- Every module MUST expose a clean public API. Internal details MUST
  NOT leak across module boundaries.
- No dead code, commented-out blocks, or unused imports in committed
  code. Linters MUST enforce this automatically.
- All code MUST pass static analysis (type checking, linting) with
  zero warnings before merge. Suppressions require an inline
  justification comment.
- Dependencies MUST be pinned to exact versions. New dependencies
  require documented justification in the PR description.

### II. Testing Standards

Every behavioral change MUST be accompanied by tests that prove it
works and guard against regression. Specific rules:

- Unit tests MUST cover all public functions and edge cases. Target
  line coverage MUST be at least 80% for new code.
- Integration tests MUST verify cross-module interactions: API
  endpoints, database queries, external service contracts, and
  retrieval pipeline accuracy.
- Tests MUST be deterministic. Flaky tests MUST be quarantined and
  fixed within one sprint or deleted.
- Test names MUST describe the scenario and expected outcome (e.g.,
  `test_search_returns_empty_when_no_documents_match`).
- Mocking is permitted only at system boundaries (HTTP clients,
  databases, external APIs). Internal module interactions MUST NOT
  be mocked.

### III. User Experience Consistency

The application MUST present a coherent, predictable interface to
every user across all interaction surfaces. Specific rules:

- All user-facing error messages MUST be actionable: state what went
  wrong, why, and what the user can do about it. Raw stack traces or
  internal codes MUST NOT be exposed.
- Response formats MUST be consistent: identical field names, casing
  conventions, and pagination structures across all API endpoints.
- Loading states, empty states, and error states MUST be handled
  explicitly in every user-facing flow. No unhandled promise
  rejections or silent failures.
- Accessibility standards (WCAG 2.1 AA) MUST be met for any
  frontend component. Semantic HTML, ARIA labels, and keyboard
  navigation are non-negotiable where a UI exists.
- Search and retrieval results MUST include relevance indicators so
  users can judge answer quality.

### IV. Performance Requirements

The system MUST meet defined latency and throughput targets under
expected load. Specific rules:

- API endpoint responses MUST complete within 500ms at p95 for
  non-retrieval queries and within 2000ms at p95 for RAG retrieval
  queries.
- Document ingestion MUST process at least 100 documents per minute
  in steady state.
- Memory usage per request MUST NOT exceed 512MB. Long-running
  processes MUST release resources promptly.
- Database queries MUST use indexes. Any full table scan on a table
  exceeding 10,000 rows MUST be flagged and optimized before merge.
- Performance-critical paths MUST have benchmarks committed alongside
  the code. Regressions exceeding 10% MUST block the PR.

## Development Workflow

- Every PR MUST be reviewed by at least one other contributor before
  merge.
- Commits MUST be atomic: one logical change per commit with a
  descriptive message.
- The main branch MUST always be in a deployable state. Broken builds
  MUST be fixed or reverted within 1 hour.
- Feature branches MUST be rebased onto main before merge to maintain
  a linear history.
- All CI checks (lint, type check, tests, performance benchmarks)
  MUST pass before a PR can be merged.

## Governance

This constitution is the authoritative source of project standards.
All development work, reviews, and architectural decisions MUST
comply with the principles defined above.

- **Amendments**: Any change to this constitution MUST be proposed as
  a PR with a clear rationale. The change MUST be approved by the
  project maintainer before merge.
- **Versioning**: The constitution follows semantic versioning.
  MAJOR: principle removed or redefined incompatibly. MINOR: new
  principle or material expansion. PATCH: wording clarifications.
- **Compliance review**: Each PR review MUST include a constitution
  compliance check. The plan template's "Constitution Check" gate
  enforces this at the feature planning stage.
- **Conflict resolution**: If a development practice conflicts with
  this constitution, the constitution wins. Update the practice or
  amend the constitution — do not silently diverge.

**Version**: 1.0.0 | **Ratified**: 2026-03-29 | **Last Amended**: 2026-03-29
