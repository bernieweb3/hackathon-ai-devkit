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
│ 14 SKILL.md  │ 9 reference  │ 5 document │ 4 workflow  │
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
Track → Idea → MVP → Code → Demo → Submission
```

| Step | Phase | Skills |
|---|---|---|
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
├── skills/                          # 14 agent skill modules
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
│   ├── hackathon-common-failures.md    # NEW: failure pattern catalogue
│   ├── hackathon-demo-psychology.md    # NEW: judge psychology and demo techniques
│   └── hackathon-tools.md             # NEW: rapid development tool recommendations
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

## License

MIT
