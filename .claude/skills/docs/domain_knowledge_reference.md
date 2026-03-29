# Domain Knowledge Progressive Reference Guide

Common procedures for progressively referencing domain knowledge documents under `stock/` when executing flow-type skills.

## Phase 1: At Processing Start

Read `stock/README.md` using the Read tool at the start of processing to understand the following (can be read in parallel with other "files to read before starting"):

- List of available domain knowledge categories
- Trigger words for each category
- Entry points for each category (README.md or main file)

**At this stage, do not read individual files within categories.**

## Phase 2: When Receiving User Input

When receiving user input and considering content for file output, execute the following:

### Decision Timing

Explore related domain knowledge when any of the following apply:

- User input (PBI content, answers, additional information) contains trigger words
- Features/domains mentioned by the user are related to a category
- Code investigation discovered code from a related domain

### Exploration Steps

1. Interpret the user's input content and match against trigger words in `stock/README.md`
2. If a related category is found, read the entry point:
   - If the entry point is README.md → Read README.md
   - If the entry point is an individual file → Read that file directly
3. From the entry point content, identify individual files related to the current task and read as needed

### Exploration Depth

- **Shallow exploration**: Read only the entry point (README.md or main file) → When overview understanding is sufficient
- **Deep exploration**: Follow from entry point to read individual files → When specific implementation methods or rules are needed

Example: For a budget management feature
- Creating acceptance criteria → Reading `stock/domain-model.md` for an overview is often sufficient
- Technical design → `stock/domain-model.md` + read `stock/glossary.md` as needed based on work content

## Notes

- **Do not read documents from unrelated categories** (context conservation)
- You don't need to read everything in a single exchange. Dig deeper progressively as needed
- If new domain topics emerge from new user input, match against trigger words again at that point
