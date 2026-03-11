# hackathon-track-analyzer

## Goal
Parse hackathon track descriptions, sponsor briefs, and theme statements to extract structured constraints, evaluation criteria, and strategic opportunities.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `track_description` | string | Yes | Raw text of the hackathon track or theme announcement |
| `sponsor_briefs` | string[] | No | Sponsor challenge briefs or prize track descriptions |
| `judging_rubric` | string | No | Any published judging criteria or scoring rubric |
| `hackathon_duration_hours` | integer | Yes | Total hackathon duration in hours |

---

## Outputs

| Output | Description |
|---|---|
| `track_summary` | One-sentence summary of the track's core theme |
| `required_constraints` | Mandatory constraints teams must satisfy |
| `optional_constraints` | Nice-to-have constraints for bonus scoring |
| `sponsor_priorities` | Ordered list of what each sponsor values most |
| `evaluation_axes` | Scoring dimensions extracted from rubric or inferred |
| `strategic_opportunities` | High-leverage angles or underserved areas within the track |
| `disqualifiers` | Conditions that would disqualify a submission |
| `recommended_skills` | Suggested devkit skills to invoke next |

---

## Rules

1. Extract only what is explicitly stated or strongly implied; do not fabricate constraints.
2. Prioritize mandatory constraints over optional ones.
3. Flag ambiguous language with `[AMBIGUOUS]` marker.
4. Map each sponsor's stated priorities to 1–3 concrete actionable requirements.
5. Identify at least one underexplored strategic opportunity per track.
6. Output `recommended_skills` drawn only from the hackathon-ai-devkit skill list.

---

## Output Format

```yaml
track_summary: "<string>"

required_constraints:
  - "<constraint>"

optional_constraints:
  - "<constraint>"

sponsor_priorities:
  - sponsor: "<name>"
    priorities:
      - "<priority>"

evaluation_axes:
  - axis: "<name>"
    weight: "<high|medium|low>"
    description: "<string>"

strategic_opportunities:
  - "<opportunity>"

disqualifiers:
  - "<condition>"

recommended_skills:
  - "<skill-name>"
```
