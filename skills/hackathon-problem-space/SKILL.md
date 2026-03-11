# hackathon-problem-space

## Goal
Map the problem domain for a hackathon project by identifying target users, core pain points, existing solutions, and whitespace opportunities.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `track_summary` | string | Yes | Output from `hackathon-track-analyzer` or manual description |
| `domain` | string | Yes | Broad domain area (e.g., healthcare, fintech, education) |
| `target_user_hint` | string | No | Initial hypothesis about target user segment |
| `constraints` | string[] | No | Known technical or sponsor constraints |

---

## Outputs

| Output | Description |
|---|---|
| `user_segments` | Identified user groups with brief profiles |
| `pain_points` | Ranked list of pain points per user segment |
| `existing_solutions` | Known tools or approaches currently addressing the problem |
| `solution_gaps` | Gaps in existing solutions that represent opportunity |
| `problem_statement` | One-sentence problem statement (how-might-we format) |
| `market_signals` | Evidence that this problem is real and worth solving |
| `recommended_skills` | Suggested next skills to invoke |

---

## Rules

1. Identify at least 2 distinct user segments.
2. Rank pain points from most to least severe for each segment.
3. List at least 3 existing solutions with their key limitations.
4. Derive solution gaps from the delta between pain points and existing solution limitations.
5. Write the problem statement as: "How might we [action] for [user] so that [outcome]?"
6. Do not invent market signals; clearly mark inferred signals as `[INFERRED]`.

---

## Output Format

```yaml
user_segments:
  - segment: "<name>"
    profile: "<brief description>"

pain_points:
  - segment: "<name>"
    pains:
      - severity: "<high|medium|low>"
        description: "<pain>"

existing_solutions:
  - name: "<solution>"
    limitation: "<key gap>"

solution_gaps:
  - "<gap description>"

problem_statement: "<How might we ...>"

market_signals:
  - source: "<real|[INFERRED]>"
    signal: "<description>"

recommended_skills:
  - "<skill-name>"
```
