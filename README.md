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

**Pipeline orchestration guide:** [`skills/hackathon-shared-resources/playbooks/hackathon-workflow.md`](skills/hackathon-shared-resources/playbooks/hackathon-workflow.md)

---

## Overview

**hackathon-ai-devkit** is a reusable collection of AI agent context modules (skills) and shared resources designed to support every phase of a hackathon — from track analysis through final submission.

Each skill is a self-contained specification that AI agents load to perform a focused task in the hackathon workflow.

---

## Architecture Overview

```
┌─────────────────────────────────────────────────────────┐
│                   hackathon-ai-devkit                   │
├─────────────────────────────────────────────────────────┤
│                         skills/                         │
│                                                         │
│ 26 standard skills + 1 shared resource package          │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Skills** are the core unit. Each `SKILL.md` defines a single agent capability with typed inputs, outputs, rules, and examples.

The **hackathon-shared-resources** skill is a special container package that houses the shared resources used across the entire suite:
- **Knowledge files** (`skills/hackathon-shared-resources/knowledge/`) provide reference information agents load as context — judging patterns, winning strategies, demo psychology, and tool recommendations.
- **Templates** (`skills/hackathon-shared-resources/templates/`) are structured Markdown documents agents populate with project-specific content (PRDs, ADRs, pitch decks, demo scripts).
- **Playbooks** (`skills/hackathon-shared-resources/playbooks/`) are time-boxed execution guides that sequence skills, knowledge, and templates into a full hackathon strategy.

---

## Workflow Overview

```
[URL] → Event Parsing → Team Setup & Recruiting → Track Understanding → Idea Development → Scope Definition → Risk Analysis → Project Planning
                                                                                                                               ↓
                                   Post-Mortem ← Submission ← Deployment Prep ← Evaluation ← Demo Preparation ← Build
