---
description: Implement a PBI using TDD based on acceptance criteria, tech design, and test design from specs/. Creates implementation branch, runs quality gates, and optionally creates PR. Requires tech_design.md to exist.
---

## Phase 0: Setup

### Step 0.1: Identify the PBI

Same logic as `pbi-tech-doc`:
- If argument provided → find matching PBI directory in `specs/epics/*/pbis/*{argument}*`
- If no argument → list all PBIs, let user pick
- Store `EPIC_NAME`, `PBI_NAME`, `PBI_DIR`

### Step 0.2: Verify Tech Design Exists (GATE)

```bash
find {PBI_DIR} -name "tech_design.md" -type f
```

- **Found** → continue
- **Not found** → **STOP**: "No tech_design.md found. Run `/012-create-tech-docs` first to create tech documents."

### Step 0.3: Read All Specs

Read these files in parallel:
- `{PBI_DIR}/acceptance_criteria.md`
- `{PBI_DIR}/tech_design.md`
- `{PBI_DIR}/test_design.md` (if exists)

Summarize: what needs to be built, which layers are affected (backend/frontend/migration/both).

### Step 0.4: Create Implementation Branch

```bash
git checkout main && git pull origin main
git checkout -b feature/{PBI_NAME}
```

If already on a feature branch, confirm with user: "Use current branch `{branch}` or create new?"

### Step 0.5: Generate Task List

Check if `{PBI_DIR}/tasks.md` exists. If not, generate from tech design:

- Break the tech design into ordered implementation tasks
- Each task should be small enough to complete in one TDD cycle
- Order tasks by dependency (foundations first, then features, then integration)
- Format:

```markdown
## Implementation Tasks

### Backend
- [ ] T1: {task description} — {which AC items this covers}
- [ ] T2: {task description}

### Frontend
- [ ] T3: {task description}

### Migration
- [ ] T4: {task description}

### Integration
- [ ] T5: {task description}
```

Save to `{PBI_DIR}/tasks.md` and present to user for confirmation.

### Step 0.6: Write State File

Create `.pbi-implement-state.md` with all setup values.

---

## Phase 1: TDD Implementation

### Execution Loop

For each task in the task list:

#### 1. Announce Current Task

```
## Task T{N}: {description}
Related AC: {AC items}
Related tests: {test items from test_design.md}
```

#### 2. Red — Write Failing Test

- Based on the test design (`test_design.md`) and acceptance criteria
- Write the test FIRST — it must fail initially
- Use existing test patterns from the codebase (table-driven tests, gomock, etc.)

#### 3. Green — Implement Minimum Code

- Write minimum code to make the test pass
- Follow the tech design approach
- Do NOT over-engineer — just make the test green

#### 4. Refactor

- Clean up the implementation while keeping tests green
- Apply existing codebase patterns and conventions
- For Medium/Complex tasks only — skip for simple changes

#### 5. Verify

Run quality checks (backend and frontend in parallel if both changed):

**Backend** (if Go files changed):
```bash
cd {PROJECT_ROOT}/backend && make test
```

**Frontend** (if TS/TSX files changed):
```bash
cd {PROJECT_ROOT}/frontend && pnpm typecheck && pnpm lint
```

If any check fails → fix immediately before moving to next task.

#### 6. Update Checkboxes

- Mark task as `[x]` in tasks.md
- Mark related AC items as `[x]` in acceptance_criteria.md (if fully covered)
- Mark related test items as `[x]` in test_design.md (if implemented)
- Update `.pbi-implement-state.md`

#### 7. Progress Report

```
## Progress: T{N} Complete ({completed}/{total})
- Tests: {pass count}/{total count}
- AC coverage: {covered}/{total}
- Next: T{N+1} — {description}
```

### Sub-Agent Delegation

Use this decision flow for each task:

1. **Is the task independent?** No (depends on previous) → direct. Yes → continue.
2. **Wide investigation needed?** 3+ files → Explore agent. Single file → direct.
3. **Multi-file changes with no user input needed?** → general-purpose agent.
4. **Needs user decision mid-task?** → direct.

