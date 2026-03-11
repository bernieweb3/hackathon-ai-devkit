# hackathon-task-planner

## Goal
Decompose the MVP scope into a sequenced, time-boxed task list with assigned roles and clear dependencies for execution during the hackathon.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `mvp_features` | object[] | Yes | MVP features with time budgets from `hackathon-scope-cutter` |
| `tech_stack` | string[] | Yes | Technologies being used |
| `team_size` | integer | Yes | Number of team members |
| `team_roles` | string[] | No | Role labels (e.g., frontend, backend, ML, design) |
| `hackathon_duration_hours` | integer | Yes | Total hours available |
| `start_offset_hours` | integer | No | Hours already elapsed since hackathon start (default: 0) |

---

## Outputs

| Output | Description |
|---|---|
| `tasks` | Full task list with estimates, roles, and dependencies |
| `critical_path` | Ordered sequence of tasks that gate project completion |
| `milestones` | Key checkpoints with target hour marks |
| `parallel_tracks` | Task groups that can be worked simultaneously |
| `buffer_hours` | Hours reserved for integration, polish, and debugging |
| `recommended_skills` | Suggested next skills to invoke |

---

## Rules

1. Decompose every MVP feature into tasks of 30 minutes to 3 hours each.
2. Include setup, integration, and deployment tasks explicitly.
3. Assign each task to exactly one role from `team_roles` (or "any" if unspecified).
4. Identify the critical path as the longest dependency chain.
5. Reserve `buffer_hours` = 15% of remaining hackathon time minimum.
6. Order `milestones` at 25%, 50%, 75%, and 90% of remaining time.
7. Flag any task without a clear owner as `[UNASSIGNED]`.

---

## Output Format

```yaml
tasks:
  - id: "T-<number>"
    title: "<task title>"
    feature: "<parent feature>"
    role: "<role|[UNASSIGNED]>"
    estimated_hours: <number>
    depends_on:
      - "T-<number>"

critical_path:
  - "T-<number>"

milestones:
  - name: "<milestone name>"
    target_hour: <number>
    deliverable: "<what must exist>"

parallel_tracks:
  - track: "<track name>"
    tasks:
      - "T-<number>"

buffer_hours: <number>

recommended_skills:
  - "<skill-name>"
```
