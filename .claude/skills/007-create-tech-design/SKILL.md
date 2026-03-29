---
description: Create technical design from acceptance criteria for a PBI. Two-stage design process (policy + detailed) with auto-selected diagrams and structured Q&A.
---

# PBI Technical Design

Create technical design from acceptance criteria. Two stages: policy design (for alignment with implementer/reviewer) and detailed design (for implementation-level decisions).

## Pre-requisites

Read these files before starting:
- `.claude/skills/docs/pbi_question_format.md` — question format
- `.claude/skills/docs/tech_design_review_perspectives.md` — tech design review perspectives

**Parallel processing**: Code investigation, file reading, and other time-consuming tasks should be parallelized as much as possible.

**Code investigation optimization**: When investigating code in the parent repo, first use Explore agents (Haiku) to understand file structure and patterns, then Read specific files. Do not load all files into main context.

## Path Convention

All paths in this skill use the **project root** as base:
- PBI directory: `epics/{EPIC}/pbis/{PBI}/`
- AC: `epics/{EPIC}/pbis/{PBI}/acceptance_criteria.md`
- Tech design output: `epics/{EPIC}/pbis/{PBI}/tech_design.md`
- Q&A: `epics/{EPIC}/pbis/{PBI}/tech_design_q_and_a.md`

## Processing Steps

### 1. Confirm PBI Directory

- Confirm the target EPIC and PBI (e.g., `epics/e001_budget-management/pbis/e001-001_budget-list/`)
- Verify the directory exists

### 2. Confirm Acceptance Criteria

- Check if `{PBI_DIR}/acceptance_criteria.md` exists
- If yes, confirm with user: "Use this acceptance criteria?"
- If no, ask user to provide URL or path to the acceptance criteria

### 3. Check Existing Tech Notes

- Check if `{PBI_DIR}/tech_design.md` already exists with content
- If it exists and has content beyond "TODO":
    - Display: "Existing tech notes found. These will be considered during tech design."
    - Remove any migration notices
    - Refine and expand the existing content

### 4. Create Policy Design

Create these files **simultaneously**:

- **Tech design file**: `{PBI_DIR}/tech_design.md`
    - Implementation approach
    - Design diagrams (auto-selected, see below)
    - Patterns used
    - Items to decide later
    - Remaining tasks
    - This file is updated throughout the Q&A process

- **Q&A document**: `{PBI_DIR}/tech_design_q_and_a.md`

#### Policy Design Process

- Read the acceptance criteria and codebase to create the implementation approach.
    - Granularity: enough for implementer and reviewer to align on the approach. Prioritize key discussion points.
    - Start with high-level approach. For details, ask whether to discuss now or defer.
    - Test approach is not needed
- Granularity:
    - **Output should be the minimum necessary to align on the approach**

#### Design Diagrams (Auto-Selected)

After writing the implementation approach, the agent MUST assess PBI complexity and auto-generate applicable diagrams as Mermaid blocks in `tech_design.md`. Do NOT ask the user which diagrams to include — decide based on the trigger rules below.

##### LOW LEVEL DIAGRAMS — PBI-Relevant Catalog

**Structural (UML)**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| UML | PBI introduces domain entities with inheritance or complex relationships | **Class Diagram** | `classDiagram` |
| UML | PBI spans 3+ components or packages | **Component Diagram** | `flowchart TD` with subgraphs |

**Behavioral (UML)**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Interaction | PBI touches 2+ layers (frontend → API → DB, or API → async service) | **Sequence Diagram** | `sequenceDiagram` |
| State | PBI introduces or changes entity lifecycle / status transitions | **State Machine Diagram** | `stateDiagram-v2` |
| Flow | PBI has branching business logic (if/else, switch by type, conditional rules) | **Activity Diagram / Flowchart** | `flowchart TD` |

**Data & Database**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Modeling | PBI adds or modifies DB tables, columns, or relationships | **ERD (partial)** | `erDiagram` |
| Modeling | PBI adds 5+ DB columns with constraints, defaults, or nullability rules | **Data Dictionary** | Markdown table |
| Flow | PBI has data transforms across 3+ layers (DB → service → API → frontend) | **Data Flow Diagram (L1/L2)** | `flowchart LR` |

**Domain-Driven Design**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Tactical | PBI creates aggregate with 2+ child entities/value objects | **Aggregate Diagram** | `classDiagram` |

**Async / Event-Driven**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Messaging | PBI involves async processing (background jobs, event queues) | **Message Flow Diagram** | `flowchart LR` |