When delegating, include:
- Task purpose (quote the AC item)
- Target files/directories
- Constraints (patterns to follow, things to avoid)
- Completion criteria

### Parallel Execution

Launch independent tasks simultaneously in a single message with multiple tool calls.
Examples:
- Backend + frontend changes that don't depend on each other
- Multiple test files that test different features
- Migration + seed data (if independent)

---

## Phase 2: Quality Gates

After all tasks are complete:

### Step 2.1: Full Quality Check

Run all checks in parallel:

**Backend**:
```bash
cd {PROJECT_ROOT}/backend && make check
```

**Frontend** (if frontend files changed):
```bash
cd {PROJECT_ROOT}/frontend && pnpm typecheck && pnpm lint && pnpm build
```

ALL must pass before proceeding.

### Step 2.2: Self-Review (3 Rounds)

For each round (1-3):

1. **Review** the implementation against:
   - Code readability
   - Error handling
   - Performance concerns
   - Security concerns
   - Test coverage
   - Design principles (DRY, single responsibility)
   - Alignment with tech design

2. **Report** findings to user

3. **Fix** issues and re-run failing checks

```
## Review Round {X}/3 Complete

### Findings
- {finding 1}
- {finding 2}

### Fixes Applied
- {fix 1}
- {fix 2}

### Test Result: PASS/FAIL
```

### Step 2.3: Manual Test Handoff

If test_design.md has manual test items:

1. Start the application:
   - Backend: `cd {PROJECT_ROOT}/backend && make run`
   - Frontend: `cd {PROJECT_ROOT}/frontend && pnpm dev`

2. Present manual test checklist to user with clear steps

3. Wait for user feedback. Fix issues if any.

### Step 2.4: Final Verification

Confirm all complete:
- [ ] All tasks `[x]`
- [ ] All AC items `[x]`
- [ ] All automated tests pass
- [ ] All manual tests pass (or signed off by user)
- [ ] Quality checks green

---

## Phase 3: Completion (Optional)

### Step 3.1: Commit

```bash
git add -A && git status
```

Show staged files to user. Then commit:

```bash
git commit -m "feat: implement {PBI display name}

- {summary of changes}
- AC: all items verified
- Tests: {auto count} automated, {manual count} manual
"
```

### Step 3.2: Push & PR

AskUserQuestion: "Create PR?"
- **Yes** → push and create PR
- **Not yet** → just push, no PR
- **Skip** → don't push

If creating PR:
```bash
git push -u origin feature/{PBI_NAME}
gh pr create \
  --title "feat: {PBI display name}" \
  --assignee "{GH_USER}" \
  --body "## {PBI display name}

### Summary
{implementation summary}

### Specs
- AC: epics/{EPIC}/pbis/{PBI}/acceptance_criteria.md
- Tech Design: epics/{EPIC}/pbis/{PBI}/tech_design.md
- Test Design: epics/{EPIC}/pbis/{PBI}/test_design.md

### Test Plan
- Automated: {count} tests passing
- Manual: {count} tests verified

### Checklist
- [x] All AC items verified
- [x] All automated tests pass
- [x] Quality checks pass (lint, typecheck, build)
- [x] Manual tests completed
"
```

### Step 3.3: Clean Up

Delete `.pbi-implement-state.md`.

---

## Error Handling

| Scenario | Action |
|----------|--------|
| Tech design not found | STOP: "Run /012-create-tech-docs first" |
| Test fails after implementation | Fix before moving to next task |
| Quality check fails | Fix immediately, re-run only failing check |
| User rejects manual test | Fix issue, re-run automated tests, re-request manual test |
| Sub-agent fails | Review error, handle in main thread |
| Context compaction | Read `.pbi-implement-state.md` to recover |

## Context Recovery

If context seems incomplete:
1. Read `.pbi-implement-state.md`
2. `git branch --show-current` to confirm branch
3. `git diff main...HEAD --stat` to see what's been done
4. Resume from the next incomplete task