# hackathon-idea-generator

## Goal
Generate a diverse set of candidate project ideas that address the identified problem space and satisfy hackathon track constraints.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `problem_statement` | string | Yes | How-might-we problem statement |
| `solution_gaps` | string[] | Yes | Gaps from `hackathon-problem-space` |
| `track_constraints` | string[] | Yes | Required constraints from `hackathon-track-analyzer` |
| `team_size` | integer | Yes | Number of team members |
| `hackathon_duration_hours` | integer | Yes | Total hours available |
| `tech_stack_preferences` | string[] | No | Preferred languages, frameworks, platforms |
| `idea_count` | integer | No | Number of ideas to generate (default: 5) |

---

## Outputs

| Output | Description |
|---|---|
| `ideas` | List of candidate project ideas |
| `diversity_axes` | Dimensions of variation across the idea set |
| `recommended_skills` | Suggested next skills to invoke |

---

## Rules

1. Generate exactly `idea_count` ideas (default 5 if not specified).
2. Ensure each idea addresses at least one `solution_gap`.
3. Ensure each idea satisfies all `required_constraints`.
4. Vary ideas across at least 3 dimensions (e.g., complexity, user segment, tech approach).
5. Each idea must be completable by `team_size` people within `hackathon_duration_hours`.
6. Include at least one bold/high-risk idea and one conservative/low-risk idea.
7. Do not duplicate ideas; each must have a meaningfully distinct core mechanism.

---

## Output Format

```yaml
ideas:
  - id: "<idea-N>"
    title: "<short title>"
    tagline: "<one sentence>"
    core_mechanism: "<what makes it work>"
    target_user: "<segment>"
    gaps_addressed:
      - "<gap>"
    risk_level: "<high|medium|low>"
    wow_factor: "<what judges will remember>"

diversity_axes:
  - axis: "<dimension>"
    range: "<low end> → <high end>"

recommended_skills:
  - "<skill-name>"
```
