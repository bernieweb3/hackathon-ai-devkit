# Hackathon Workflow Orchestration

Defines the recommended skill execution order for a complete hackathon project lifecycle. Use this as the master orchestration guide when running the devkit end-to-end.

---

## Workflow Diagram

```
Track Understanding → Idea Development → Scope Definition → Project Planning
                                                                    ↓
                                              Submission ← Evaluation ← Demo Preparation ← Implementation
```

---

## Phase Overview

| Phase | Skills | Gate Condition |
|---|---|---|
| 1. Track Understanding | `hackathon-track-analyzer` | Constraints and criteria extracted |
| 2. Idea Development | `hackathon-problem-space` → `hackathon-idea-generator` → `hackathon-idea-scoring` | Top idea selected |
| 3. Scope Definition | `hackathon-scope-cutter` → `hackathon-wow-detector` | MVP and demo flow locked |
| 4. Project Planning | `hackathon-doc-writer` → `hackathon-task-planner` | Task list with estimates ready |
| 5. Implementation | `hackathon-code-implementer` → `hackathon-test-generator` | Demo runs end-to-end |
| 6. Demo Preparation | `hackathon-demo-video` → `hackathon-pitchdeck` | Video uploaded, slides ready |
| 7. Evaluation | `hackathon-judge-simulator` | Q&A prepared, pitch refined |
| 8. Submission | `hackathon-submission-prep` | All artifacts submitted |

---

## Phase 1: Track Understanding

**Objective:** Extract the judging environment before generating any ideas.

**Skills:**
- [`hackathon-track-analyzer`](../skills/hackathon-track-analyzer/SKILL.md) — Parse track description, sponsor briefs, and judging rubric into structured constraints and evaluation axes.

**Required Inputs:**
- Raw track description text
- Sponsor briefs (if available)
- Judging rubric (if published)
- Hackathon duration in hours

**Key Outputs:**
- `track_summary`
- `required_constraints`
- `evaluation_axes`
- `strategic_opportunities`
- `disqualifiers`

**Gate:** Do not proceed to Phase 2 until `evaluation_axes` and `required_constraints` are confirmed.

**Recommended Knowledge:**
- `knowledge/hackathon-judging-criteria.md`
- `knowledge/hackathon-winning-patterns.md`

---

## Phase 2: Idea Development

**Objective:** Systematically generate and select the best project idea.

**Skills (run in sequence):**
1. [`hackathon-problem-space`](../skills/hackathon-problem-space/SKILL.md) — Map target users, pain points, and solution gaps.
2. [`hackathon-idea-generator`](../skills/hackathon-idea-generator/SKILL.md) — Produce 3–7 candidate ideas that address the problem space.
3. [`hackathon-idea-scoring`](../skills/hackathon-idea-scoring/SKILL.md) — Score and rank ideas; select the top recommendation.

**Key Data Flow:**
```
track_analyzer.track_summary → problem_space.track_summary
problem_space.solution_gaps  → idea_generator.solution_gaps
problem_space.problem_statement → idea_generator.problem_statement
idea_generator.ideas         → idea_scoring.ideas
track_analyzer.evaluation_axes → idea_scoring.evaluation_axes
```

**Gate:** Team commits to exactly one idea before Phase 3. No idea changes after this gate.

**Recommended Knowledge:**
- `knowledge/hackathon-winning-patterns.md`
- `knowledge/hackathon-common-failures.md` (scope failure patterns)

---

## Phase 3: Scope Definition

**Objective:** Reduce the idea to a shippable MVP and identify the wow moment.

**Skills (run in sequence):**
1. [`hackathon-scope-cutter`](../skills/hackathon-scope-cutter/SKILL.md) — Cut features to MVP; define demo flow and time budget.
2. [`hackathon-wow-detector`](../skills/hackathon-wow-detector/SKILL.md) — Identify and amplify the primary wow moment.

