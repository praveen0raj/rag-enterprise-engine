# Test Design Review Perspectives

Review the test design from the following perspectives. Check in priority order and collect findings.

## High Priority (must check)

| Perspective | Check Items |
|-------------|-------------|
| Acceptance criteria coverage | Are all acceptance criteria items covered by tests? |
| Alignment with technical design | Are behaviors defined in the technical design being tested? |
| Boundary values / error cases | Are edge cases and error cases comprehensively covered? |
| Regression | Are there tests confirming no impact on existing functionality? |

## Medium Priority

| Perspective | Check Items |
|-------------|-------------|
| Test independence | Can each test be executed independently? |
| Test data | Is the test data preparation method clear? |
| Auto/manual classification | Is the distinction between automated and manual tests appropriate? |
| Prioritization | Are high-importance tests prioritized? |
| Executability | Are test procedures specific and executable? |

## Domain Model / Object Diagram Consistency (if available)

If domain model diagrams or object diagrams exist in the project, verify consistency:

| Perspective | Check Items |
|-------------|-------------|
| Entity names / attribute names | Do domain model diagram names match test case descriptions? |
| Relationships | Are domain model diagram relationships considered in test cases? |
| Domain knowledge (rules/constraints) | Are domain model rules and constraints covered by test cases? |
| Object diagram examples | Are object diagram examples reflected in test cases? |
| Example coverage | Is there room to add object diagram examples as test cases? |
