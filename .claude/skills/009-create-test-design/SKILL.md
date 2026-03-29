---
description: Create test design draft from technical design for a PBI. Analyzes modifications, impact scope, risks, and produces categorized test cases with technique tagging.
---

# PBI Test Design

Create test design draft based on the technical design. Analyzes modifications, impact scope, risks, and produces categorized test cases.

## Pre-requisites

Read these files before starting:
- `.claude/skills/docs/pbi_question_format.md` — question format
- `.claude/skills/docs/test_design_review_perspectives.md` — test design review perspectives

**Parallel processing**: Code investigation, impact analysis, and file reading should be parallelized as much as possible.

## Path Convention

All paths use the **project root** as base:
- PBI directory: `epics/{EPIC}/pbis/{PBI}/`
- AC: `epics/{EPIC}/pbis/{PBI}/acceptance_criteria.md`
- Tech design: `epics/{EPIC}/pbis/{PBI}/tech_design.md`
- Test design output: `epics/{EPIC}/pbis/{PBI}/test_design.md`
- Q&A: `epics/{EPIC}/pbis/{PBI}/test_design_q_and_a.md`
- Investigation: `epics/{EPIC}/pbis/{PBI}/test_design_investigation.md`

## Processing Steps

### 1. Confirm PBI Directory

- Confirm the target EPIC and PBI
- Verify the directory exists

### 2. Confirm Input Documents

- Check for these documents and confirm with user:
    - AC: `{PBI_DIR}/acceptance_criteria.md`
    - Tech design: `{PBI_DIR}/tech_design.md`
- If either is missing, ask user to provide URL or path

### 3. Create Working Files

Create these two documents:
- Test draft: `{PBI_DIR}/test_design.md`
- Q&A document: `{PBI_DIR}/test_design_q_and_a.md`

### 4. Analyze (6-Step Process)

#### Step 4.1: List Modifications
- Based on the tech design, read the actual code to identify what changes are needed

#### Step 4.2: List Impact Scope
- Based on the tech design, read the actual code to investigate impact scope
- Analyze from the perspective of risks and test considerations
- Identify which features are affected

#### Step 4.3: List Risks
- Severity assessment of changes
- Impact assessment on existing functionality
- Security and performance impact assessment
- Data integrity impact verification

#### Step 4.4: List Test Cases with Systematic Techniques

Select applicable techniques from the catalog below based on PBI scope. Apply them systematically to produce test cases.

##### Technique Catalog — Trigger-Based Selection

**Specification-Based (Black Box) — Core techniques, apply first**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[EP]` | **Equivalence Partitioning** | Every input parameter (ALWAYS) | Valid/invalid partitions with one representative test per partition |
| `[BVA]` | **Boundary Value Analysis** | Parameters with ranges or limits (ALWAYS) | Tests at min, max, min-1, max+1 boundaries |
| `[DT]` | **Decision Table** | Multi-condition branches (2+ conditions interact) | One test per condition combination |
| `[ST]` | **State Transition** | Entities with lifecycle / status transitions | Tests for each valid/invalid transition |
| `[UC]` | **Use Case Testing** | PBI has multi-step user journeys | Tests based on main/alternate/exception flows |
| `[PW]` | **Pairwise / Combinatorial** | 3+ input parameters that interact | All-pairs combination coverage |
| `[EG]` | **Error Guessing** | Complex logic or historically buggy areas | Experience-based defect prediction tests |

**Structure-Based (White Box) — Apply when code structure matters**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[SC]` | **Statement Coverage** | New functions (ALWAYS for unit tests) | Every line executed at least once |
| `[BC]` | **Branch Coverage** | Functions with if/else/switch branches | Every true/false branch executed |
| `[PC]` | **Path Coverage** | Complex functions with nested conditions | Every possible execution path |
| `[CC]` | **Condition Coverage** | Boolean expressions with multiple sub-conditions | Every sub-expression evaluated both ways |

**Experience-Based — Apply as supplement**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[EX]` | **Exploratory Testing** | New UI features, complex user flows | Manual: simultaneous learning + test design + execution |
| `[CL]` | **Checklist-Based** | Repetitive test patterns (CRUD, form validation) | Tests from predefined checklists |

**Test Levels — Functional**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[UT]` | **Unit Testing** | Every code change (ALWAYS) | Isolated function/method tests |
| `[IT]` | **Integration Testing** | PBI touches 2+ modules/services or DB | Module interaction tests |
| `[SYS]` | **System Testing** | PBI adds complete feature visible to user | Full-stack behavior tests |
| `[UAT]` | **Acceptance Testing** | PBI has explicit AC items | Validate system meets business requirements |
| `[REG]` | **Regression Testing** | Any existing functionality could be affected | Existing tests re-run, new regression tests added |

**Test Levels — Non-Functional**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[PERF]` | **Performance Testing** | PBI has DB queries, bulk operations, or API endpoints | Speed/throughput/resource measurements |
| `[SEC]` | **Security Testing** | PBI handles auth, user input, or sensitive data | Vulnerability scanning, injection testing |

**Test Approaches — Automation**

| Tag | Technique | Trigger Condition | What It Produces |
|-----|-----------|-------------------|-----------------|
| `[TDD]` | **TDD** | All implementation (ALWAYS per project rules) | Red/green/refactor test cycle |
| `[PBT]` | **Property-Based Testing** | Mathematical operations (calculations, amounts, dates) | Random inputs to find edge cases |
| `[CT]` | **Contract Testing** | PBI modifies API between frontend and backend | Producer/consumer API contract verification |
| `[SNAP]` | **Snapshot Testing** | PBI modifies API response shape or UI output | Detect unexpected output changes |

##### Technique Selection Rules

1. **Always apply**: `[EP]`, `[BVA]`, `[UT]`, `[TDD]`, `[SC]` — these are mandatory for every PBI
2. **Apply when triggered**: Select from remaining techniques based on trigger conditions
3. **Prioritize by PBI scope**: Simple CRUD may need 6-8 techniques; complex multi-layer PBIs may need 15-20

##### Technique Summary Output

After selecting techniques, produce a summary table in `test_design.md`:

```markdown
## Applied Test Techniques

