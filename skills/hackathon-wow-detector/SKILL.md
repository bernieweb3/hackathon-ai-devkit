# hackathon-wow-detector

## Goal
Identify and amplify the single strongest wow-factor moment in a hackathon project, ensuring it is front-loaded in the demo and pitch for maximum judge impact.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the project |
| `mvp_features` | object[] | Yes | MVP features from `hackathon-scope-cutter` |
| `mvp_demo_flow` | object[] | Yes | Demo steps from `hackathon-scope-cutter` |
| `target_user` | string | Yes | Primary user segment |
| `evaluation_axes` | object[] | Yes | Judging criteria from `hackathon-track-analyzer` |
| `competitor_ideas` | string[] | No | Other projects in the same track, if known |

---

## Outputs

| Output | Description |
|---|---|
| `wow_moments` | All candidate wow moments ranked by impact |
| `primary_wow_moment` | The single strongest moment to lead with |
| `amplification_tactics` | How to make the primary wow moment land harder |
| `demo_placement` | Where in the demo/pitch the wow moment should appear |
| `judge_reaction_prediction` | What judges will likely think/say after seeing it |
| `differentiation_statement` | One sentence that separates this project from others |

---

## Rules

1. Evaluate each feature for emotional impact, novelty, and relevance to `evaluation_axes`.
2. Select `primary_wow_moment` as the feature with highest combined impact × judging weight.
3. `primary_wow_moment` must be demonstrable live, not just described.
4. `amplification_tactics` must include at least: visual framing, narration timing, contrast setup.
5. `demo_placement` must be within the first 40% of the demo runtime.
6. `differentiation_statement` must be falsifiable — it must not apply to generic projects.
7. If `competitor_ideas` are known, ensure `differentiation_statement` is distinct from all of them.

---

## Output Format

```yaml
wow_moments:
  - rank: <number>
    feature: "<feature name>"
    impact_score: <1-10>
    novelty_score: <1-10>
    judging_relevance: <1-10>
    combined_score: <number>
    description: "<why this wows judges>"

primary_wow_moment:
  feature: "<feature name>"
  description: "<what happens>"
  live_demonstrable: <true|false>

amplification_tactics:
  - tactic: "<name>"
    description: "<how to apply it>"

demo_placement:
  position: "<percent through demo>"
  context: "<what comes immediately before>"

judge_reaction_prediction: "<string>"

differentiation_statement: "<string>"
```