**Key Data Flow:**
```
idea_scoring.top_recommendation → scope_cutter.project_title + core_mechanism
scope_cutter.mvp_features       → wow_detector.mvp_features
scope_cutter.mvp_demo_flow      → wow_detector.mvp_demo_flow
track_analyzer.evaluation_axes  → wow_detector.evaluation_axes
```

**Gate:** `mvp_demo_flow` is locked. No new MVP features after this gate.

**Recommended Knowledge:**
- `knowledge/hackathon-mvp-strategy.md`

---

## Phase 4: Project Planning

**Objective:** Document decisions and sequence work into executable tasks.

**Skills (run in parallel or sequence):**
- [`hackathon-doc-writer`](../skills/hackathon-doc-writer/SKILL.md) — Generate PRD and key ADRs for architectural decisions.
- [`hackathon-task-planner`](../skills/hackathon-task-planner/SKILL.md) — Decompose MVP into time-boxed tasks with roles and dependencies.

**Key Data Flow:**
```
scope_cutter.mvp_features   → task_planner.mvp_features
scope_cutter.time_budget    → task_planner (validates estimates)
scope_cutter.mvp_demo_flow  → doc_writer.mvp_features
```

**Key Outputs:**
- Task list with `critical_path`
- `milestones` at 25/50/75/90% time marks
- `buffer_hours` reservation
- PRD and ADR documents

**Gate:** Every team member has their tasks. Critical path is known.

**Templates:**
- `templates/PRD-template.md`
- `templates/ADR-template.md`
- `templates/feature-spec-template.md`

---

## Phase 5: Implementation

**Objective:** Build the demo path. Protect the wow feature.

**Skills (iterate per task):**
- [`hackathon-code-implementer`](../skills/hackathon-code-implementer/SKILL.md) — Invoke per task from the task planner. Get implementation plan, code scaffolds, and done criteria.
- [`hackathon-test-generator`](../skills/hackathon-test-generator/SKILL.md) — Invoke once the demo flow is connected. Generate demo-blocking test cases and manual verification checklist.

**Key Data Flow:**
```
task_planner.tasks[each]    → code_implementer (one invocation per task)
code_implementer.done_criteria → test_generator.done_criteria
scope_cutter.mvp_demo_flow  → test_generator.mvp_demo_flow
```

**Implementation Checkpoints:**

| Time Used | Required State |
|---|---|
| 25% | Project scaffold running; core API endpoint responds |
| 50% | Core mechanism working; connected end-to-end |
| 75% | Full demo flow runs cleanly |
| 90% | Demo path frozen; all demo-blocking tests pass |

**Gate:** Demo runs end-to-end without crashing. `demo_blockers` are resolved.

**Recommended Knowledge:**
- `knowledge/hackathon-tools.md`
- `knowledge/hackathon-common-failures.md` (technical failure patterns)
- `knowledge/hackathon-mvp-strategy.md`

---

## Phase 6: Demo Preparation

**Objective:** Produce the demo video and pitch deck.

**Skills (can run in parallel once implementation is frozen):**
- [`hackathon-demo-video`](../skills/hackathon-demo-video/SKILL.md) — Script and record a time-coded demo video.
- [`hackathon-pitchdeck`](../skills/hackathon-pitchdeck/SKILL.md) — Build slide deck with speaker notes and judging alignment.

**Key Data Flow:**
```
scope_cutter.mvp_demo_flow   → demo_video.mvp_demo_flow
wow_detector.primary_wow_moment → demo_video.wow_factor
wow_detector.primary_wow_moment → pitchdeck.wow_factor
track_analyzer.evaluation_axes  → pitchdeck.evaluation_axes
```

**Gate:** Demo video uploaded. Pitch deck slide count and timing are verified against pitch duration.

**Templates:**
- `templates/demo-script-template.md`
- `templates/pitchdeck-outline.md`

**Recommended Knowledge:**
- `knowledge/hackathon-demo-patterns.md`
- `knowledge/hackathon-demo-psychology.md`
- `knowledge/hackathon-pitch-strategy.md`

---

## Phase 7: Evaluation