```

| Step | Phase | Skills |
|---|---|---|
| 0 | **Event Parsing** *(autonomous entry)* | `hackathon-event-parser` |
| 0.5 | **Team Setup** | `hackathon-team-recruiter` OR `hackathon-role-allocator` |
| 1 | **Track Understanding** | `hackathon-track-analyzer` |
| 2 | **Idea Development** | `hackathon-problem-space` → `hackathon-idea-generator` → `hackathon-idea-scoring` |
| 3 | **Scope Definition** | `hackathon-scope-cutter` → `hackathon-wow-detector` |
| 4 | **Project Planning** | `hackathon-risk-analyzer` → `hackathon-doc-writer` + `hackathon-task-planner` |
| 5 | **Build** | `hackathon-repo-bootstrap` → `hackathon-git-master` → `hackathon-sponsor-integrator` → `hackathon-mock-data-generator` → `hackathon-code-implementer` (with `hackathon-milestone-monitor`) → `hackathon-test-generator` |
| 6 | **Demo Preparation** | `hackathon-demo-script` + `hackathon-demo-video` + `hackathon-pitchdeck` |
| 7 | **Evaluation** | `hackathon-judge-simulator` |
| 8 | **Deployment Prep** | `hackathon-deployment-prep` |
| 9 | **Submission** | `hackathon-submission-prep` |
| 10 | **Post-Mortem** | `hackathon-post-mortem` |

**Full orchestration guide:** [`skills/hackathon-shared-resources/playbooks/hackathon-workflow.md`](skills/hackathon-shared-resources/playbooks/hackathon-workflow.md)

---

## Repository Structure

```
hackathon-ai-devkit/
│
├── README.md
│
├── skills/                          # 26 agent skill modules
│   ├── hackathon-event-parser/      # Parse hackathon URL → structured event data
│   ├── hackathon-team-recruiter/    # Generate team recruitment posts
│   ├── hackathon-role-allocator/    # Allocate team roles based on skill profiles
│   ├── hackathon-track-analyzer/    # Analyze hackathon tracks and themes
│   ├── hackathon-problem-space/     # Map problem domains and user pain points
│   ├── hackathon-idea-generator/    # Generate project ideas
│   ├── hackathon-idea-scoring/      # Score and rank ideas
│   ├── hackathon-scope-cutter/      # Trim scope to MVP
│   ├── hackathon-wow-detector/      # Identify wow-factor moments
│   ├── hackathon-risk-analyzer/     # Identify technical and demo risks
│   ├── hackathon-doc-writer/        # Write technical documentation (PRDs, ADRs)
│   ├── hackathon-task-planner/      # Plan and sequence tasks
│   ├── hackathon-repo-bootstrap/    # Scaffold project files and templates
│   ├── hackathon-git-master/        # Rapid collaboration branch workflow & conflict handling
│   ├── hackathon-sponsor-integrator/# Scaffold minimal boilerplate for integrating sponsor APIs
│   ├── hackathon-mock-data-generator/# Generate realistic seed/mock data conforming to judging rules
│   ├── hackathon-code-implementer/  # Guide code implementation of individual tasks
│   ├── hackathon-milestone-monitor/ # Monitor progress velocity at checkpoints (25/50/75/90%)
│   ├── hackathon-test-generator/    # Generate demo-protecting test cases
│   ├── hackathon-demo-script/       # Generate time-coded demo scripts
│   ├── hackathon-demo-video/        # Script and structure demo video recordings
│   ├── hackathon-pitchdeck/         # Build pitch decks (React or traditional formats)
│   ├── hackathon-judge-simulator/   # Simulate judge evaluation and hard Q&A Qs
│   ├── hackathon-deployment-prep/   # Validate deployment targets and checklists
│   ├── hackathon-submission-prep/   # Compile and validate final submission artifacts
│   ├── hackathon-post-mortem/       # Cleanup cloud assets, scrub history credentials, and build public READMEs
│   └── hackathon-shared-resources/  # Shared resources (knowledge, templates, and playbooks) for CLI packaging
│       ├── SKILL.md                 # Package entry point for npx skills add
│       ├── knowledge/               # Domain knowledge files
│       │   ├── hackathon-winning-patterns.md
│       │   ├── hackathon-judging-criteria.md
│       │   ├── hackathon-demo-patterns.md
│       │   ├── hackathon-mvp-strategy.md
│       │   ├── hackathon-pitch-strategy.md
│       │   ├── hackathon-pitchdeck-winning-pattern.md  # Pitchdeck Rule of Three & 7-slide framework
│       │   ├── hackathon-pitchdeck-design-with-react.md # Genspark-style React slide deck design guide
│       │   ├── hackathon-submission-guidelines.md
│       │   ├── hackathon-common-failures.md    # failure pattern catalogue
│       │   ├── hackathon-demo-psychology.md    # judge psychology and demo techniques
│       │   ├── hackathon-tools.md             # rapid development tools + recommended stack
│       │   └── hackathon-reference-architecture.md  # reference stack architecture
│       │
│       ├── templates/               # Reusable document templates
│       │   ├── ADR-template.md
│       │   ├── PRD-template.md
│       │   ├── feature-spec-template.md
│       │   ├── pitchdeck-outline.md
│       │   └── demo-script-template.md
│       │
│       └── playbooks/               # Time-boxed strategy playbooks
│           ├── hackathon-workflow.md          # master skill orchestration guide
│           ├── 24h-hackathon-playbook.md
│           ├── 36h-hackathon-playbook.md
│           └── 48h-hackathon-playbook.md
```

---

## Skill Categories

### Autonomous Entry
| Skill | Purpose |
|---|---|
| `hackathon-event-parser` | Parse hackathon URL; extract tracks, criteria, timeline, sponsor tools |

### Team Setup
| Skill | Purpose |
|---|---|
| `hackathon-team-recruiter` | Generate strategic and targeted team recruitment posts |
| `hackathon-role-allocator` | Allocate team roles and responsibilities based on skill sets |

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
| `hackathon-wow-detector` | Identify and amplify the primary wow-factor moments |
| `hackathon-risk-analyzer` | Identify and mitigate technical and demo risks |
| `hackathon-doc-writer` | Author ADRs, PRDs, and technical specifications |
| `hackathon-task-planner` | Sequence work into time-boxed tasks and paths |

### Engineering
| Skill | Purpose |
|---|---|
| `hackathon-repo-bootstrap` | Scaffold workspace, repositories, and config templates |
| `hackathon-git-master` | Establish rapid trunk-based workflows and resolve merge conflicts |
| `hackathon-sponsor-integrator` | Scaffold minimal boilerplate for integrating sponsor APIs |
| `hackathon-mock-data-generator` | Populate realistic seed/mock data conforming to judging rules |
| `hackathon-code-implementer` | Direct code implementation tasks and checklist verification |
| `hackathon-milestone-monitor` | Track and analyze velocity at major completion checkpoints |
| `hackathon-test-generator` | Generate demo-protecting test cases and validations |

### Presentation & Submission
| Skill | Purpose |
|---|---|
| `hackathon-demo-script` | Generate time-coded live presentation scripts |
| `hackathon-demo-video` | Script and storyboard pre-recorded presentation videos |
| `hackathon-pitchdeck` | Design and construct pitch deck narrative and React components |
| `hackathon-judge-simulator` | Perform judge mock evaluations and Q&A hardening |
| `hackathon-deployment-prep` | Verify production builds, fallbacks, and checklists |
| `hackathon-submission-prep` | Package artifacts and optimize submission text |
| `hackathon-post-mortem` | Archive assets, purge secrets, and publish portfolio READMEs |

---

## Installation

You can install this skill pack directly into your AI coding agent (e.g. Claude Code, Cursor, Windsurf) using the **Skills CLI**:

```bash
# Add the entire hackathon devkit to your agent
npx skills add bernieweb3/hackathon-ai-devkit
```

This installs all 26 skills alongside the `hackathon-shared-resources` package, which contains all shared playbooks, knowledge, and templates.

---

## How to Use

### Load a Skill

Point your AI agent at the installed skill's `SKILL.md` as a context module:

```
skills/hackathon-track-analyzer/SKILL.md
```

Provide the required **Inputs** defined in the skill specification. The agent will produce the **Outputs** defined by the skill.

### Use a Playbook

Select the playbook matching your hackathon duration from the shared resources package:

```
skills/hackathon-shared-resources/playbooks/24h-hackathon-playbook.md
skills/hackathon-shared-resources/playbooks/36h-hackathon-playbook.md
skills/hackathon-shared-resources/playbooks/48h-hackathon-playbook.md
```

Each playbook maps skills and milestones to time blocks.

### Reference Knowledge Files

Load knowledge files as background context for your agent:

```
skills/hackathon-shared-resources/knowledge/hackathon-winning-patterns.md
skills/hackathon-shared-resources/knowledge/hackathon-judging-criteria.md
skills/hackathon-shared-resources/knowledge/hackathon-demo-patterns.md
skills/hackathon-shared-resources/knowledge/hackathon-mvp-strategy.md
skills/hackathon-shared-resources/knowledge/hackathon-pitch-strategy.md
skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-winning-pattern.md
skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-design-with-react.md
skills/hackathon-shared-resources/knowledge/hackathon-submission-guidelines.md
skills/hackathon-shared-resources/knowledge/hackathon-common-failures.md
skills/hackathon-shared-resources/knowledge/hackathon-demo-psychology.md
skills/hackathon-shared-resources/knowledge/hackathon-tools.md
skills/hackathon-shared-resources/knowledge/hackathon-reference-architecture.md
```

### Use Templates

Populate templates directly or instruct your agent to fill them:

```
skills/hackathon-shared-resources/templates/PRD-template.md
skills/hackathon-shared-resources/templates/pitchdeck-outline.md
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
┌──────────────────────────────────────────────────────────────────────────────────┐
│  AUTONOMOUS ENTRY POINT                                                          │
│  hackathon-event-parser (URL → structured event data)                            │
├──────────────────────────────────────────────────────────────────────────────────┤
│  TEAM SETUP LAYER                                                                │
│  hackathon-team-recruiter         hackathon-role-allocator                       │
├──────────────────────────────────────────────────────────────────────────────────┤
│  STRATEGY LAYER                                                                  │
│  hackathon-track-analyzer         hackathon-problem-space                        │
│  hackathon-idea-generator         hackathon-idea-scoring                         │
├──────────────────────────────────────────────────────────────────────────────────┤
│  PLANNING LAYER                                                                  │
│  hackathon-scope-cutter           hackathon-wow-detector                         │
│  hackathon-risk-analyzer          hackathon-doc-writer                           │
│  hackathon-task-planner                                                          │
├──────────────────────────────────────────────────────────────────────────────────┤
│  BUILD LAYER                                                                     │
│  hackathon-repo-bootstrap         hackathon-git-master                           │
│  hackathon-sponsor-integrator     hackathon-mock-data-generator                  │
│  hackathon-code-implementer       hackathon-milestone-monitor                    │
│  hackathon-test-generator                                                        │
├──────────────────────────────────────────────────────────────────────────────────┤
│  PRESENTATION, DEPLOYMENT & CLEANUP LAYER                                        │
│  hackathon-demo-script            hackathon-demo-video                           │
│  hackathon-pitchdeck              hackathon-judge-simulator                      │
│  hackathon-deployment-prep        hackathon-submission-prep                      │
│  hackathon-post-mortem                                                           │
└──────────────────────────────────────────────────────────────────────────────────┘
```

| Layer | Skills | Purpose |
|---|---|---|
| **Autonomous Entry** | `event-parser` | Parse hackathon URL; bootstrap pipeline without manual input |
| **Team Setup** | `team-recruiter`, `role-allocator` | Assemble teammates or assign roles based on skills |
| **Strategy** | `track-analyzer`, `problem-space`, `idea-generator`, `idea-scoring` | Understand tracks/rubrics and select a winning idea |
| **Planning** | `scope-cutter`, `wow-detector`, `risk-analyzer`, `doc-writer`, `task-planner` | Cut scope to MVP, prioritize wow factor, write PRD/ADR, sequence tasks |
| **Build** | `repo-bootstrap`, `git-master`, `sponsor-integrator`, `mock-data-generator`, `code-implementer`, `milestone-monitor`, `test-generator` | Scaffold project, establish git trunk flow, build sponsor API scaffolds, generate mock data, code features, monitor checkpoints, write tests |
| **Presentation, Deployment & Cleanup** | `demo-script`, `demo-video`, `pitchdeck`, `judge-simulator`, `deployment-prep`, `submission-prep`, `post-mortem` | Design slides/demo/video scripts, run judge simulator Q&A, build fallback, package submissions, clean cloud assets |

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

**Full stack guide:** [`skills/hackathon-shared-resources/knowledge/hackathon-tools.md`](skills/hackathon-shared-resources/knowledge/hackathon-tools.md) · [`skills/hackathon-shared-resources/knowledge/hackathon-reference-architecture.md`](skills/hackathon-shared-resources/knowledge/hackathon-reference-architecture.md)

---

## Example Usage

### Typical Workflow (Manual Agent Invocation)

1. **Parse Event URL**: Load `skills/hackathon-event-parser/SKILL.md`
   → Provide: `hackathon_url`
   → Output: `event_name`, `tracks`, `judging_criteria`, `sponsors`, `timeline`

2. **Team Setup**: Load `skills/hackathon-team-recruiter/SKILL.md` or `skills/hackathon-role-allocator/SKILL.md`
   → Output: Social recruitment post or team role allocation mapping

3. **Track Understanding**: Load `skills/hackathon-track-analyzer/SKILL.md`
   → Provide: `track_description`, `judging_rubric`, `hackathon_duration_hours`
   → Output: `evaluation_axes`, `required_constraints`, `strategic_opportunities`

4. **Map Problem Space**: Load `skills/hackathon-problem-space/SKILL.md`
   → Provide: `track_summary`, `domain`
   → Output: `problem_statement`, `solution_gaps`, `user_segments`

5. **Generate Ideas**: Load `skills/hackathon-idea-generator/SKILL.md`
   → Provide: `problem_statement`, `solution_gaps`, `track_constraints`, `team_size`
   → Output: 5 candidate ideas with risk levels and wow factors

6. **Rank Ideas**: Load `skills/hackathon-idea-scoring/SKILL.md`
   → Provide: `ideas`, `evaluation_axes`, `team_skills`
   → Output: Ranked ideas with a single recommended target idea

7. **Determine MVP & Wow Factor**: Load `skills/hackathon-scope-cutter/SKILL.md` & `skills/hackathon-wow-detector/SKILL.md`
   → Provide: Selected idea, feature wishlist, duration
   → Output: `mvp_features`, `mvp_demo_flow`, and the target primary wow-factor moment

8. **Plan & Write PRD**: Load `skills/risk-analyzer/SKILL.md`, `skills/hackathon-doc-writer/SKILL.md`, & `skills/hackathon-task-planner/SKILL.md`
   → Output: Mitigated risk log, PRD, ADRs, and a task checklist with time estimates

9. **Scaffold & Set Up Repo**: Load `skills/hackathon-repo-bootstrap/SKILL.md` and `skills/hackathon-git-master/SKILL.md`
   → Output: Repository template scaffolding and rapid collaboration branch/merge strategy

10. **Integrate Sponsors & Seed Data**: Load `skills/hackathon-sponsor-integrator/SKILL.md` and `skills/hackathon-mock-data-generator/SKILL.md`
    → Output: Minimal sponsor SDK boilerplate integration and rule-compliant demo seed data

11. **Implement and Monitor**: Load `skills/hackathon-code-implementer/SKILL.md` and `skills/hackathon-milestone-monitor/SKILL.md`
    → Output: Done-verified code files and milestone velocity/red-flag health tracking

12. **Generate Tests**: Load `skills/hackathon-test-generator/SKILL.md`
    → Output: End-to-end regression checks protecting the key demo path

13. **Script & Build Pitch**: Load `skills/hackathon-demo-script/SKILL.md`, `skills/hackathon-demo-video/SKILL.md`, and `skills/hackathon-pitchdeck/SKILL.md`
    → Output: Time-coded narration scripts, video storyboard, and interactive React/traditional slide deck

14. **Harden Pitch**: Load `skills/hackathon-judge-simulator/SKILL.md`
    → Output: Adversarial judge Q&A bank, scoring forecasts, and rebuttal templates

15. **Validate Deployment**: Load `skills/hackathon-deployment-prep/SKILL.md`
    → Output: Production build validation, fallback plan, and environment configuration

16. **Package Submission**: Load `skills/hackathon-submission-prep/SKILL.md`
    → Output: Form-ready text, video links, demo description, and submission ZIP checklist

17. **Post-Mortem**: Load `skills/hackathon-post-mortem/SKILL.md`
    → Output: Paused paid instances, git history purged of secrets, and public developer portfolio README

---

## Autonomous Agent Workflow

The devkit supports a fully autonomous pipeline triggered by a single hackathon event URL. An AI agent can execute the complete workflow — from event parsing through submission preparation — without manual input.

### Pipeline: URL → Submission

```
                         INPUT: hackathon event URL
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │  hackathon-event-parser  │
                        └────────────┬─────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-team-recruiter / role-allocator       │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │ hackathon-track-analyzer │
                        └────────────┬─────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-problem-space                         │
            │  → hackathon-idea-generator                      │
            │  → hackathon-idea-scoring (top selection)        │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-scope-cutter                          │
            │  → hackathon-wow-detector                        │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-risk-analyzer                         │
            │  → hackathon-doc-writer                          │
            │  → hackathon-task-planner                        │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-repo-bootstrap → git-master           │
            │  → sponsor-integrator → mock-data-generator      │
            │  → hackathon-code-implementer (checkpointed by   │
            │    milestone-monitor) → test-generator           │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-demo-script + demo-video + pitchdeck  │
            │  → hackathon-judge-simulator (hardening)         │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
            ┌──────────────────────────────────────────────────┐
            │  hackathon-deployment-prep                       │
            │  → hackathon-submission-prep                     │
            └────────────────────────┬─────────────────────────┘
                                     │
                                     ▼
                        ┌──────────────────────────┐
                        │  hackathon-post-mortem   │
                        └────────────┬─────────────┘
                                     │
                                     ▼
                       OUTPUT: secured submission &
                            portfolio-ready project
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

