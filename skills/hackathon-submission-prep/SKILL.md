# hackathon-submission-prep

## Goal
Compile and validate all required hackathon submission artifacts into a complete, polished submission package ready for upload.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the project |
| `tagline` | string | Yes | One-sentence description |
| `problem_statement` | string | Yes | The problem being solved |
| `solution_summary` | string | Yes | How the project solves it |
| `tech_stack` | string[] | Yes | Technologies used |
| `team_members` | object[] | Yes | Team member names and roles |
| `demo_url` | string | No | Live demo URL |
| `video_url` | string | No | Demo video URL |
| `repo_url` | string | Yes | Source code repository URL |
| `track` | string | Yes | Hackathon track being entered |
| `submission_platform` | string | No | Platform name (e.g., Devpost, Dorahacks) |
| `custom_fields` | object[] | No | Platform-specific required fields |

---

## Outputs

| Output | Description |
|---|---|
| `submission_description` | Full formatted project description for submission |
| `short_description` | 280-character tagline/elevator pitch |
| `artifact_checklist` | All required submission items with completion status |
| `missing_artifacts` | Items not yet provided that must be completed |
| `submission_quality_score` | 1–10 rating of submission readiness |
| `last_mile_actions` | Final actions required before submitting |

---

## Rules

1. `submission_description` must include: problem, solution, tech stack, team, and demo link.
2. `short_description` must be 280 characters or fewer.
3. `artifact_checklist` must verify: repo URL accessible, README exists, demo video linked, team listed.
4. Assign `submission_quality_score` using: completeness (40%), clarity (30%), impact framing (30%).
5. `last_mile_actions` must be ordered by priority with estimated time per action.
6. Flag any missing required field from `custom_fields` as a blocking `missing_artifact`.
7. Do not submit placeholder text; flag any `[TBD]` fields as incomplete in `missing_artifacts`.

---

## Output Format

```yaml
submission_description: |
  <Full Markdown submission text>

short_description: "<≤280 characters>"

artifact_checklist:
  - item: "<artifact name>"
    status: "<complete|incomplete|missing>"
    url_or_value: "<link or value if complete>"

missing_artifacts:
  - item: "<name>"
    blocking: <true|false>
    action_required: "<what to do>"

submission_quality_score:
  score: <1-10>
  breakdown:
    completeness: <1-10>
    clarity: <1-10>
    impact_framing: <1-10>

last_mile_actions:
  - priority: <number>
    action: "<what to do>"
    estimated_minutes: <number>
```
