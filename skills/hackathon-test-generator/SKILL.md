# hackathon-test-generator

## Goal
Generate a focused set of test cases and a lightweight test coverage plan that validates the demo flow without requiring full test suite completeness.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `mvp_features` | string[] | Yes | MVP features from `hackathon-scope-cutter` |
| `mvp_demo_flow` | object[] | Yes | Demo flow steps from `hackathon-scope-cutter` |
| `tech_stack` | string[] | Yes | Technologies in use |
| `done_criteria` | string[] | Yes | Done criteria from `hackathon-code-implementer` |
| `time_budget_hours` | number | Yes | Hours available for testing |

---

## Outputs

| Output | Description |
|---|---|
| `test_cases` | Prioritized test cases covering the demo flow |
| `coverage_plan` | What is tested vs. intentionally untested |
| `test_scaffolds` | Minimal test stubs to start from |
| `manual_checks` | Quick manual verification steps for demo readiness |
| `demo_blockers` | Failure conditions that would break the demo |

---

## Rules

1. Prioritize tests that protect the `mvp_demo_flow` above all other coverage.
2. Generate at minimum one test per `done_criterion`.
3. Mark any test not critical to the demo as `[NICE-TO-HAVE]`.
4. Include at least one negative/edge case test per MVP feature.
5. `test_scaffolds` must use the testing framework standard for the primary `tech_stack` language.
6. `manual_checks` must be completable in under 5 minutes total.
7. Any scenario that would cause a live demo failure must appear in `demo_blockers`.

---

## Output Format

```yaml
test_cases:
  - id: "TC-<number>"
    feature: "<feature name>"
    description: "<what is tested>"
    type: "<unit|integration|e2e|manual>"
    priority: "<critical|high|[NICE-TO-HAVE]>"
    input: "<test input>"
    expected_output: "<expected result>"

coverage_plan:
  covered:
    - "<area>"
  intentionally_skipped:
    - area: "<area>"
      reason: "<why skipped>"

test_scaffolds:
  - label: "<test name>"
    language: "<language>"
    snippet: |
      <code>

manual_checks:
  - step: <number>
    action: "<what to do>"
    pass_condition: "<what success looks like>"

demo_blockers:
  - scenario: "<failure scenario>"
    mitigation: "<how to prevent or recover>"
```
