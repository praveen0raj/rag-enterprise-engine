# Technical Design Review Perspectives

Review the technical design from the following perspectives. Check in priority order and collect findings.

## High Priority (must check)

| Perspective | Check Items |
|-------------|-------------|
| Alignment with acceptance criteria | Does the design satisfy all acceptance criteria items? |
| Feasibility | Is it technically feasible and consistent with existing architecture? |
| Completeness | Are all necessary design items covered (error handling, transactions, etc.)? |
| Clarity | Is it specific enough that implementers won't be confused? |

## Medium Priority

| Perspective | Check Items |
|-------------|-------------|
| Consistency with existing code | Does it follow existing patterns and conventions? |
| Impact scope | Is the scope of change impact properly considered? |
| Performance | Are there any performance concerns? |
| Security | Are there any security concerns? |
| Testability | Is the design easy to test? |
| Maintainability | Can the design accommodate future changes? |

## Domain Model / Object Diagram Consistency (if available)

If domain model diagrams or object diagrams exist in the project, verify consistency:

| Perspective | Check Items |
|-------------|-------------|
| Entity names / attribute names | Do domain model diagram names match technical design descriptions? |
| Relationships | Do domain model diagram relationships align with technical design structure? |
| Domain knowledge (rules/constraints) | Are domain model rules and constraints reflected in the technical design? |
| Object diagram examples | Do object diagram examples align with technical design scenarios? |
