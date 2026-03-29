---
description: Generate high-level architecture and design diagrams for an EPIC by reading overview.md and requirements_analysis.md. Outputs a single diagrams.md file to {en,ja}/.
---

## 1. {Diagram Title}

**Category**: {Category} > {Subcategory}

{1-2 sentences explaining what this diagram shows and why it's relevant to this EPIC}

```mermaid
{mermaid diagram content}
```

**Key Decisions & Notes**
- {Bullet points explaining design choices visible in the diagram}
- {Any assumptions made}

---

## 2. {Next Diagram Title}

...
```

### 5. End

Present:
- Path to the generated file
- List of diagrams included with their trigger reasons
- Any diagrams that were close to triggering but didn't (for user awareness)

AskUserQuestion:
- "Looks good — done"
- "Add/modify specific diagrams"
- "Regenerate with different scope"