# Requirements Analysis Output Format

## Components

- **Problem Statement**: Clearly describe the problem to be solved in 2-3 sentences
- **Evidence for each section**: Explain why each requirement is necessary and where it was derived from in the input
- **Detail formats**: Choose from the following based on what you want to express

| What to Express | Format | Example |
|----------------|--------|---------|
| Listing requirements | Checklist (default) | `- [ ] Can do ~` |
| Behavioral differences based on conditions | Gherkin table | Normal/Error/Edge cases |
| Organizing items with multiple attributes | Summary table | Priority × Item, Stakeholder × Concerns |
| Sequential procedures | Numbered list | 1. → 2. → 3. |

## Output Items

### Required Sections

- **Abstract** — A section that concisely summarizes the overview of issues, requests, requirements, and acceptance criteria. Volume that can be explained verbally in 1 minute (approximately 5-8 sentences). Place at the beginning of the document so readers can immediately grasp the big picture
- **Problem Statement** — What is the problem and why does it need to be solved now (2-3 sentences)
- **Goals and Value** — Business value and user value gained through the solution
- **Stakeholders** — Users, teams, and systems that are affected or involved
- **Use Cases** — Scenarios of how stakeholders specifically use this feature, in what situations and for what purposes. Enable engineers to understand "why this feature is needed" through concrete usage scenarios
- **Functional Requirements** — List of features to be implemented (checklist format recommended)
- **Non-Functional Requirements/Constraints** — Performance, security, technical constraints, operational constraints
- **Current State** — Current system/workflow (As-Is)
- **Scope** — Target scope and explicitly out-of-scope items
- **Unresolved Items/Risks** — Remaining unknowns, items requiring decisions, risks
- **Priority Assessment** — Requirement priority (MoSCoW, etc.) and rationale

### Optional Sections

- **Approach Candidates** — Solution options and comparison (if multiple exist)
- **Codebase Investigation Results** — Technical findings from investigation (if conducted)
- **Decision Records** — For recording Q&A content

## Format Example

```markdown
## Abstract

[Concisely summarize the overview of issues, requests, requirements, and acceptance criteria in 5-8 sentences. This section alone should convey "what," "why," "how to solve it," and "what are the completion criteria." Keep it to a volume explainable in 1 minute verbally.]

## Problem Statement

[Describe what the problem is and why it needs to be solved now in 2-3 sentences]

## Goals and Value

- [Business value/User value 1]
- [Business value/User value 2]

## Stakeholders

| Stakeholder | Role/Concerns |
|------------|---------------|
| [User/Team] | [Concerns] |

## Use Cases

### UC-1: [Use Case Name]
- **Actor**: [Who]
- **Trigger**: [In what situation/timing]
- **Purpose**: [What they want to achieve]
- **Basic Flow**:
  1. [Step 1]
  2. [Step 2]
  3. [Step 3]
- **Success Criteria**: [What constitutes success]
- **Current Workaround**: [How they currently deal with it (or "None")]

### UC-2: [Use Case Name]
...

## Functional Requirements

### [Feature Category 1]
- [ ] [Requirement 1]
- [ ] [Requirement 2]

### [Feature Category 2]
- [ ] [Requirement 3]

## Non-Functional Requirements/Constraints

- [ ] [Performance requirement]
- [ ] [Security requirement]
- [ ] [Technical constraint]

## Current State

[Description of current system/workflow]

## Scope

### In Scope
- [Included item 1]
- [Included item 2]

### Out of Scope
- [Explicitly excluded item 1]

## Unresolved Items/Risks

| Item | Details | Impact |
|------|---------|--------|
| [Unresolved item 1] | [Details] | High/Medium/Low |

## Priority Assessment

| Requirement | Priority | Rationale |
|------------|----------|-----------|
| [Requirement 1] | Must | [Reason] |
| [Requirement 2] | Should | [Reason] |
| [Requirement 3] | Could | [Reason] |

{Include the following only if applicable}

## Approach Candidates

| Approach | Pros | Cons | Recommendation |
|----------|------|------|----------------|
| [Option A] | [Advantages] | [Disadvantages] | ⭐︎/⚪︎/△ |

## Codebase Investigation Results

[Technical findings from investigation]

{Include the following only if outputting decision records}

## Decision Records
```
