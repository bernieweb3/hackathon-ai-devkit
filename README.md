# hackathon-ai-devkit

A modular AI agent skill suite for accelerating hackathon development workflows.

---

## Problem Statement

Hackathons are time-compressed competitions where developers must simultaneously understand a problem space, generate viable ideas, build a working prototype, and deliver a compelling pitch — all within 24–48 hours.

Most teams fail not because their idea is weak, but because they:
- spend too long on ideation and lose implementation time
- build too many features and ship nothing cleanly
- neglect the demo and pitch until the final hour
- walk into judging unprepared for adversarial questions

**hackathon-ai-devkit** provides structured AI agent skills that eliminate these failure modes by giving teams a repeatable, agent-assisted workflow from track analysis through final submission.

---

## Autonomous Agent Mode

The fastest way to use this devkit — paste a hackathon URL and the agent runs the full pipeline.

```
Input:  hackathon event URL
        team_size: 3
        team_skills: ["Python", "React", "OpenAI API"]

Agent steps:
  1  Parse event page          → hackathon-event-parser
  2  Extract tracks & criteria → hackathon-track-analyzer
  3  Map problem space         → hackathon-problem-space
  4  Generate ideas            → hackathon-idea-generator
  5  Score and select idea     → hackathon-idea-scoring
  6  Cut scope to MVP          → hackathon-scope-cutter
  7  Identify wow moment       → hackathon-wow-detector
  8  Write PRD + task plan     → hackathon-doc-writer + hackathon-task-planner
  9  Implement prototype       → hackathon-code-implementer (×N tasks)
 10  Verify demo path          → hackathon-test-generator
 11  Build pitch + video       → hackathon-pitchdeck + hackathon-demo-video
 12  Simulate judging          → hackathon-judge-simulator
 13  Prepare submission        → hackathon-submission-prep

Output: complete project brief, task plan, pitch deck, submission package
```

**Pipeline orchestration guide:** [`playbooks/hackathon-workflow.md`](playbooks/hackathon-workflow.md)

---

## Overview

**hackathon-ai-devkit** is a reusable collection of AI agent context modules (skills), knowledge files, templates, and playbooks designed to support every phase of a hackathon — from track analysis through final submission.

Each skill is a self-contained specification that AI agents load to perform a focused task in the hackathon workflow.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                   hackathon-ai-devkit                   │
├──────────────┬──────────────┬────────────┬─────────────┤
│   skills/    │  knowledge/  │ templates/ │  playbooks/ │
│              │              │            │             │
│ 15 SKILL.md  │ 10 reference │ 5 document │ 4 workflow  │
│ agent specs  │ knowledge    │ templates  │ guides      │
│              │ files        │            │             │
└──────────────┴──────────────┴────────────┴─────────────┘
```

**Skills** are the core unit. Each `SKILL.md` defines a single agent capability with:
- Typed inputs and outputs
- Enforcement rules
- YAML output format
- Concrete example

**Knowledge files** provide reference information agents load as context — judging patterns, winning strategies, demo psychology, and tool recommendations.

**Templates** are structured Markdown documents agents populate with project-specific content (PRDs, ADRs, pitch decks, demo scripts).

**Playbooks** are time-boxed execution guides that sequence skills, knowledge, and templates into a full hackathon strategy.

---

## Workflow Overview

```
[URL] → Event Parsing → Track → Idea → MVP → Code → Demo → Submission
```

| Step | Phase | Skills |
|---|---|---|
| 0 | **Event Parsing** *(autonomous entry)* | `hackathon-event-parser` |
| 1 | **Track Understanding** | `hackathon-track-analyzer` |
| 2 | **Idea Development** | `hackathon-problem-space` → `hackathon-idea-generator` → `hackathon-idea-scoring` |
| 3 | **Scope Definition** | `hackathon-scope-cutter` → `hackathon-wow-detector` |
| 4 | **Project Planning** | `hackathon-doc-writer` → `hackathon-task-planner` |
| 5 | **Implementation** | `hackathon-code-implementer` → `hackathon-test-generator` |
| 6 | **Demo Preparation** | `hackathon-demo-video` → `hackathon-pitchdeck` |
| 7 | **Evaluation** | `hackathon-judge-simulator` |
| 8 | **Submission** | `hackathon-submission-prep` |

**Full orchestration guide:** [`playbooks/hackathon-workflow.md`](playbooks/hackathon-workflow.md)

---

## Repository Structure

```
hackathon-ai-devkit/
│
├── README.md
│
├── skills/                          # 15 agent skill modules
│   ├── hackathon-event-parser/      # NEW: Parse hackathon URL → structured event data
│   ├── hackathon-track-analyzer/    # Analyze hackathon tracks and themes
│   ├── hackathon-problem-space/     # Map problem domains and user pain points
│   ├── hackathon-idea-generator/    # Generate project ideas
│   ├── hackathon-idea-scoring/      # Score and rank ideas
│   ├── hackathon-scope-cutter/      # Trim scope to MVP
│   ├── hackathon-doc-writer/        # Write technical documentation
│   ├── hackathon-task-planner/      # Plan and sequence tasks
│   ├── hackathon-code-implementer/  # Guide code implementation
│   ├── hackathon-test-generator/    # Generate test cases
│   ├── hackathon-pitchdeck/         # Build pitch decks
│   ├── hackathon-demo-video/        # Script demo videos
│   ├── hackathon-wow-detector/      # Identify wow-factor moments
│   ├── hackathon-judge-simulator/   # Simulate judge evaluation
│   └── hackathon-submission-prep/   # Prepare final submissions
│
├── knowledge/                       # Domain knowledge files
│   ├── hackathon-winning-patterns.md
│   ├── hackathon-judging-criteria.md
│   ├── hackathon-demo-patterns.md
│   ├── hackathon-mvp-strategy.md
│   ├── hackathon-pitch-strategy.md
│   ├── hackathon-submission-guidelines.md
│   ├── hackathon-common-failures.md    # failure pattern catalogue
│   ├── hackathon-demo-psychology.md    # judge psychology and demo techniques
│   ├── hackathon-tools.md             # rapid development tools + recommended stack
│   └── hackathon-reference-architecture.md  # NEW: reference stack architecture
│
├── templates/                       # Reusable document templates
│   ├── ADR-template.md
│   ├── PRD-template.md
│   ├── feature-spec-template.md
│   ├── pitchdeck-outline.md
│   └── demo-script-template.md
│
└── playbooks/                       # Time-boxed strategy playbooks
    ├── hackathon-workflow.md          # NEW: master skill orchestration guide
    ├── 24h-hackathon-playbook.md
    ├── 36h-hackathon-playbook.md
    └── 48h-hackathon-playbook.md
