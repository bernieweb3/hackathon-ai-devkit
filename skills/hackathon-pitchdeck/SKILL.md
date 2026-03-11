# hackathon-pitchdeck

## Goal
Construct a complete hackathon pitch deck narrative with slide-by-slide content, speaker notes, and a persuasive storyline aligned to judging criteria.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the project |
| `tagline` | string | Yes | One-sentence project description |
| `problem_statement` | string | Yes | The problem being solved |
| `solution_summary` | string | Yes | How the project solves the problem |
| `mvp_demo_flow` | object[] | Yes | Demo steps from `hackathon-scope-cutter` |
| `target_user` | string | Yes | Primary user segment |
| `wow_factor` | string | Yes | The single most impressive aspect |
| `evaluation_axes` | object[] | Yes | Judging criteria from `hackathon-track-analyzer` |
| `team_members` | string[] | Yes | Team member names and roles |
| `pitch_duration_minutes` | integer | No | Available pitch time (default: 3) |

---

## Outputs

| Output | Description |
|---|---|
| `slides` | Ordered slide definitions with title, content, and speaker notes |
| `opening_hook` | First 15-second attention-grabbing statement |
| `closing_call_to_action` | Final memorable statement for judges |
| `judging_alignment` | How each slide addresses a judging axis |

---

## Rules

1. Map every slide to at least one `evaluation_axis`.
2. Open with the problem, not the team or technology.
3. Lead with the `wow_factor` within the first 60 seconds.
4. Include a live demo slide referencing `mvp_demo_flow`.
5. Each slide must be completable in under 30 seconds of speaking time.
6. Do not use more than 7 words per bullet point on any slide.
7. End with a memorable `closing_call_to_action`, not a "thank you" slide.

---

## Output Format

```yaml
opening_hook: "<string>"

slides:
  - number: <number>
    title: "<slide title>"
    type: "<hook|problem|solution|demo|technology|team|vision|cta>"
    bullets:
      - "<bullet>"
    speaker_notes: "<what to say>"
    judging_axes_addressed:
      - "<axis name>"

closing_call_to_action: "<string>"

judging_alignment:
  - axis: "<axis name>"
    addressed_in_slides:
      - <slide number>
```