**Security & Auth**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Auth | PBI adds/modifies auth flow or permission logic | **Auth Flow Diagram** | `sequenceDiagram` |
| Auth | PBI modifies role-based access (admin, viewer, editor permissions) | **RBAC Permission Matrix** | Markdown table |

**API & Integration**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Documentation | PBI adds 3+ new API endpoints | **API Endpoint Map** | Markdown table |
| Documentation | PBI adds complex API endpoints with nested request/response bodies | **OpenAPI / Swagger Schema** | YAML code block |

**Process & Workflow**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Logic | PBI has complex conditional rules | **Decision Tree** | `flowchart TD` |

**Migration**

| Subcategory | Trigger condition | Diagram | Mermaid format |
|---|---|---|---|
| Database | PBI adds DB migration with rollback concern | **DB Migration Flow** | `flowchart TD` |

##### Selection Rules

- Include only diagrams whose trigger condition is met. Simple CRUD PBIs may need 0-1 diagrams.
- **Maximum 3 diagrams per PBI.** If more than 3 triggers match, pick the 3 most valuable for the implementer by priority: Sequence > State Machine > ERD > others.
- Each diagram must have a brief title comment and be placed under a `## Design Diagrams` section.
- Keep diagrams focused on the PBI scope — do not diagram the entire system.

#### Patterns Used (Auto-Selected)

After the diagrams, add a `## Patterns` section listing 1-3 patterns the implementation will use. Select from:

| Pattern | Trigger |
|---|---|
| **Repository** | PBI has data access |
| **Aggregate Root** | PBI creates/modifies entity with child entities |
| **Value Object** | PBI introduces a typed domain concept |
| **Strategy** | Behavior varies by type |
| **State Machine** | Entity has lifecycle transitions |
| **Middleware Chain** | PBI adds cross-cutting concern (auth, tenant filter, logging) |
| **Observer / Domain Event** | PBI triggers side effects on state change |

Format: `- **Pattern Name** — one-line reason why it applies to this PBI`

Do NOT list patterns that don't apply. Zero patterns is valid for trivial PBIs.

#### Structured Q&A (Taxonomy-Driven)

Before generating ad-hoc questions, scan AC against these categories and mark each as **Clear / Partial / Missing**:

1. **Functional scope** — boundaries, out-of-scope declarations, feature interactions
2. **Domain/data model** — entities, relationships, lifecycle/state transitions, uniqueness rules
3. **Interaction flow** — user journeys, error/empty/loading states, navigation
4. **Non-functional** — performance targets, scalability limits, reliability/availability
5. **Edge cases** — boundary conditions, concurrent access, data volume limits
6. **Integration** — external services, failure modes, retry/timeout behavior
7. **Terminology** — consistent naming between AC, tech design, and existing codebase

Generate questions from **Partial/Missing** categories first. Prioritize by **(Impact x Uncertainty)** — max 5 questions per round.

For each multiple-choice question, include a **Recommended** option with 1-line reasoning at the top.

- Create questions in multiple-choice format in the Q&A document.
    - Use question format from `.claude/skills/docs/pbi_question_format.md`
    - If user says "skip/decide later", add to "Items to decide later" section
- After outputting questions, verify these options are included:
    - Other
    - Skip / decide later
    - Explain the background
- Display "Please let me know when you have answered" and present AskUserQuestion:
    - "Answered — continue"
    - "Other"
    - Note: "You only need to answer what is relevant right now"
- Update working files as answers are received
- Propose additions to acceptance criteria if needed (shift-left — confirm before adding)
- When all questions are resolved, output final version and show file path (don't output content to console)
    - Ask user to review for gaps or errors
    - "Let me know if anything concerns you. We can proceed and fix later if needed."
- AskUserQuestion: "Save Q&A as decision record?"
    - No (first option): do nothing
    - Yes: append "Decision Records" section to tech_design.md with all Q&A choices and answers verbatim

### 5. Detailed Design (Optional)

- Ask if detailed design is needed. If not needed, end this skill.
- Append detailed design to `{PBI_DIR}/tech_design.md`
- Same Q&A process as policy design (including taxonomy-driven question generation)
- Granularity: enough for implementer to confirm detailed approach with AI

### 6. Recommend Next Steps

Use AskUserQuestion to select next action:
1. Run AI review of tech design (recommended) → Execute `/008-review-tech-design` via Skill tool
2. Proceed to test design → Execute `/009-create-test-design` via Skill tool
3. Finish