```

---

## Skill Categories

### Autonomous Entry
| Skill | Purpose |
|---|---|
| `hackathon-event-parser` | Parse hackathon URL; extract tracks, criteria, timeline, sponsor tools |

### Strategy
| Skill | Purpose |
|---|---|
| `hackathon-track-analyzer` | Parse tracks, themes, and sponsor constraints |
| `hackathon-problem-space` | Identify problem domains, users, and pain points |
| `hackathon-idea-generator` | Produce candidate project ideas |
| `hackathon-idea-scoring` | Rank ideas by feasibility, impact, and novelty |

### Scope & Planning
| Skill | Purpose |
|---|---|
| `hackathon-scope-cutter` | Reduce features to a shippable MVP |
| `hackathon-doc-writer` | Author ADRs, PRDs, and specs |
| `hackathon-task-planner` | Sequence work into time-boxed sprints |

### Engineering
| Skill | Purpose |
|---|---|
| `hackathon-code-implementer` | Drive prototype implementation |
| `hackathon-test-generator` | Generate test cases and coverage plans |

### Presentation
| Skill | Purpose |
|---|---|
| `hackathon-pitchdeck` | Construct pitch deck narrative and slides |
| `hackathon-demo-video` | Script and structure demo recordings |
| `hackathon-wow-detector` | Surface the strongest wow-factor moments |
| `hackathon-judge-simulator` | Simulate judge Q&A and scoring |
| `hackathon-submission-prep` | Finalize and package submission artifacts |

---

## How to Use

### Load a Skill

Point your AI agent at the skill's `SKILL.md` as a context module:

```
skills/hackathon-track-analyzer/SKILL.md
```

Provide the required **Inputs** defined in the skill specification. The agent will produce the **Outputs** defined by the skill.

### Use a Playbook

Select the playbook matching your hackathon duration:

```
playbooks/24h-hackathon-playbook.md
playbooks/36h-hackathon-playbook.md
playbooks/48h-hackathon-playbook.md
```

Each playbook maps skills and milestones to time blocks.

### Reference Knowledge Files

Load knowledge files as background context for your agent:

```
knowledge/hackathon-winning-patterns.md
knowledge/hackathon-judging-criteria.md
knowledge/hackathon-demo-patterns.md
knowledge/hackathon-mvp-strategy.md
knowledge/hackathon-pitch-strategy.md
knowledge/hackathon-submission-guidelines.md
knowledge/hackathon-common-failures.md
knowledge/hackathon-demo-psychology.md
knowledge/hackathon-tools.md
knowledge/hackathon-reference-architecture.md
```

### Use Templates

Populate templates directly or instruct your agent to fill them:

```
templates/PRD-template.md
templates/pitchdeck-outline.md
```

---

## Design Principles

- **Single Responsibility** — each skill does exactly one thing
- **Agent-Optimized** — specifications are concise and structured for LLM consumption
- **Independent & Reusable** — skills have no hard dependencies on each other
- **Workflow-Aligned** — skills map directly to hackathon execution phases

---

## Architecture Layers

```
┌─────────────────────────────────────────────────────────────────┐
│  AUTONOMOUS ENTRY POINT                                         │
│  hackathon-event-parser  (URL → structured event data)         │
├─────────────────────────────────────────────────────────────────┤
│  STRATEGY LAYER                                                 │
│  hackathon-track-analyzer   hackathon-problem-space             │
│  hackathon-idea-generator   hackathon-idea-scoring              │
├─────────────────────────────────────────────────────────────────┤
│  PLANNING LAYER                                                 │
│  hackathon-scope-cutter     hackathon-doc-writer                │
│  hackathon-task-planner                                         │
├─────────────────────────────────────────────────────────────────┤
│  BUILD LAYER                                                    │
│  hackathon-code-implementer hackathon-test-generator            │
├─────────────────────────────────────────────────────────────────┤
│  PRESENTATION LAYER                                             │
│  hackathon-pitchdeck        hackathon-demo-video                │
│  hackathon-wow-detector     hackathon-judge-simulator           │
│  hackathon-submission-prep                                      │
└─────────────────────────────────────────────────────────────────┘
```

| Layer | Skills | Purpose |
|---|---|---|
| **Autonomous Entry** | event-parser | Parse hackathon URL; bootstrap pipeline without manual input |
| **Strategy** | track-analyzer, problem-space, idea-generator, idea-scoring | Understand the playing field; select the winning idea |
| **Planning** | scope-cutter, doc-writer, task-planner | Define exactly what to build and how to build it |
| **Build** | code-implementer, test-generator | Implement the prototype and protect the demo path |
| **Presentation** | pitchdeck, demo-video, wow-detector, judge-simulator, submission-prep | Win on stage and in the submission form |

---

## Recommended Stack

This stack is optimized for fast hackathon deployment. The full stack can be operational in under 30 minutes.

| Layer | Technology | Why |
|---|---|---|
| **Frontend** | Next.js on **Vercel** | Zero-config deploy; production URL in 2 min |
| **Backend** | FastAPI on **Render** | One-click deploy from GitHub; free always-on tier |
| **Database** | **Supabase** (Postgres) | Instant managed DB + auth + REST API |
| **AI Routing** | **Groq** / **OpenRouter** / **Nvidia NIM** | Fast, flexible LLM inference |

```
User → Vercel (Next.js) → Render (FastAPI) → Supabase
                                    ↓
                          Groq / OpenRouter / Nvidia NIM
