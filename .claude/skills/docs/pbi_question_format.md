# Question Format

## Basic Format

```
## Question N: [Title]
[Question text]

1. [Option 1] (Recommendation: ⭐︎ | [Reason])
2. [Option 2] (Recommendation: ⚪︎ | [Reason])
3. [Option 3] (Recommendation: △ | [Reason])
 :
x. Other
x. Skip / decide later
x. Explain the background of this question

Answer:

Reason (optional — use this to record decision rationale or to give AI additional context):

---
```

- Recommendation has 3 levels: ⭐︎ (Recommended) / ⚪︎ (Runner-up) / △ (Not recommended)
- Reason field display:
    - First question: Include full note "(optional — use this to record decision rationale or to give AI additional context)"
    - Subsequent questions: Just "Reason (optional):"
- If "Other" is selected, ask for details
- If "Skip / decide later" is selected, add to "Items to decide later" section
- When transcribing to decision record: if a reason is provided, add "Reason: [content]" below the answer

## Background (only when necessary)

Add background in these cases:
- Questions arising from technical investigation (e.g., discovered during code review)
- Options whose impact is not intuitively clear
- Questions about edge cases or boundary conditions
- Options with trade-offs

Background is NOT needed when:
- The question is clearly understandable as a business requirement
- The meaning and intent of options are self-evident

Since background may not always be needed, place it after the answer field.

```
## Question N: [Title]

[Question text]

1. ...

Answer:

(Background: [Brief explanation of why this question is needed])

```

## Priority Classification (for review findings)

When outputting questions as review findings, classify by priority:
- **High priority**: Core to the acceptance criteria
- **Medium priority**: Clarification is desirable, but can be decided during implementation

## Common Rules

- Place an answer field immediately after each question
- After outputting, display "Please let me know when you've answered"
- Present AskUserQuestion with options: "Done", "Other"

## Additional Review Perspectives (only when specified)

If the user specifies additional perspectives, save them in the Q&A file under an "Additional Review Perspectives" section:

```markdown
## Additional Review Perspectives

- [User-specified perspective]
```
