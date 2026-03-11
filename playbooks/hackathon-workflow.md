# Hackathon Workflow Orchestration

Defines the recommended skill execution order for a complete hackathon project lifecycle. Use this as the master orchestration guide when running the devkit end-to-end.

---

## Workflow Diagram

```
[URL] → Event Parsing → Track Understanding → Idea Development → Scope Definition → Project Planning
                                                                                           ↓
                                                       Submission ← Evaluation ← Demo Preparation ← Implementation
```

---

## Phase → Skill Mapping Reference

| Phase | Skills | Mode |
|---|---|---|
| **0. Event Parsing** | `hackathon-event-parser` | Once; autonomous entry point |
| **1. Track Understanding** | `hackathon-track-analyzer` | Once; feeds all downstream phases |
| **2. Idea Development** | `hackathon-problem-space` → `hackathon-idea-generator` → `hackathon-idea-scoring` | Sequential |
| **3. Scope Definition** | `hackathon-scope-cutter` → `hackathon-wow-detector` | Sequential |
| **4. Project Planning** | `hackathon-doc-writer` + `hackathon-task-planner` | Parallel |
| **5. Implementation** | `hackathon-code-implementer` (×N tasks) → `hackathon-test-generator` | Per-task iteration |
| **6. Demo Preparation** | `hackathon-demo-video` + `hackathon-pitchdeck` | Parallel after code freeze |
| **7. Evaluation** | `hackathon-judge-simulator` | Once; re-invoke after pitch edits |
| **8. Submission** | `hackathon-submission-prep` | Once; 1h before deadline minimum |

---

## Phase Overview

| Phase | Skills | Gate Condition |
|---|---|---|
| 0. Event Parsing | `hackathon-event-parser` | Event metadata, tracks, and criteria extracted from URL |
| 1. Track Understanding | `hackathon-track-analyzer` | Constraints and evaluation axes confirmed |
| 2. Idea Development | `hackathon-problem-space` → `hackathon-idea-generator` → `hackathon-idea-scoring` | Top idea selected and committed |
| 3. Scope Definition | `hackathon-scope-cutter` → `hackathon-wow-detector` | MVP and demo flow locked |
| 4. Project Planning | `hackathon-doc-writer` → `hackathon-task-planner` | Task list with estimates ready |
| 5. Implementation | `hackathon-code-implementer` → `hackathon-test-generator` | Demo runs end-to-end |
| 6. Demo Preparation | `hackathon-demo-video` → `hackathon-pitchdeck` | Video uploaded, slides ready |
| 7. Evaluation | `hackathon-judge-simulator` | Q&A prepared, pitch refined |
| 8. Submission | `hackathon-submission-prep` | All artifacts submitted |

---

## Phase 0: Event Parsing  *(Autonomous Pipeline Entry Point)*

**Objective:** Extract all hackathon event data from a URL so downstream skills receive structured input without manual copy-paste.

**Entry Conditions:**
- A hackathon event URL is available (Devpost, DoraHacks, MLH, Hackathon.com, or any event page)
- This is the starting point of an autonomous pipeline run

**Exit Conditions:**
- `event_metadata` is populated with name, platform, and deadline
- At least one `track` is extracted with a description
- `judging_criteria` are extracted or inferred with `rubric_source` set
- `recommended_track` is identified (or multiple tracks are available for team selection)
- `next_skill` is set to `hackathon-track-analyzer`
- `extraction_confidence` is documented; any gaps listed in `extraction_warnings`

**Skills:**
- [`hackathon-event-parser`](../skills/hackathon-event-parser/SKILL.md) — Fetch and parse event page; extract tracks, criteria, sponsors, timeline, and recommended track.

**Required Inputs:**
- `event_url` — hackathon event URL
- `team_size` (optional)
- `team_skills` (optional)

**Key Outputs:**
- `event_metadata` (name, deadline, duration, platform)
- `tracks[]` with descriptions, sponsor tools, feasibility signals
- `judging_criteria[]` per track
- `sponsor_tools[]`
- `timeline[]`
- `recommended_track`

**Gate:** `extraction_confidence` is `high` or `medium`. If `low`, supplement with manual inputs before proceeding to Phase 1.

**Data Flow into Phase 1:**
```
event_parser.recommended_track.description → track_analyzer.track_description
event_parser.judging_criteria[track]        → track_analyzer.judging_rubric
event_parser.sponsor_tools                  → track_analyzer.sponsor_briefs
event_parser.event_metadata.duration_hours  → track_analyzer.hackathon_duration_hours
```

**Skip condition:** If no URL is available, skip Phase 0 and provide track description manually to `hackathon-track-analyzer`.

---

## Phase 1: Track Understanding

**Objective:** Extract the judging environment before generating any ideas.

**Entry Conditions:**
- Hackathon event URL, brief, or track description is available
- Hackathon duration in hours is known
- At least one of: track description text, sponsor brief, or judging rubric is accessible

**Exit Conditions:**
- `evaluation_axes` are extracted and confirmed
- `required_constraints` are documented
- `disqualifiers` are identified
- `recommended_skills` list is produced for Phase 2

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

**Entry Conditions:**
- Phase 1 is complete: `track_summary`, `required_constraints`, and `evaluation_axes` are available
- Team size and hackathon duration are confirmed
- Tech stack preferences or constraints are known