```

- **Groq** — ultra-low latency inference; best for real-time demo UX
- **OpenRouter** — single API key routes to 200+ models; ideal when sponsor requires specific model
- **Nvidia NIM** — GPU-accelerated open-weight model inference

**Full stack guide:** [`knowledge/hackathon-tools.md`](knowledge/hackathon-tools.md) · [`knowledge/hackathon-reference-architecture.md`](knowledge/hackathon-reference-architecture.md)

---

## Example Usage

### Typical Workflow (Manual Agent Invocation)

```
1. Obtain hackathon track description and judging rubric
2. Load skills/hackathon-track-analyzer/SKILL.md
   → Provide: track_description, judging_rubric, hackathon_duration_hours
   → Output:  evaluation_axes, required_constraints, strategic_opportunities

3. Load skills/hackathon-problem-space/SKILL.md
   → Provide: track_summary (from step 2), domain
   → Output:  problem_statement, solution_gaps, user_segments

4. Load skills/hackathon-idea-generator/SKILL.md
   → Provide: problem_statement, solution_gaps, track_constraints, team_size
   → Output:  5 candidate ideas with risk levels and wow factors

5. Load skills/hackathon-idea-scoring/SKILL.md
   → Provide: ideas (from step 4), evaluation_axes (from step 2), team_skills
   → Output:  ranked ideas, top_recommendation

6. Load skills/hackathon-scope-cutter/SKILL.md
   → Provide: top idea, feature_wishlist, hackathon_duration_hours, team_size
   → Output:  mvp_features, mvp_demo_flow, time_budget (scope is now locked)

7. Load skills/hackathon-task-planner/SKILL.md + skills/hackathon-doc-writer/SKILL.md
   → Output:  task list with critical path, PRD, ADRs

8. Load skills/hackathon-code-implementer/SKILL.md (per task)
   → Output:  implementation_plan, code_scaffolds, done_criteria

