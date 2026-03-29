---
description: Create and improve requirements analysis from raw inputs
---

Create and improve structured requirements analysis from raw inputs (ideas, Notion tickets, conversations, etc.).

## Files to Read Before Starting

Read the following files using the Read tool before starting:
- `.claude/skills/docs/requirements_analysis_format.md` — Requirements analysis output format
- `.claude/skills/docs/question_format.md` — Question format
- `.claude/skills/docs/requirements_analysis_review_perspectives.md` — Review perspectives (to create review-resistant requirements analysis)
- `.claude/skills/docs/domain_knowledge_reference.md` — Domain knowledge progressive reference guide

**Parallel processing principle**: Time-consuming tasks such as code investigation and file reading should be processed in parallel as much as possible.

**Principle**: When creating requirements analysis, always be mindful of the review perspectives in `.claude/skills/docs/requirements_analysis_review_perspectives.md` to create review-resistant requirements analysis.

## Processing Steps

### 1. Request Input Specification

- Use AskUserQuestion to have the user choose from:
    1. **Specify a URL** (Notion ticket, existing document, etc.)
    2. **Direct input** (ideas, conversation summaries, etc.)
    3. **Improve existing requirements analysis** (specify a local file)
- For URLs: Fetch and read the content
- For direct input: Ask the user to share what they're considering, even if fragmentary
- For existing files: Confirm the file path and proceed to Step 3

### 2. Confirm Target EPIC Directory

- Confirm the target EPIC (e.g., `epics/e002_custom-format-upload/`)
- Create the directory if it doesn't exist

#### Requirements Analysis Document

- `epics/{EPIC_NAME}/requirements_analysis.md`
- See `.claude/skills/docs/requirements_analysis_format.md` for output format

#### Q&A Document

- Filename: `epics/{EPIC_NAME}/requirements_analysis_q_and_a.md`
- See `.claude/skills/docs/question_format.md` for question format

#### Investigation Results Document (if needed)

- Filename: `epics/{EPIC_NAME}/requirements_analysis_investigation.md`

### 3. Analyze Input and Create Requirements Analysis Draft

- Extract and structure the following from input:
    - Abstract (overview of issues, requests, requirements, acceptance criteria. Volume explainable in 1 minute verbally. Place at document beginning)
    - Problem Statement (what the problem is and why it needs solving now)
    - Goals and Value
    - Stakeholders
    - Use Cases (specific usage scenarios per stakeholder. See `requirements_analysis_format.md` for format)
    - Functional Requirements
    - Non-Functional Requirements/Constraints
    - Current State (As-Is)
    - Scope (in-scope and out-of-scope)
    - Unresolved Items/Risks
    - Priority Assessment
- Don't force-fill information not contained in the input — treat it as "unresolved items" or questions
- If needed, investigate the codebase to understand current state and technical constraints
- Output the draft to the "Requirements Analysis Document"

### 4. Ask Questions About Unclear Points in Requirements Analysis

- Consider questions about unclear points in the requirements analysis as multiple-choice, and **output them so they can be answered in the "Q&A Document."**
  Only show the filename in the console, not the question text.
- If needed, investigate the codebase.
- See `.claude/skills/docs/question_format.md` for question format

#### Use Case Deep-Dive Questions (Required)

When generating questions, **always include** questions that deep-dive into use cases from these perspectives:

- **Usage scenario specifics**: When, where, and in what situation does each stakeholder use this feature? Even if readable from the input, confirm to increase resolution
- **Current workarounds**: How are they dealing with this without the feature? (manual work, different tool, not doing it, etc.)
- **Frequency/Impact**: How often does this occur? What happens if it can't be addressed?
- **Hypothetical use case presentation**: If use cases aren't sufficiently clear from input, form hypotheses and confirm with "Are you envisioning this kind of usage?" format

Reflect answers to these questions in the "Use Cases" section of the requirements analysis.
- After output, confirm that questions include these options:
    - Other
    - Skip / Consider later
    - Please explain the background of this question
- Display "Q&A file created: {full file path}. Opening file." then run `open {Q&A file path}` using the Bash tool
- Display "Please let me know when you've answered" and use AskUserQuestion with options: "Done", "I'd like investigation results output", "Other"
    - Note: Let them know it's fine to skip questions they can't answer right now.
    - For "I'd like investigation results output": Output results to `requirements_analysis_investigation.md`
- As answers are received, reflect them in the "Requirements Analysis Document"
- If there are further questions after answers, output them at the end of the Q&A document and request input again. Repeat until no questions remain.
- When all questions are done, display "Opening requirements analysis file: {full file path}" then run `open {requirements analysis document path}` using the Bash tool. (No need to output content to console)

### 5. Select Next Action

- After presenting the "Requirements Analysis Document" path, use AskUserQuestion with these options:
    1. Run AI review (recommended) → Execute `/003-review-requirements-analysis` via Skill tool
    2. Select output destination → Proceed to Step 6

### 6. Ask About Output Destination

- Have the user choose from:
    1. Update the document at the originally referenced URL
    2. Output to a newly specified URL
    3. Do nothing (local only)
- For options 1 or 2, additionally ask via AskUserQuestion: "Would you like to keep Q&A content as decision records? They can be used for reviews and retrospectives."
    - "No" (first option): Do nothing
    - "Yes": Add a section at the end of the requirements analysis document as "Decision Records"
        - Transcribe all options and selected options, plus free-text answers without modification
        - Don't insert divider lines (---) between questions
- Output the same content as the locally created final version
- For Notion, suggest creating it as a sub-page under the specified URL

### 7. Recommend Next Steps

- Use AskUserQuestion to select next action:
    1. Run AI review → Execute `/003-review-requirements-analysis` via Skill tool
    3. Proceed to acceptance criteria → Execute `/005-create-acceptance-criteria` via Skill tool
    4. Proceed to technical design → Execute `/007-create-tech-design` via Skill tool
    5. Finish
- This command terminates
