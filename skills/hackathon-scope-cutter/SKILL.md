# hackathon-scope-cutter

## Goal
Reduce a project's feature set to the minimum viable product (MVP) that can be shipped within the hackathon time limit while preserving demo impact.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the selected project |
| `feature_wishlist` | string[] | Yes | Full desired feature list |
| `core_mechanism` | string | Yes | The single mechanism the project must demonstrate |
| `hackathon_duration_hours` | integer | Yes | Total hours available |
| `team_size` | integer | Yes | Number of team members |
| `team_skills` | string[] | Yes | Technologies the team can use effectively |
| `wow_factor` | string | Yes | What must land for judges to be impressed |

---

## Outputs

| Output | Description |
|---|---|
| `mvp_features` | Features included in the MVP |
| `deferred_features` | Features cut from MVP (post-hackathon backlog) |
| `cut_rationale` | Why each deferred feature was cut |
| `mvp_demo_flow` | Minimal user journey that demonstrates core value |
| `time_budget` | Rough hour estimate per MVP feature |
| `scope_risk` | Remaining risks even after scoping |
| `recommended_skills` | Suggested next skills to invoke |

---

## Rules

1. Include a feature in MVP only if it is required to demonstrate `core_mechanism` or `wow_factor`.
2. Default to cutting any feature that cannot be implemented in less than 20% of total time.
3. Preserve at least one visually compelling UI moment in `mvp_demo_flow`.
4. Total `time_budget` must not exceed 70% of `hackathon_duration_hours` (reserve 30% for polish and presentation).
5. Mark features as `[FAKE-OK]` if they can be simulated or hardcoded for demo purposes.
6. Do not cut the feature that delivers `wow_factor`.

---

## Output Format

```yaml
mvp_features:
  - feature: "<name>"
    purpose: "<why it's essential>"
    fake_ok: <true|false>

deferred_features:
  - feature: "<name>"
    cut_rationale: "<reason>"

mvp_demo_flow:
  - step: <number>
    action: "<user action>"
    outcome: "<visible result>"

time_budget:
  - feature: "<name>"
    estimated_hours: <number>

scope_risk:
  - "<risk>"

recommended_skills:
  - "<skill-name>"
```
