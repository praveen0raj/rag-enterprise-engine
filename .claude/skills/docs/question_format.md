# Question Format

## Basic Format

```
## Question N: [Title]
[Question text]

1. [Option 1] (Recommendation: ⭐︎| [Reason])
2. [Option 2] (Recommendation: ⚪︎| [Reason])
3. [Option 3] (Recommendation: △| [Reason])
 :
x(sequential). Other
x(sequential). Skip / Consider later
x(sequential). Please explain the background of this question

Answer:

Reason (optional - fill in if you want to record the decision rationale or want AI to consider your intent):

---
```

- Recommendation has 3 levels: ⭐︎ (Recommended) / ⚪︎ (Runner-up) / △ (Not recommended)
- Reason field display:
    - First question: "(optional - fill in if you want to record the decision rationale or want AI to consider your intent)" — only include this note on the first question
    - Second question onwards: Only write "Reason (optional):"
- For "Other": Ask for details
- For "Skip / Consider later": Add to "Items to decide later" section
- When transcribing to decision records: If a reason is provided, append "Reason: [content]" below the answer

## Background (only when necessary)

Add background in these cases:
- Questions arising from technical investigation results (discovered through code investigation, etc.)
- When the impact of options is not intuitively clear
- Questions about edge cases or boundary conditions
- Options with trade-offs

Background is not needed when:
- The question is clearly understandable as a business requirement
- The meaning and intent of the options are self-evident

Since background may not always be needed, place it after the answer field.

```
## Question N: [Title]

[Question text]

1. ...

Answer:

(Background: [Briefly explain why this question is necessary])

```

## Priority Classification (for review findings)

When presenting questions as review findings, classify by priority:
- **High priority**: Relates to the core of acceptance criteria
- **Medium priority**: Clarification is desirable but can be decided during implementation

## Common Rules

- Provide an answer field immediately after each question
- After output, display "Please let me know when you've answered"
- Use AskUserQuestion with options: "Done", "Other"

## Additional Review Perspectives (only when specified)

When the user specifies additional perspectives, save them in the "Additional Review Perspectives" section of `acceptance_criteria_q_and_a.md`:

```markdown
## Additional Review Perspectives

- [User-entered perspective]
```