2. Load skills/hackathon-team-recruiter/SKILL.md or skills/hackathon-role-allocator/SKILL.md
   Establish team roles and generate recruitment templates.

3. Load skills/hackathon-track-analyzer/SKILL.md
   Use the parsed track data. Confirm evaluation axes and constraints.

4. Run the full pipeline in sequence:
   problem-space → idea-generator → idea-scoring →
   scope-cutter → wow-detector →
   risk-analyzer → doc-writer → task-planner →
   repo-bootstrap → git-master → sponsor-integrator → mock-data-generator →
   code-implementer (per task) → milestone-monitor (checkpoints) → test-generator →
   demo-script → demo-video → pitchdeck → judge-simulator →
   deployment-prep → submission-prep → post-mortem

5. At each phase gate, verify exit conditions before proceeding.
   Pause and request human input at Phase 2 (idea commitment gate)
   and Phase 5 (code freeze gate).

6. Output: complete hackathon project brief, task plan, pitch deck
   narrative, and submission description.
```

### Human Checkpoints

The autonomous pipeline has two recommended human-in-the-loop gates:

| Gate | Phase | Reason |
|---|---|---|
| **Idea commitment** | After Phase 2 | Human confirms the selected idea before scope is locked |
| **Code freeze approval** | After Phase 5 | Human approves demo state before pitch and submission begin |

### Skill Reference

- **Entry point:** [`hackathon-event-parser`](skills/hackathon-event-parser/SKILL.md)
- **Orchestration:** [`skills/hackathon-shared-resources/playbooks/hackathon-workflow.md`](skills/hackathon-shared-resources/playbooks/hackathon-workflow.md)
- **Architecture:** [`skills/hackathon-shared-resources/knowledge/hackathon-reference-architecture.md`](skills/hackathon-shared-resources/knowledge/hackathon-reference-architecture.md)
- **Pitch Deck Winning Pattern:** [`skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-winning-pattern.md`](skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-winning-pattern.md)
- **React Presentation Guide:** [`skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-design-with-react.md`](skills/hackathon-shared-resources/knowledge/hackathon-pitchdeck-design-with-react.md)

---