| Category | Techniques Applied | Count |
|----------|--------------------|-------|
| Specification-Based | EP, BVA, DT | 3 |
| Structure-Based | SC, BC | 2 |
| Functional Levels | UT, IT, REG | 3 |
| Non-Functional | PERF, SEC | 2 |
| Automation | TDD, CT | 2 |
| **Total** | | **12** |
```

##### Test Case Generation

For each test case, tag with ALL applicable techniques (e.g., `[EP][UT][TDD]`, `[BVA][IT]`).

- Identify required automated test cases
    - Mark already-implemented tests with checkmark
    - Tag each with technique tags from the catalog above
- Identify required manual test cases
    - Tag with `[EX]`, `[CL]`, `[UAT]`, or other manual technique tags
- Clearly indicate: new test vs modification of existing test
- Clearly mark regression tests with `[REG]`

##### Test Priority Definitions

| Priority | Criteria | Examples |
|----------|----------|---------|
| **High** | Blocks AC delivery; data integrity risk; security boundary; core business logic | Auth bypass, incorrect calculation, data leak |
| **Medium** | Edge cases; non-critical paths; UX quality; performance targets | Empty list rendering, pagination boundary, slow query |
| **Low** | Cosmetic; nice-to-have coverage; defensive regression | UI alignment, tooltip text, rarely-hit error message |

#### Step 4.4.1: AC Traceability Table

After listing test cases, produce a traceability table mapping every AC item to its test cases:

```markdown
## AC Traceability

| AC Item | Test Case(s) | Technique | Status |
|---------|-------------|-----------|--------|
| AC-1: {description} | T1, T3 | EP, BVA | Covered |
| AC-2: {description} | T5 | DT | Covered |
| AC-3: {description} | — | — | GAP |

**Coverage: {covered}/{total} ({percentage}%)**
```

- Every AC item MUST map to at least one test case
- Items with no test case are marked **GAP** — these must be resolved before finalizing

#### Step 4.5: Output Questions to Q&A Document
- Create questions for unclear points and specification confirmations in the Q&A document
- Use question format from `.claude/skills/docs/pbi_question_format.md`
- After outputting, verify these options are included:
    - Other
    - Skip / decide later
    - Explain the background
- Display "Please let me know when you have answered" and present AskUserQuestion:
    - "Answered — continue"
    - "Need investigation output" → output to `{PBI_DIR}/test_design_investigation.md`
    - "Other"
    - Note: "You only need to answer what is relevant right now"
- Update test draft as answers are received
- Propose additions to acceptance criteria or tech design if needed (shift-left — confirm before adding)
- When all questions are resolved, output final version and show file path (don't output content to console)
    - Ask user to review for gaps or errors
    - "Let me know if anything concerns you. We can proceed and fix later if needed."

**Do NOT proceed to Step 4.6 or later steps until the user has answered questions or explicitly skipped.**

#### Step 4.6: Propose Updates to AC / Tech Design
- If the analysis reveals gaps in the acceptance criteria or tech design, propose updates
- Confirm with user before making changes

Repeat questions and answers until no unclear points remain.

### 5. Output Format (test_design.md)

All content goes into a single `test_design.md` file:

```markdown
## Impact Scope
- [affected areas, features] (minimum necessary description)

## Risk Assessment
- [risk items] (minimum necessary description)

## Applied Test Techniques

| Category | Techniques Applied | Count |
|----------|--------------------|-------|
| Specification-Based | EP, BVA, DT | 3 |
| ... | ... | ... |
| **Total** | | **{N}** |

## Test Analysis (what to test and why)

- Use as reference to add missing perspectives, not as a final checklist.
- Auto/manual classification is flexible — adjust based on implementation.

### Automated Tests

#### High Priority
- [ ] `[EP][UT][TDD]` [required test and reason]

#### Medium Priority
- [ ] `[BVA][IT]` [required test and reason]

#### Low Priority
- [ ] `[REG]` [required test and reason]

### Manual Tests

#### High Priority
- [ ] `[EX][UAT]` [required test and reason]

#### Medium Priority
- [ ] `[CL]` [required test and reason]

### Non-Functional Tests

#### Security
- [ ] `[SEC]` [test and reason]

#### Performance
- [ ] `[PERF]` [test and reason]

## AC Traceability

| AC Item | Test Case(s) | Technique | Status |
|---------|-------------|-----------|--------|
| AC-1: {description} | T1, T3 | EP, BVA | Covered |
| AC-2: {description} | — | — | GAP |

**Coverage: {covered}/{total} ({percentage}%)**

## Specification Clarifications Needed
```

### 6. Recommend Next Steps

Use AskUserQuestion to select next action:
1. Run AI review of test design (recommended) → Execute `/010-review-test-design` via Skill tool
2. Proceed to implementation → Execute `/011-implement-pbi` via Skill tool
3. Finish
