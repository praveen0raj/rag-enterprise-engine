---
description: AI review for technical design. Checks alignment with acceptance criteria, existing code patterns, feasibility, and completeness.
---

# PBI Technical Design Review

AI review for technical design. Checks alignment with acceptance criteria, existing code patterns, feasibility, and completeness.

## Pre-requisites

Read these files before starting:
- `.claude/skills/docs/tech_design_review_perspectives.md` — review perspectives
- `.claude/skills/docs/pbi_question_format.md` — question format

**Parallel processing**: Code investigation, file reading, and existing pattern checks should be parallelized.

## Path Convention

All paths use the **project root** as base:
- Tech design: `epics/{EPIC}/pbis/{PBI}/tech_design.md`
- AC: `epics/{EPIC}/pbis/{PBI}/acceptance_criteria.md`
- Q&A: `epics/{EPIC}/pbis/{PBI}/tech_design_q_and_a.md`
- Investigation: `epics/{EPIC}/pbis/{PBI}/tech_design_investigation.md`

## Processing Steps

### 1. Identify Review Target

- Confirm the target EPIC and PBI
- Check for these files:
    - Tech design: `{PBI_DIR}/tech_design.md`
    - AC: `{PBI_DIR}/acceptance_criteria.md`

**If tech design exists**: Use it as review target. Display "Reviewing {file path}". Also load AC if available.

**If not found**: Ask user to provide the path or URL.

### 2. Review Check

- Display: "Starting review. If you have specific perspectives you'd like reviewed, send them during the review."
- **Read AC and verify the tech design covers all acceptance criteria**
- **Investigate existing code to verify design aligns with existing patterns**
- If user specified additional review perspectives, include them and save to Q&A file under "Additional Review Perspectives"
- Ask about any unclear points
- Investigate codebase as needed

### 3. Process Review Findings

Classify each finding by severity:

| Severity | Criteria | Action |
|----------|----------|--------|
| **CRITICAL** | AC misalignment, security gap | Must fix — blocks review completion |
| **HIGH** | Pattern inconsistency, missing error handling, performance concern | Should fix — output as review finding question |
| **MEDIUM** | Naming inconsistency, incomplete edge case handling, testability concern | Recommend fix — add to Q&A |
| **LOW** | Style/formatting, minor documentation gap | Auto-fix or note |

- Add questions to `{PBI_DIR}/tech_design_q_and_a.md`
- Header: "# Review Findings (Additional Questions)"
- Use sequential numbering (continue from existing questions)
- Include mandatory options: Other, Skip/decide later, Explain background
- Open the Q&A file: `open {Q&A file path}`
- Display "Please let me know when you've answered" with AskUserQuestion:
    - "Answered — continue"
    - "Need investigation output" → write to `{PBI_DIR}/tech_design_investigation.md`
    - "Other"

### 4. Iterative Review

When answers are received:
1. Reflect answers in `{PBI_DIR}/tech_design.md`
2. Decision record handling:
    - If "Decision Records" section exists → append Q&A without asking
    - If user previously chose "no" → skip
    - Otherwise → ask "Save Q&A as decision record?"
3. Re-run review from Step 2
4. Repeat until no additional questions remain

### 5. Review Complete

Display:
- "AI review complete."
- "Repeating the review 2-3 times improves accuracy. Select 're-review' to run again."
- "AI can make mistakes. Domain knowledge and context-dependent aspects require human verification."
- Open the tech design file

Present AskUserQuestion (always show all options):
1. Re-review (recommended for 1st/2nd review)
2. Add perspectives and re-review
3. Review complete
4. Other

If "Re-review" → go to Step 2.
If "Add perspectives" → ask for perspectives, then go to Step 2.

### 6. Recommend Next Steps

Use AskUserQuestion to select next action:
1. Proceed to test design (recommended) → Execute `/009-create-test-design` via Skill tool
2. Finish
