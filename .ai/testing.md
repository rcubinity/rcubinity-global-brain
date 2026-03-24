# Testing

## Balance

- Aim for a **practical mix**: unit tests for pure logic, integration tests for boundaries (DB, HTTP), E2E for critical user paths — exact ratios depend on the team (avoid arbitrary fixed percentages unless policy mandates).

## Requirements

- **New features** should include tests where they add risk or complexity; **bug fixes** should add a regression test when feasible.
- Tests must **pass in CI** before merge to protected branches.

## Quality

- Deterministic tests; avoid flakiness (proper async handling, clocks, network mocks).
- Test names should describe **behavior**, not implementation details only.
