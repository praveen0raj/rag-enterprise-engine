---
description: New EPIC kickoff — end-to-end from raw input to EPIC structure (requirements analysis + PBI decomposition + directory creation)
---

Launch a new EPIC. Performs end-to-end processing from receiving input through requirements analysis, PBI decomposition, and directory structure creation.

## Files to Read Before Starting

Read the following files using the Read tool before starting:
- `.claude/skills/docs/requirements_analysis_format.md` — Requirements analysis output format
- `.claude/skills/docs/question_format.md` — Question format
- `.claude/skills/docs/domain_knowledge_reference.md` — Domain knowledge progressive reference guide
- `CLAUDE.md` — Document rules and directory structure

**Parallel processing principle**: Time-consuming tasks such as file reading and code investigation should be processed in parallel as much as possible.

## Processing Steps

### 1. Select Input Format

Use AskUserQuestion to have the user choose from:
1. **Specify a URL** (Notion ticket, PRD, existing document, etc.)
2. **Direct input** (ideas, conversation summaries, etc.)
3. **Specify a Figma URL** (feature extraction from designs)

- For URLs: Fetch and read the content (Notion: MCP ToolSearch → notion-fetch, Web: WebFetch)
- For Figma: Load Figma tools via ToolSearch, then use get_design_context to fetch content
- For direct input: Ask the user to share what they're considering, even if fragmentary

### 2. Determine EPIC Number and Name

1. Scan under `epics/` to check existing EPIC numbers (Glob tool)
2. Automatically determine the next sequential number (e.g., if e001, e002 exist → e003)
3. Confirm EPIC name (English, kebab-case) via AskUserQuestion
   - Example: `e003_custom-format-upload`
4. Naming convention: `e{NNN}_{english-kebab-name}`

### 3. Create EPIC Directory Skeleton

Create the following directory structure:
```
epics/e{NNN}_{name}/
├── overview.md              ← Empty template
└── pbis/
```

### 4. Generate Requirements Analysis

Launch `pdm-requirements-analyst` as a subagent (Agent tool):
- `subagent_type: "general-purpose"`
- Include input content, EPIC name, and output path in the prompt
- Instruct it to follow `.claude/agents/pdm-requirements-analyst.md`

Output location:
- `epics/{EPIC}/requirements_analysis.md`

### 5. Q&A Cycle with User

1. Output unclear points from requirements analysis as a Q&A file (following `question_format.md`)
2. Display the file path and run `open {Q&A file path}` using the Bash tool
3. Display "Please let me know when you've answered"
4. Use AskUserQuestion with options: "Done", "I'd like investigation results output", "Other"
5. Update requirements analysis based on answers
6. Repeat until no questions remain

### 6. PBI Decomposition and Estimation

Launch `pdm-backlog-strategist` as a subagent (Agent tool):
- Pass the requirements analysis file path
- Request PBI candidate decomposition and relative estimation (Fibonacci scale)
- Naming convention: `e{NNN}-{NNN}_{english-name}/`

### 7. PBI List Approval

1. Present PBI candidates to the user in table format (name, summary, estimate, priority)
2. Request approval via AskUserQuestion:
   - "Approve"
   - "I want to add/modify PBIs"
   - "I want to reconsider the PBI splits"
3. Apply any modifications before proceeding

### 8. Create PBI Directory Skeletons

Create a directory for each approved PBI:
```
pbis/e{NNN}-{NNN}_{name}/
```

Record PBI list, estimates, and priorities in overview.md.

### 9. Consistency Check

Launch `pdm-consistency-checker` as a subagent (Agent tool):
- Check consistency between the newly created EPIC and existing `stock/`
- Report any findings to the user

### 10. Select Next Action

Use AskUserQuestion to have the user choose from:
1. **Run AI review of requirements analysis** (recommended) → Execute `/003-review-requirements-analysis` via Skill tool
2. **Proceed to acceptance criteria creation** → Execute `/005-create-acceptance-criteria` via Skill tool
3. **Sync to Notion** → Load Notion tools via MCP ToolSearch and sync (follow "Notion Sync Content Guidelines" below)
4. **Finish**

#### Notion Sync Content Guidelines

**EPIC Ticket:**
- Include overview.md content (summary, issue list, requirements list, PBI list and dependencies) in the body

**PBI Ticket:**

The PBI ticket body should be structured so that engineers can understand "why we're building this feature" as a narrative. Acceptance criteria (What) alone can enable spec-compliant implementation, but don't convey "whose problem are we solving." Only when engineers understand the user's current workflow and pain points can they correctly interpret the intent behind the spec and make better implementation decisions.

Body structure:

1. **Why: Current Pain & What This PBI Solves** (placed at the top of the body)
   - Extract related issue IDs (P{N}) and requirement IDs (R{N}) from `requirements_analysis.md` for this PBI
   - Rather than simply listing IDs and summaries, describe narratively with the following structure:
     - **Current Situation**: Explain the user's current workflow and the specific problems arising from it
     - **What This PBI Delivers**: Explain how this PBI solves the problems
   - Example: "Currently, users perform XX tasks using YY, but manual work is required every time ZZ occurs, resulting in approximately N hours of loss per month (P3). This PBI provides the XX feature, eliminating this manual work (R5)." — structure it so the story of background → problem → solution is clear in a single read
   - If multiple related issues/requirements exist, explain the connections between them
   - **Must include Mermaid diagrams**: Include 1-2 diagrams that visually convey the core of the PBI, such as Before/After flow comparisons, process flows, or state transitions. Notion natively renders mermaid code blocks (```mermaid). Diagram type examples:
     - Before/After flow diagrams (current manual process vs. new flow): Use `flowchart LR` with red→green color coding
     - Process branching/decision flows: Use `flowchart TD` for decision trees
     - State transitions: Use `stateDiagram-v2` for state changes
     - Comparison diagrams: Use `subgraph` for parallel placement
   - List related issue IDs and requirement IDs as a summary at the end

2. **Acceptance Criteria** (if `acceptance_criteria.md` exists)
   - Based on the issues/requirements above, describe "what specifically needs to be satisfied"

3. **Dependencies** (if dependencies between PBIs exist)
   - Specify which PBIs are prerequisites and why that ordering is necessary