9. Load skills/hackathon-test-generator/SKILL.md
   → Output:  demo-protecting test cases, manual_checks, demo_blockers

10. Load skills/hackathon-demo-video/SKILL.md + skills/hackathon-pitchdeck/SKILL.md
    → Output:  time-coded demo script, slide deck with speaker notes

11. Load skills/hackathon-judge-simulator/SKILL.md
    → Output:  hard questions + answers, objection rebuttals, pitch improvements

12. Load skills/hackathon-submission-prep/SKILL.md
    → Output:  submission description, artifact checklist, last-mile actions
```

---

## Autonomous Agent Workflow

The devkit supports a fully autonomous pipeline triggered by a single hackathon event URL. An AI agent can execute the complete workflow — from event parsing through submission preparation — without manual input.

### Pipeline: URL → Submission

```
INPUT: hackathon event URL
           │
           ▼
┌──────────────────────────┐
│  hackathon-event-parser  │  fetch & parse event page
│                          │  → tracks, criteria, timeline,
│                          │    sponsor tools, deadlines
└───────────┬──────────────┘
            │  recommended_track + judging_criteria
            ▼
┌──────────────────────────┐
│ hackathon-track-analyzer │  structure constraints &
│                          │  evaluation axes
└───────────┬──────────────┘
            │  track_summary + evaluation_axes
            ▼
┌──────────────────────────────────────────────────────┐
│  hackathon-problem-space                             │
│  → hackathon-idea-generator (3–7 ideas)              │
│  → hackathon-idea-scoring   (ranked + top pick)      │
└───────────────────────────┬──────────────────────────┘
                            │  top_recommendation
                            ▼
┌──────────────────────────────────────────────────────┐
│  hackathon-scope-cutter  (MVP features + demo flow)  │
│  → hackathon-wow-detector (primary wow moment)       │
└───────────────────────────┬──────────────────────────┘
                            │  mvp_features + mvp_demo_flow
                            ▼
┌──────────────────────────────────────────────────────┐
│  hackathon-doc-writer   (PRD + ADRs)                 │
│  hackathon-task-planner (tasks + critical path)      │
└───────────────────────────┬──────────────────────────┘
                            │  task_list + milestones
                            ▼
┌──────────────────────────────────────────────────────┐
│  hackathon-code-implementer  (per task ×N)           │
│  → hackathon-test-generator  (demo-blocking checks)  │
└───────────────────────────┬──────────────────────────┘
                            │  implementation done; demo stable
                            ▼
┌──────────────────────────────────────────────────────┐
│  hackathon-demo-video   (script + shot list)         │
│  hackathon-pitchdeck    (slides + speaker notes)     │
│  → hackathon-judge-simulator (Q&A hardening)         │
└───────────────────────────┬──────────────────────────┘
                            │  pitch ready; artifacts validated
                            ▼
┌──────────────────────────┐
│ hackathon-submission-prep│  final package + checklist
└──────────────────────────┘
            │
            ▼
OUTPUT: complete hackathon submission
```

### Agent Instructions (Minimal Prompt)

```
You are a hackathon AI agent. Use the hackathon-ai-devkit skill suite.

Input: <hackathon event URL>
Team size: <N>
Team skills: <list>
Hackathon duration: <hours>

Steps:
1. Load skills/hackathon-event-parser/SKILL.md
   Fetch and parse the event URL. Extract tracks, criteria, and timeline.

2. Load skills/hackathon-track-analyzer/SKILL.md
   Use the parsed track data. Confirm evaluation axes and constraints.

3. Run the full pipeline in sequence:
   problem-space → idea-generator → idea-scoring →
   scope-cutter → wow-detector →
   doc-writer + task-planner →
   code-implementer (per task) → test-generator →
   demo-video + pitchdeck → judge-simulator →
   submission-prep

4. At each phase gate, verify exit conditions before proceeding.
   Pause and request human input at Phase 3 (idea commitment gate)
   and Phase 5 (code freeze gate).

5. Output: complete hackathon project brief, task plan, pitch deck
   outline, and submission description.
```

### Human Checkpoints

The autonomous pipeline has two recommended human-in-the-loop gates:

| Gate | Phase | Reason |
|---|---|---|
| **Idea commitment** | After Phase 2 | Human confirms the selected idea before scope is locked |
| **Code freeze approval** | After Phase 5 | Human approves demo state before pitch and submission begin |

### Skill Reference

- **Entry point:** [`hackathon-event-parser`](skills/hackathon-event-parser/SKILL.md)
- **Orchestration:** [`playbooks/hackathon-workflow.md`](playbooks/hackathon-workflow.md)
- **Architecture:** [`knowledge/hackathon-reference-architecture.md`](knowledge/hackathon-reference-architecture.md)

---