**Objective:** Simulate judging and harden the pitch against adversarial questions.

**Skills:**
- [`hackathon-judge-simulator`](../skills/hackathon-judge-simulator/SKILL.md) — Generate judge personas, expected questions, objections, and predicted scores.

**Key Data Flow:**
```
pitchdeck.slides             → judge_simulator.pitch_content
track_analyzer.evaluation_axes → judge_simulator.evaluation_axes
scope_cutter.mvp_features    → judge_simulator.mvp_features
```

**Key Outputs:**
- `questions` with recommended answers
- `objections` with rebuttal strategies
- `pitch_improvements` ordered by priority
- `predicted_scores` per evaluation axis

**Gate:** All `high` priority `pitch_improvements` addressed. Top 5 hard questions rehearsed.

**Recommended Knowledge:**
- `knowledge/hackathon-demo-psychology.md` (Q&A psychology section)
- `knowledge/hackathon-judging-criteria.md`

---

## Phase 8: Submission

**Objective:** Compile and validate all submission artifacts.

**Skills:**
- [`hackathon-submission-prep`](../skills/hackathon-submission-prep/SKILL.md) — Generate submission description, validate checklist, and produce last-mile action list.

**Key Data Flow:**
```
pitchdeck.opening_hook + slides → submission_prep.solution_summary
demo_video output               → submission_prep.video_url
repo_url                        → submission_prep.repo_url
```

**Submission Deadline Rules:**
- Complete submission draft: deadline minus 4 hours
- All artifacts validated: deadline minus 2 hours
- Final submission confirmed: deadline minus 1 hour
- Never submit in the final 15 minutes

**Gate:** `submission_quality_score ≥ 7`. All `blocking` missing artifacts resolved.

**Recommended Knowledge:**
- `knowledge/hackathon-submission-guidelines.md`

---

## Skill Dependency Map

```
hackathon-track-analyzer
    ├── hackathon-problem-space
    │       └── hackathon-idea-generator
    │               └── hackathon-idea-scoring
    │                       └── hackathon-scope-cutter
    │                               ├── hackathon-wow-detector
    │                               ├── hackathon-doc-writer
    │                               ├── hackathon-task-planner
    │                               │       └── hackathon-code-implementer
    │                               │               └── hackathon-test-generator
    │                               ├── hackathon-demo-video
    │                               └── hackathon-pitchdeck
    │                                       └── hackathon-judge-simulator
    │                                               └── hackathon-submission-prep
    └── (evaluation_axes feeds) hackathon-idea-scoring
                                hackathon-wow-detector
                                hackathon-pitchdeck
                                hackathon-judge-simulator
```

---

## Time Allocation by Workflow Phase

| Phase | 24h | 36h | 48h |
|---|---|---|---|
| 1. Track Understanding | H+0 → H+0:30 | H+0 → H+0:45 | H+0 → H+0:45 |
| 2. Idea Development | H+0:30 → H+1:30 | H+0:45 → H+2:00 | H+0:45 → H+2:15 |
| 3. Scope Definition | H+1:30 → H+2:00 | H+2:00 → H+3:00 | H+2:15 → H+4:00 |
| 4. Project Planning | H+2:00 → H+2:30 | H+3:00 → H+4:00 | H+4:00 → H+5:00 |
| 5. Implementation | H+2:30 → H+15 | H+4:00 → H+22 | H+5:00 → H+30 |
| 6. Demo Preparation | H+15 → H+18 | H+22 → H+26 | H+30 → H+36 |
| 7. Evaluation | H+18 → H+20 | H+26 → H+28 | H+36 → H+38 |
| 8. Submission | H+20 → H+21 | H+28 → H+30 | H+38 → H+40 |

For detailed per-phase breakdowns, see the appropriate playbook:
- [`24h-hackathon-playbook.md`](24h-hackathon-playbook.md)
- [`36h-hackathon-playbook.md`](36h-hackathon-playbook.md)
- [`48h-hackathon-playbook.md`](48h-hackathon-playbook.md)
