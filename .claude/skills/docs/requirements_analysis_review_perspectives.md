# Requirements Analysis Review Perspectives

Review the requirements analysis from the following perspectives. Check in priority order and collect findings.

## High Priority (Always Check)

| Perspective | Check Content |
|------------|---------------|
| Problem clarity | Is the problem statement specific and the issue to solve clearly defined? |
| Completeness | Are all information items from the input covered as requirements? |
| Ambiguity | Is there insufficient specificity? (Ambiguous expressions like "as appropriate", "as needed") |
| Goal alignment | Do functional requirements directly contribute to achieving goals and value? |
| Use case specificity | Are specific usage scenarios (when, where, why) described for each stakeholder? Is "current workaround" documented, making the necessity of the feature clear to engineers? |
| Use case and functional requirements alignment | Can all functional requirements be derived from at least one use case? Are there functional requirements not covered by any use case? |
| Scope validity | Is the target scope neither too broad nor too narrow for solving the problem? Is out-of-scope explicitly stated? |

## Medium Priority

| Perspective | Check Content |
|------------|---------------|
| Stakeholders | Are all affected stakeholders identified? |
| Current state understanding | Is the current system/workflow accurately described? |
| Non-functional requirements | Are performance, security, and operational constraints considered? |
| Priority rationale | Does the priority assessment have rational justification? |
| Risk identification | Are unresolved items and risks appropriately identified? |
| Terminology consistency | Are different terms being used for the same concept? |
| Inter-requirement consistency | Are there contradictions between functional requirements, or between functional and non-functional requirements? |

## Low Priority (Recommended)

| Perspective | Check Content |
|------------|---------------|
| Verifiability | Can completion of each requirement be objectively determined? |
| Input deviation | Does it deviate from the intent of the original input? |
| Assumption documentation | Are implicit assumptions explicitly stated? |

## Domain Model / Object Diagram Consistency (If They Exist)

If domain model diagrams or object diagrams exist in the project, check for consistency:

| Perspective | Check Content |
|------------|---------------|
| Entity/attribute names | Do names in domain model diagrams match descriptions in requirements analysis? |
| Relationships | Are relationships in domain model diagrams consistent with requirements analysis descriptions? |
| Domain knowledge (rules/constraints) | Are domain model rules/constraints consistent with requirements analysis? |
| Object diagram examples | Are concrete examples in object diagrams consistent with requirements analysis scenarios? |
