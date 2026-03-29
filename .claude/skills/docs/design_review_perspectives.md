# Design Review Perspectives

Additional perspectives to check during acceptance criteria review and technical design review when design artifacts (Figma, drawio, design_context.md) exist.

## When to Apply

Apply these perspectives when any of the following conditions are met:
- `design_context.md` exists in the PBI directory
- `.drawio` files exist in the PBI directory
- Acceptance criteria or technical design references a Figma URL

**Skip these perspectives if none of the above apply.**

## Additional Perspectives for Acceptance Criteria Review

| Perspective | Check Content |
|------------|---------------|
| Visual state coverage | Are all states defined in the design (normal, loading, error, empty, disabled) reflected in the acceptance criteria? |
| Interaction completeness | Are all major operation paths (start → complete → error) fully covered as scenarios? |
| UI copy/label consistency | Do screen element names used in acceptance criteria match the design and stock/glossary.md? |
| State transition clarity | Are screen state transition triggers and destinations described without ambiguous expressions? |

## Additional Perspectives for Technical Design Review

| Perspective | Check Content |
|------------|---------------|
| Component mapping | Are design components correctly mapped to implementation components? |
| Design constraint reflection | Are responsive, accessibility, and other constraints reflected in the technical design? |
| State management design | Is state management designed to handle all states from the design? |
| Existing component reuse | Are existing components from the implementation repository being appropriately reused? |
