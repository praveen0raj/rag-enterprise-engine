---
description: AI review for test design. Checks AC coverage, alignment with tech design, boundary values, error cases, and regression coverage.
---

# PBI Test Design Review

AI review for test design. Checks AC coverage, alignment with tech design, boundary values, error cases, and regression coverage.

## Pre-requisites

Read these files before starting:
- `.claude/skills/docs/test_design_review_perspectives.md` — review perspectives
- `.claude/skills/docs/pbi_question_format.md` — question format

**Parallel processing**: Code investigation, file reading, and test coverage checks should be parallelized.

## Path Convention

All paths use the **project root** as base:
- Test design: `epics/{EPIC}/pbis/{PBI}/test_design.md`
- Tech design: `epics/{EPIC}/pbis/{PBI}/tech_design.md`
- AC: `epics/{EPIC}/pbis/{PBI}/acceptance_criteria.md`
- Q&A: `epics/{EPIC}/pbis/{PBI}/test_design_q_and_a.md`
- Investigation: `epics/{EPIC}/pbis/{PBI}/test_design_investigation.md`

## Processing Steps

### 1. Identify Review Target

- Confirm the target EPIC and PBI
- Check for these files:
    - Test design: `{PBI_DIR}/test_design.md`
    - Tech design: `{PBI_DIR}/tech_design.md` (if available)
    - AC: `{PBI_DIR}/acceptance_criteria.md`

**If test design exists**: Use it as review target. Display "Reviewing {file path}". Also load AC and tech design if available.

**If not found**: Ask user to provide the path or URL.

### 2. Review Check

- Display: "Starting review. If you have specific perspectives you'd like reviewed, send them during the review."
- **Read AC and verify all items are covered by tests**
- **Read tech design and verify all defined behaviors are being tested**
- **Check for boundary value and error case coverage**
- **Verify regression tests exist for existing functionality**
- If user specified additional review perspectives, include them and save to Q&A file under "Additional Review Perspectives"
- Investigate codebase as needed

### 3. Process Review Findings

| Finding | Action |
|---------|--------|
| Document issues (typos, formatting) | Auto-fix, notify user |
| AC coverage gap | Output as review finding question |
| Missing boundary/error cases | Output as review finding question |
| Test design concerns | Add to Q&A file as additional questions |

- Add questions to `{PBI_DIR}/test_design_q_and_a.md`
- Header: "# Review Findings (Additional Questions)"
- Use sequential numbering (continue from existing questions)
- Include mandatory options: Other, Skip/decide later, Explain background
- Open the Q&A file: `open {Q&A file path}`
- Display "Please let me know when you've answered" with AskUserQuestion:
    - "Answered — continue"
    - "Need investigation output" → write to `{PBI_DIR}/test_design_investigation.md`
    - "Other"

### 4. Iterative Review

When answers are received:
1. Reflect answers in `{PBI_DIR}/test_design.md`
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
- Open the test design file

Present AskUserQuestion (always show all options):
1. Re-review (recommended for 1st/2nd review)
2. Add perspectives and re-review
3. Review complete
4. Other

If "Re-review" → go to Step 2.
If "Add perspectives" → ask for perspectives, then go to Step 2.

### 6. Recommend Next Steps

Use AskUserQuestion to select next action:
1. Proceed to implementation (recommended) → Execute `/011-implement-pbi` via Skill tool
2. Finish
