# hackathon-demo-video

## Goal
Produce a structured script and shot list for a hackathon demo video that showcases the project's core value within a strict time limit.

---

## Inputs

| Input | Type | Required | Description |
|---|---|---|---|
| `project_title` | string | Yes | Name of the project |
| `tagline` | string | Yes | One-sentence description |
| `mvp_demo_flow` | object[] | Yes | Demo steps from `hackathon-scope-cutter` |
| `wow_factor` | string | Yes | The single most impressive moment |
| `video_duration_seconds` | integer | Yes | Target video length in seconds |
| `demo_environment` | string | No | Where the demo runs (e.g., web browser, mobile, CLI) |
| `voiceover` | boolean | No | Whether the video includes voiceover (default: true) |

---

## Outputs

| Output | Description |
|---|---|
| `script` | Time-coded narration script |
| `shot_list` | What is shown on screen at each moment |
| `wow_moment_timestamp` | When in the video the wow factor hits |
| `recording_checklist` | Pre-recording setup and validation steps |
| `fallback_plan` | What to do if live demo fails during recording |

---

## Rules

1. The first 5 seconds must state the problem or hook — no introductions.
2. `wow_factor` must appear before the 60% mark of `video_duration_seconds`.
3. Every `mvp_demo_flow` step must map to at least one shot.
4. Narration sentences must be under 15 words each.
5. Reserve the final 10% of video time for a summary and call-to-action.
6. `recording_checklist` must include at least: notifications off, test data ready, screen resolution set.
7. `fallback_plan` must be a static slide or pre-recorded segment, never "we'll skip this part."

---

## Output Format

```yaml
wow_moment_timestamp: <seconds>

script:
  - timestamp_start: <seconds>
    timestamp_end: <seconds>
    narration: "<spoken words>"

shot_list:
  - timestamp_start: <seconds>
    timestamp_end: <seconds>
    screen_content: "<what is visible>"
    action: "<what the presenter does>"

recording_checklist:
  - "<item>"

fallback_plan:
  trigger: "<condition>"
  action: "<what to do>"
```