**Exit Conditions:**
- At least 3 candidate ideas have been generated
- All ideas have been scored against `evaluation_axes`
- One idea is selected as `top_recommendation` with documented rationale
- Team has formally committed to the selected idea — no changes after this point

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

**Entry Conditions:**
- Phase 2 is complete: `top_recommendation` is locked and the team has committed
- `core_mechanism` and `wow_factor` hypotheses are known from idea scoring
- Feature wishlist has been drafted (even informally)

**Exit Conditions:**
- `mvp_features` list is finalized — no additions after this point
- `deferred_features` are explicitly documented
- `mvp_demo_flow` is locked as the canonical demo sequence
- `primary_wow_moment` is identified and its placement in the demo is confirmed
- `time_budget` per feature is within 70% of remaining hackathon hours

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

**Entry Conditions:**
- Phase 3 is complete: `mvp_features`, `mvp_demo_flow`, and `time_budget` are locked
- Tech stack has been selected
- Team roles are defined or inferable from team composition

**Exit Conditions:**
- PRD document is drafted with success metrics
- At least one ADR exists for the primary architectural decision
- Full task list is produced with estimates, roles, and dependencies
- `critical_path` is identified
- `milestones` are set at 25%, 50%, 75%, and 90% of remaining time
- Every team member has at least one assigned task

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

**Entry Conditions:**
- Phase 4 is complete: task list with estimates and critical path is ready
- Tech stack and architecture decisions are documented
- All team members have assigned tasks and understand their dependencies
- Development environment is set up and running

**Exit Conditions:**
- All MVP features are implemented
- Full `mvp_demo_flow` runs end-to-end without crashing
- All `demo_blockers` from `hackathon-test-generator` are resolved
- `done_criteria` for every task are verified
- Demo path is frozen — no new feature changes after this gate

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
- `knowledge/hackathon-reference-architecture.md`
- `knowledge/hackathon-common-failures.md` (technical failure patterns)
- `knowledge/hackathon-mvp-strategy.md`

---

## Phase 6: Demo Preparation

**Objective:** Produce the demo video and pitch deck.

**Entry Conditions:**
- Phase 5 is complete: demo path is frozen and runs cleanly
- `primary_wow_moment` and `mvp_demo_flow` are confirmed from Phase 3
- Pitch duration (minutes) is known
- Demo recording environment is prepared (notifications off, test data loaded)

**Exit Conditions:**
- Demo video is recorded, reviewed, and uploaded to a permanent URL
- Pitch deck is complete with speaker notes for every slide
- Slide count and total speaking time verified against pitch duration limit
- `judging_alignment` confirms every `evaluation_axis` is addressed in at least one slide

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

**Entry Conditions:**
- Phase 6 is complete: pitch deck is drafted and demo video is uploaded
- `evaluation_axes` from Phase 1 are available
- At least one rehearsal of the pitch has been completed

**Exit Conditions:**
- All `high` priority `pitch_improvements` have been addressed in the deck
- Top 5 hard questions from `judge_simulator` have written answers rehearsed by the presenter
- All `objections` have documented `rebuttal_strategy` entries the team can recall under pressure
- `predicted_scores` reviewed; any axis below 3/5 has a remediation plan

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

**Entry Conditions:**
- Phase 7 is complete: pitch is hardened and all `high` priority improvements addressed
- Demo video URL is confirmed live and accessible
- Repository is public and README loads correctly
- Submission platform account is created and project page is started

**Exit Conditions:**
- `submission_quality_score ≥ 7`
- All `blocking` missing artifacts are resolved
- Submission description is published on the platform
- All platform-required fields are populated (no `[TBD]` placeholders)
- Submission confirmed at least 1 hour before the deadline

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
hackathon-event-parser  [Phase 0 — optional autonomous entry]
    └── hackathon-track-analyzer  [Phase 1 — required]
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
| 0. Event Parsing | H+0 → H+0:15 | H+0 → H+0:15 | H+0 → H+0:20 |
| 1. Track Understanding | H+0:15 → H+0:45 | H+0:15 → H+1:00 | H+0:20 → H+1:05 |
| 2. Idea Development | H+0:45 → H+1:45 | H+1:00 → H+2:15 | H+1:05 → H+2:30 |
| 3. Scope Definition | H+1:45 → H+2:15 | H+2:15 → H+3:15 | H+2:30 → H+4:15 |
| 4. Project Planning | H+2:15 → H+2:45 | H+3:15 → H+4:15 | H+4:15 → H+5:15 |
| 5. Implementation | H+2:45 → H+15 | H+4:15 → H+22 | H+5:15 → H+30 |
| 6. Demo Preparation | H+15 → H+18 | H+22 → H+26 | H+30 → H+36 |
| 7. Evaluation | H+18 → H+20 | H+26 → H+28 | H+36 → H+38 |
| 8. Submission | H+20 → H+21 | H+28 → H+30 | H+38 → H+40 |

For detailed per-phase breakdowns, see the appropriate playbook:
- [`24h-hackathon-playbook.md`](24h-hackathon-playbook.md)
- [`36h-hackathon-playbook.md`](36h-hackathon-playbook.md)
- [`48h-hackathon-playbook.md`](48h-hackathon-playbook.md)
