# Acceptance Criteria Review Perspectives

Review the acceptance criteria from the following perspectives. Check in priority order and collect findings.

## High Priority (Always Check)

| Perspective | Check Content |
|------------|---------------|
| Completeness | Are necessary requirements fully covered? |
| Ambiguity | Is there insufficient specificity? (Ambiguous expressions like "as appropriate", "as needed") |
| Boundary conditions | Are error cases and edge cases considered? |
| Consistency | Are there contradictions between requirements? |

## Medium Priority

| Perspective | Check Content |
|------------|---------------|
| Scope | Is the scope too broad or too narrow? |
| Assumptions | Are implicit assumptions explicitly documented? |
| Terminology | Is terminology consistent? |
| User perspective | Is it valuable to end users? |
| Business value | Is it aligned with business goals? |

## Domain Model / Object Diagram Consistency (If They Exist)

If domain model diagrams or object diagrams exist in the project, check for consistency:

| Perspective | Check Content |
|------------|---------------|
| Entity/attribute names | Do names in domain model diagrams match PBI descriptions? |
| Relationships | Are relationships in domain model diagrams consistent with PBI descriptions? |
| Domain knowledge (rules/constraints) | Are domain model rules/constraints consistent with PBI descriptions? |
| Object diagram examples | Are concrete examples in object diagrams consistent with PBI scenarios? |
| Example reflection | Is there room to reflect object diagram examples in acceptance criteria? |

## Domain Knowledge Document Consistency (If Related Categories Exist)

For domain knowledge documents under `docs/stock/` loaded following the domain knowledge progressive reference guide, verify bidirectional consistency:

| Perspective | Check Content |
|------------|---------------|
| Domain knowledge → Acceptance criteria | Are rules, constraints, and notes documented in domain knowledge reflected in acceptance criteria? |
| Acceptance criteria → Domain knowledge | If new specifications or rules are discovered through this acceptance criteria, should domain knowledge documents be updated? |
