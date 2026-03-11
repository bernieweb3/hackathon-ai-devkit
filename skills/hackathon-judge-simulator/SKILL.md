# hackathon-judge-simulator

## Goal
Simulate a panel of hackathon judges evaluating a project, generating likely questions, critical objections, and a predicted scoring outcome so the team can strengthen their pitch and demo.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the project |
| `problem_statement` | string | Yes | The problem being solved |
| `solution_summary` | string | Yes | How the project solves it |
| `mvp_features` | string[] | Yes | What was built |
| `tech_stack` | string[] | Yes | Technologies used |
| `evaluation_axes` | object[] | Yes | Judging criteria from `hackathon-track-analyzer` |
| `pitch_content` | string | No | Draft pitch or slide content for more targeted simulation |
| `judge_personas` | string[] | No | Types of judges expected (e.g., technical, business, domain expert) |

---

## Outputs

| Output | Description |
|---|---|
| `judge_personas_used` | Simulated judge types with their likely priorities |
| `questions` | Expected judge questions with recommended answers |
| `objections` | Critical concerns judges are likely to raise |
| `predicted_scores` | Score per evaluation axis with reasoning |
| `overall_verdict` | Simulated overall impression and ranking likelihood |
| `pitch_improvements` | Specific changes to address predicted weaknesses |

---

## Rules

1. Simulate at least 3 distinct judge personas if `judge_personas` is not provided.
2. Generate at least 2 questions per evaluation axis.
3. Include at least one question that targets a weakness in the solution.
4. `predicted_scores` must use the same 1–5 scale as `hackathon-idea-scoring`.
5. `objections` must be paired with a recommended rebuttal strategy.
6. `pitch_improvements` must be actionable within the remaining hackathon time.
7. Do not simulate only favorable outcomes; include at least one skeptical judge perspective.

---

## Output Format

```yaml
judge_personas_used:
  - persona: "<type>"
    priorities:
      - "<priority>"

questions:
  - judge_persona: "<type>"
    question: "<question text>"
    recommended_answer: "<suggested response>"
    difficulty: "<easy|medium|hard>"

objections:
  - objection: "<concern>"
    likelihood: "<high|medium|low>"
    rebuttal_strategy: "<how to address>"

predicted_scores:
  - axis: "<axis name>"
    score: <1-5>
    reasoning: "<why>"

overall_verdict:
  impression: "<string>"
  ranking_likelihood: "<top-3|mid-field|long-shot>"
  key_strengths:
    - "<strength>"
  key_weaknesses:
    - "<weakness>"

pitch_improvements:
  - issue: "<problem>"
    action: "<what to change>"
    priority: "<high|medium|low>"
```
