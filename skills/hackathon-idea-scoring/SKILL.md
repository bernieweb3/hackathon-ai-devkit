# hackathon-idea-scoring

## Goal
Evaluate and rank candidate project ideas against a weighted scoring rubric aligned with hackathon judging criteria and team capabilities.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `ideas` | object[] | Yes | Ideas list from `hackathon-idea-generator` |
| `evaluation_axes` | object[] | Yes | Scoring axes from `hackathon-track-analyzer` |
| `team_skills` | string[] | Yes | Technologies and domains the team is proficient in |
| `hackathon_duration_hours` | integer | Yes | Total hours available |
| `team_size` | integer | Yes | Number of team members |
| `weights` | object | No | Custom weight overrides per evaluation axis |

---

## Outputs

| Output | Description |
|---|---|
| `scored_ideas` | Each idea with scores per axis and total score |
| `ranking` | Ordered list of ideas by total score |
| `top_recommendation` | Single recommended idea with rationale |
| `risk_flags` | Concerns for each top-3 idea |
| `recommended_skills` | Suggested next skills to invoke |

---

## Rules

1. Score every idea on every evaluation axis using a 1–5 scale.
2. Apply axis weights when computing total score; default weight is 1.0 if not specified.
3. Apply a `feasibility_penalty` (-0.5 per axis point) if the team lacks skills required.
4. Apply a `time_penalty` (-1.0) if the idea cannot realistically ship in `hackathon_duration_hours`.
5. Surface at least one risk flag per top-3 idea.
6. Select `top_recommendation` based on highest adjusted total score.
7. Break ties in favor of the idea with the highest feasibility score.

---

## Output Format

```yaml
scored_ideas:
  - id: "<idea-N>"
    title: "<title>"
    scores:
      - axis: "<axis name>"
        score: <1-5>
        notes: "<rationale>"
    penalties:
      - type: "<feasibility|time>"
        value: <number>
    total_score: <number>

ranking:
  - rank: <number>
    id: "<idea-N>"
    total_score: <number>

top_recommendation:
  id: "<idea-N>"
  title: "<title>"
  rationale: "<string>"

risk_flags:
  - id: "<idea-N>"
    risks:
      - "<risk>"

recommended_skills:
  - "<skill-name>"
```
