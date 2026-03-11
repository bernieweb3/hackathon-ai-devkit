# hackathon-code-implementer

## Goal
Provide structured implementation guidance for a hackathon project task, including code patterns, integration strategies, and shortcuts appropriate for prototype speed.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `task_title` | string | Yes | Task being implemented (from `hackathon-task-planner`) |
| `task_description` | string | Yes | Detailed description of what the task must achieve |
| `tech_stack` | string[] | Yes | Technologies in use |
| `mvp_demo_flow` | object[] | Yes | Demo flow steps from `hackathon-scope-cutter` |
| `time_budget_hours` | number | Yes | Hours allocated to this task |
| `existing_code_context` | string | No | Relevant existing code snippets or file structure |
| `fake_ok` | boolean | No | Whether hardcoded/simulated data is acceptable (default: false) |

---

## Outputs

| Output | Description |
|---|---|
| `implementation_plan` | Ordered list of sub-steps to complete the task |
| `code_scaffolds` | Key code snippets, patterns, or stubs to start from |
| `integration_points` | Where this task connects to other components |
| `shortcuts` | Hackathon-appropriate shortcuts (mocks, hardcoding, libraries) |
| `gotchas` | Common failure modes to avoid |
| `done_criteria` | Conditions that signal the task is complete |

---

## Rules

1. Prioritize working code over clean code; note tech debt explicitly.
2. Recommend existing libraries over custom implementations whenever possible.
3. If `fake_ok` is true, provide mock/stub patterns alongside real implementations.
4. Keep `code_scaffolds` minimal — entry points only, not full implementations.
5. `done_criteria` must be observable and verifiable within `time_budget_hours`.
6. Flag any sub-step that risks taking longer than 50% of `time_budget_hours` as `[HIGH-RISK]`.
7. Do not generate production-quality architecture; optimize for demo completeness.

---

## Output Format

```yaml
implementation_plan:
  - step: <number>
    action: "<what to do>"
    risk: "<[HIGH-RISK]|normal>"

code_scaffolds:
  - label: "<purpose>"
    language: "<language>"
    snippet: |
      <code>

integration_points:
  - component: "<name>"
    connection: "<how this task connects>"

shortcuts:
  - shortcut: "<description>"
    trade_off: "<what is sacrificed>"

gotchas:
  - "<pitfall>"

done_criteria:
  - "<verifiable condition>"
```
