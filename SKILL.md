---
name: test-planner
description: >-
  Generate test plans and test strategy from Jira tickets, feature descriptions,
  or pull requests. Use when the user asks to create a test plan, generate test
  cases, plan test coverage, or asks "what tests should we write for this
  feature?" Works for Robottelo, upstream Foreman, or any pytest-based project.
---

# Test Planner

Generate structured test plans and test strategy from requirements, following
project conventions.

## Workflow

Copy this checklist and track progress:

```
Task Progress:
- [ ] Step 1: Gather requirement input
- [ ] Step 2: Analyze existing coverage
- [ ] Step 3: Generate test plan
- [ ] Step 4: Review and refine
```

### Step 1: Gather Requirement Input

Accept input in any of these forms:
- **Jira ticket ID** (e.g., SAT-12345) — extract summary, description, acceptance criteria
- **Feature description** — free-text description of the feature
- **PR diff** — code changes that need test coverage

Extract from the input:
- **Feature area** — map to a CaseComponent from `testimony.yaml` (e.g., ActivationKeys, ContentViews, Provisioning)
- **Testable requirements** — discrete behaviors that can be verified
- **Acceptance criteria** — conditions that define "done"
- **Affected interfaces** — which of API / CLI / UI are relevant

### Step 2: Analyze Existing Coverage

Search the codebase for existing tests covering the same feature:

1. Search test directories for test files matching the feature area
2. Identify:
   - What scenarios are already covered
   - What interfaces have coverage
   - Gaps in positive/negative/boundary/e2e coverage
3. Search the https://github.com/theforeman/foremanctl/tree/master/tests repo as well for the existing coverage

### Step 3: Generate Test Plan

Produce a structured test plan with these sections:

```markdown
# Test Plan: [Feature Name]

## Feature Summary
[1-2 sentence summary of what is being tested]

## Component: [CaseComponent from testimony.yaml]
## Team: [Team name]
## Interfaces: [API / CLI / UI — which are relevant]

## Test Scenarios

### API Tests (`tests/foreman/api/test_{feature}.py`)

| # | Scenario | Type | Importance | Markers |
|---|----------|------|------------|---------|
| 1 | Create {entity} with valid name | Positive | Critical | parametrize |
| 2 | Create {entity} with invalid name | Negative | Low | parametrize |
| ...

### CLI Tests (`tests/foreman/cli/test_{feature}.py`)
[Same table format but make sure to have this as separate and empty if there are no scenarios]

### UI Tests (`tests/foreman/ui/test_{feature}.py`)
[Same table format but make sure to have this as separate and empty if there are no scenarios]

## Existing Coverage (already tested)
- List tests that already exist for reference
```

#### Test Type Guidelines

**Positive tests** (`test_positive_*`): Happy-path scenarios
- Create/Read/Update/Delete with valid data
- Parametrize with `valid_data_list()` or `gen_string()` variants
- E2e workflows (`@pytest.mark.e2e`)
- A separate table for positive tests if applicable

**Negative tests** (`test_negative_*`): Error-handling scenarios
- Invalid input (use `invalid_names_list()`)
- Missing required fields
- Unauthorized access
- Expect `HTTPError`, `CLIReturnCodeError`, or UI validation messages
- A separate table for negative tests if applicable

**Boundary tests**: Edge cases
- Max/min values
- Empty strings, special characters
- Concurrent operations
- A separate table for boundary tests if applicable

**Upgrade tests** (`tests/new_upgrades/`): Only if feature has upgrade implications
- Uses `SharedResource` pattern
- A separate table for upgrade tests if applicable

### Step 4: Review and Refine

Present the plan to the peers for review. Ask:
- Are there scenarios missing?
- Should any tests be parametrized differently?
- Are there any known blockers or open issues to account for?

## Reference Files

- For complete CaseComponent choices, see `testimony.yaml`
- For test patterns, examine existing tests in `tests/foreman/` and https://github.com/theforeman/foremanctl/tree/master/tests
