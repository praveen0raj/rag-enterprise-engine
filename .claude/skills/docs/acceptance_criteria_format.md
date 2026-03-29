# Acceptance Criteria Output Format

## Components

- **Overview**: State the purpose in [As a]/[Value/Purpose]/[I want to] format (optional)
- **Why for each scenario**: Explain in one line why this requirement is necessary
- **Detail formats**: Choose from the following based on what you want to express

| What to Express | Format | Example |
|----------------|--------|---------|
| Items to verify | Checklist (default) | `- [ ] Can do ~` |
| Behavioral differences based on conditions | Gherkin table | Normal/Error/Edge cases |
| Organizing items with multiple attributes | Summary table | Priority × Item, Permission × Feature |
| Sequential procedures | Numbered list | 1. → 2. → 3. |

## Output Items

- Related issues and requirements (list relevant issue IDs and requirement IDs from EPIC's requirements_analysis.md)
  - Rather than bullet-point lists, describe so that "user's current workflow → problem → solution in this PBI" reads as a narrative
  - Include 1-2 Mermaid diagrams (Before/After flow, process branching, state transitions, etc.) for visual communication
- Overview ([As a]/[Value/Purpose]/[I want to]) — optional
- Acceptance criteria (Scenario + Why + appropriate format)
- Implementation approach
    - Details will be explored in technical design, so only include overview-level at this stage
- Items to decide later

## Format Example

```markdown
## Related Issues and Requirements

### Current Problems

{Describe the user's current workflow and the problems arising from it as a narrative.}
{Example: "Currently, users perform XX tasks using YY, but manual work is required every time ZZ occurs (P3)."}

(Mermaid diagram: Before/After flow comparison, process branching/decision flow, etc.)

### What This PBI Solves

{Describe how this PBI solves the problems above.}

**Related issues:** P{N} (summary), P{M} (summary)
**Related requirements:** R{N} (summary, priority), R{M} (summary, priority)

## Overview

- **[As a]:**
- **[Value/Purpose]:**
- **[I want to]:**

## Acceptance Criteria

### Scenario X: [Scenario Name]
**Why**: [One line explaining why this requirement is necessary]

- [ ] [Verification item 1]
- [ ] [Verification item 2]

### Scenario Y: [Scenario with conditional branching]
**Why**: [One line explaining why this requirement is necessary]

| Given | When | Then |
|-------|------|------|
| [Precondition 1] | [Action] | [Expected result 1] |
| [Precondition 2] | [Action] | [Expected result 2] |

## Out of Scope

## Items to Decide Later

{Include the following only when outputting decision records}
---

## Decision Records
```
