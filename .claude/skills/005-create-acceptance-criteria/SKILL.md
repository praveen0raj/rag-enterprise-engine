---
description: Improve acceptance criteria
---

Improve acceptance criteria.

## Files to Read Before Starting

Read the following files using the Read tool before starting:
- `.claude/skills/docs/acceptance_criteria_format.md` — Acceptance criteria output format
- `.claude/skills/docs/question_format.md` — Question format
- `.claude/skills/docs/acceptance_criteria_review_perspectives.md` — Review perspectives (to create review-resistant acceptance criteria)
- `.claude/skills/docs/domain_knowledge_reference.md` — Domain knowledge progressive reference guide

**Parallel processing principle**: Time-consuming tasks such as code investigation and file reading should be processed in parallel as much as possible.

**Principle**: When creating acceptance criteria, always be mindful of the review perspectives in `.claude/skills/docs/acceptance_criteria_review_perspectives.md` to create review-resistant acceptance criteria.

## Description Approach Based on PBI Nature

The granularity of acceptance criteria descriptions varies depending on whether the PBI content is business-oriented or technical:

| PBI Nature | Example | Acceptance Criteria Description |
|-----------|---------|-------------------------------|
| Business-oriented | Candidate search feature, status change | Describe from business perspective. Technical details go to technical design |
| Technical | Repository implementation, refactoring | May include technical details |

**Decision criteria**: Judge by **the PBI's main content**, not by the actor (who)
- Business value is primary → Business-oriented PBI
- Technical implementation/improvement is primary → Technical PBI

**Decision hints**:
- "User can ~" is the main acceptance criteria → Business-oriented
- "~ is implemented", "~ tests pass" is the main acceptance criteria → Technical
- If both elements exist → Judge by main value, sub-elements go in technical design

**What NOT to include in business-oriented PBIs:**
- File paths (e.g., `infrastructure/persistence/xxx.kt`)
- Method/function names (e.g., `repo.save()`)
- Class/struct names
- Table definition details
- Scenarios like "Technical verification (code)"

These should be documented in technical design (`/007-create-tech-design`).

## Processing Steps

### 1. Request Where Acceptance Criteria to Improve Are Located

- Have the user choose whether to specify via URL, direct input, or if acceptance criteria haven't been created yet and should be built through Q&A
    - If not yet created, ask them to share what they're considering, even if fragmentary

### 1.5. Design Context Check (Auto-detection)

- Check if `design_context.md` or `.drawio` files exist under the target PBI's directory
- If found:
    - Load design context and understand visual state matrix and interaction flow
    - Display "Design context found. Will reflect visual state scenarios in acceptance criteria."
    - In Step 4, generate questions/scenarios with awareness of visual state coverage
    - Also reference design review perspectives (`.claude/skills/docs/design_review_perspectives.md`) and suggest scenario additions if there are state coverage gaps
- If not found:
    - Skip this step and proceed normally to Step 2

### 2. Confirm PBI Nature and Determine Description Approach

- Confirm whether the PBI content is business-oriented or technical
- For business-oriented PBIs:
    - Don't include technical details in acceptance criteria, describe from business perspective
    - If scenarios like "Technical verification (code)" exist, suggest moving them to technical design
- For technical PBIs:
    - Technical details may be included

### 3. Confirm Target PBI Directory

- Confirm the target EPIC and PBI (e.g., `epics/e001_budget-management/pbis/e001-001_budget-list/`)
- Create the directory if it doesn't exist

#### Acceptance Criteria Document

- `epics/{EPIC_NAME}/pbis/{PBI_NAME}/acceptance_criteria.md`

#### Q&A Document

- Filename: `epics/{EPIC_NAME}/pbis/{PBI_NAME}/acceptance_criteria_q_and_a.md`
- See `.claude/skills/docs/question_format.md` for question format

#### Investigation Results Document (if needed)

- Filename: `epics/{EPIC_NAME}/pbis/{PBI_NAME}/acceptance_criteria_investigation.md`

### 4. Ask Questions About Unclear Points in Acceptance Criteria

- Consider questions about unclear points in acceptance criteria as multiple-choice, and **output them so they can be answered in the "Q&A Document."**
  Only show the filename in the console, not the question text.
- If needed, investigate the codebase.
- See `.claude/skills/docs/question_format.md` for question format
- After output, confirm that questions include these options:
    - Other
    - Skip / Consider later
    - Please explain the background of this question
- Display "Q&A file created: {full file path}. Opening file." then run `open {Q&A file path}` using the Bash tool
- Display "Please let me know when you've answered" and use AskUserQuestion with options: "Done", "I'd like investigation results output", "Other"
    - Note: Let them know it's fine to skip questions they can't answer right now.
    - For "I'd like investigation results output": Output results to `acceptance_criteria_investigation.md`
- As answers are received, reflect them in the "Acceptance Criteria Document"
- If there are further questions after answers, output them at the end of the Q&A document and request input again. Repeat until no questions remain.
- When all questions are done, display "Opening acceptance criteria file: {full file path}" then run `open {acceptance criteria document path}` using the Bash tool. (No need to output content to console)
- See `.claude/skills/docs/acceptance_criteria_format.md` for acceptance criteria output format

### 5. Select Next Action

- After presenting the "Acceptance Criteria Document" path, use AskUserQuestion with these options:
    1. Run AI review (recommended) → Execute `/006-review-acceptance-criteria` via Skill tool
    2. Select output destination → Proceed to Step 6

### 6. Ask About Output Destination

- Have the user choose from:
    1. Update the acceptance criteria section in the originally referenced URL document
    2. Append to a newly specified URL
    3. Do nothing
- For options 1 or 2, additionally ask via AskUserQuestion: "Would you like to keep Q&A content as decision records? They can be used for reviews and retrospectives."
    - "No" (first option): Do nothing
    - "Yes": Add a section at the end of the acceptance criteria document as "Decision Records"
        - Transcribe all options and selected options, plus free-text answers without modification
        - Don't insert divider lines (---) between questions
- Output the same content as the locally created final version
- For Notion, suggest creating it as a sub-page under the specified URL

### 7. Recommend Next Steps

- Use AskUserQuestion to select next action:
    1. Run AI review → Execute `/006-review-acceptance-criteria` via Skill tool
    3. Proceed to technical design → Execute `/007-create-tech-design` via Skill tool
    4. Finish
- This command terminates
