# Bloom System Architecture

**System for AI-assisted product development: from intent to deployed app.**

---

## The Problem

Building products involves multiple concerns:

| Concern | Question | Current State |
|---------|----------|---------------|
| **Intent** | What are we building? | User stories, PRDs |
| **Governance** | How do we align? | Mission, agents, decisions |
| **Preferences** | How do we like to build? | Scattered in memory |
| **Code** | The actual implementation | Scaffolding, patterns |
| **Deployment** | Getting it live | CI/CD, infrastructure |

These are usually disconnected. Bloom connects them.

---

## The Components

### 1. prefs (Submodule)

**What**: Declarative configuration for how you like to build.

**Why**: Personal/team preferences shouldn't be re-explained. Store once, use everywhere.

**Important**: prefs is a separate repo used as a **git submodule**. This allows:
- Independent versioning and development
- Easy updates via `git submodule update`
- Users can have their own prefs repo
- Seed is agnostic to specific preferences

**Contents** (prefs.yaml):
```yaml
# How to build
techStack:
  monorepo: NX
  testing: Playwright
  ui: shadcn
  buildOrder: [cli, api, ui]
  containerization: docker

# How it looks
design:
  ethos: minimalism
  colors: { primary: black, background: white }
   
# How to work
commands:
  wrap up: commit + tag + push + end session

# What surfaces to include
productSurfaces:
  default: [marketing, auth, dashboard]
  optional: [blog, docs]
```

**Reads**: seed (when generating), hatch (when scaffolding)
**Writes**: You (or your AI assistant)

---

### 2. grove (Submodule)

**What**: Lightweight agentic development layer - autonomous agents build the product.

**Why**: Agents need mission, roles, and patterns to work autonomously. Grove is the simplified, pragmatic version focused on Product/UX/Dev instead of C-suite complexity.

**Role in the flow**:
```
Idea → Seed (bootstrap) → Hatch (scaffold) → grove (autonomous development)
```

**Key features**:
- **Mission** - guides agents on what matters
- **Product/UX/Dev model** - assigns responsibilities to agents (simpler than C-suite)
- **Roadmap** - agents pick up work autonomously
- **Patterns** - how agents collaborate
- **Activity log** - track what agents did and why

**Important**: grove is a separate repo used as a **git submodule**. This allows:
- Independent versioning and development
- Easy updates via `git submodule update`
- Agents get mission context at session start

**Contents**:
```
grove/                    # Submodule from github.com/derrybirkett/grove
├── mission/           # Why we exist
│   └── mission.md    # Problem, solution, vision
├── agent-manifest.md # Product/UX/Dev responsibilities
├── workflows/        # How work gets done
├── patterns/         # Collaboration patterns
└── docs/             # Activity log
```

**Reads**: seed (when generating), agents (at session start)
**Writes**: Agents (during autonomous development)

---

### 3. seed (Intent Bridge)

**What**: Bootstrapper that converts human intent into a running project.

**Why**: Translation layer between human idea and machine-ready project.

**Flow**:
```
User: "I want an AI news site with subscriptions"
         ↓
    seed init --story "..."
         ↓
    ├──→ grove (generates mission, agent manifest, workflows)
    ├──→ prefs (adds as submodule)
    └──→ hatch (applies preferences)
         ↓
    Project ready for autonomous development
```

**Commands**:
```bash
seed init --story "..." --vision "..." --metric "..."
seed init --no-llm --story "..."   # Without LLM parsing
seed init --prefs user/prefs       # Custom prefs repo
seed init --no-grove               # Skip grove
seed init --no-hatch               # Skip hatch
```

**Reads**: prefs (preferences)
**Writes**: grove (governance), apps/ (code)

---

### 4. hatch (Submodule)

**What**: Executable scaffolding for applications.

**Why**: Don't repeat setup. Code once, use everywhere.

**Opinionated defaults**: Hatch has strong opinions about:
- Frameworks (Next.js, NestJS)
- Language (TypeScript)
- Project structure
- Testing setup
- Docker configuration

**User overrides**: Preferences in `prefs/` take priority over Hatch's defaults.

**Priority for scaffolding**:
```
1. prefs/ preferences    ← wins if specified
2. Hatch opinions         ← fills gaps, provides defaults
3. System defaults        ← last resort
```

**Templates**:
```
hatch/                 # Submodule from github.com/derrybirkett/hatch
├── templates/
│   ├── cli/
│   ├── api/
│   ├── ui/
│   │   ├── marketing/
│   │   ├── auth/
│   │   └── dashboard/
│   └── shared/
├── docker/
└── ci/
```

**Reads**: prefs/ preferences (overrides hatch defaults)
**Writes**: apps/, libs/

---

## Integration

```
                          ┌─────────────────────────────────────┐
                          │             prefs/                   │
                          │  (SUBMODULE - your preferences)    │
                          │                                     │
                          │  github.com/derrybirkett/prefs       │
                          │  or: github.com/you/your-prefs       │
                          └──────────────┬──────────────────────┘
                                         │
                          ┌──────────────┴──────────────────────┐
                          │                 seed                 │
                          │   (agnostic tool - reads prefs)     │
                          └──────────────┬──────────────────────┘
                                         │
          ┌───────────────────────────────┼───────────────────────────────┐
          │                               │                               │
          ▼                               ▼                               ▼
┌─────────────────┐            ┌─────────────────┐            ┌─────────────────┐
│  prefs (SUB)   │            │ grove (SUBMODULE)│            │ hatch (SUBMODULE)
│                 │            │                 │            │                 │
│ - prefs.yaml    │            │  github.com/    │            │  github.com/    │
│ - CLAUDE.md     │            │  derrybirkett/  │            │  derrybirkett/  │
│                 │            │  grove          │            │  hatch          │
│ (your prefs)   │            │                 │            │                 │
└─────────────────┘            │  - mission/     │            │  - apps/        │
                               │  - agent-       │            │  - libs/        │
                               │    manifest.md  │            │  - docker/      │
                               │  - workflows/   │            │  - ci/          │
                               │  - docs/        │            └────────┬────────┘
                               └────────┬────────┘                      │
                                        │                               │
                                        └───────────────┬───────────────┘
                                                        │
                                                        ▼
                                              ┌─────────────────┐
                                              │   Generated     │
                                              │    Project      │
                                              │                 │
                                              │  prefs/        │
                                              │  grove/        │
                                              │  apps/         │
                                              └─────────────────┘
```

**Submodule Management**:
```bash
# Add all submodules (seed init does this)
git submodule add https://github.com/derrybirkett/prefs prefs
git submodule add https://github.com/derrybirkett/grove grove
git submodule add https://github.com/derrybirkett/hatch hatch

# Use your own prefs
git submodule add https://github.com/you/your-prefs prefs

# Update to latest
git submodule update --remote

# After update, commit the new versions
git add prefs grove hatch
git commit -m "chore: update submodules"
```

---

## Data Flow

### Bootstrap Phase

```
1. User writes intent
   "As an AI fan, I want a site that researches and publishes AI news..."

2. seed adds submodules
   → Adds prefs/ (from prefs repo)
   → Adds grove/ (from grove repo)
   → Adds hatch/ (from hatch repo)

3. seed reads intent
   → Parses with LLM
   → Extracts: users, problems, solutions, suggested metrics

4. seed reads prefs/prefs.yaml
   → Loads preferences: NX, Playwright, shadcn, build order...

5. seed generates grove/
   → Creates grove/mission/mission.md
   → Creates grove/agent-manifest.md
   → Creates grove/workflows/*
   → Maps features to metrics

6. seed calls hatch
   → Passes prefs/prefs.yaml preferences
   → hatch scaffolds apps/cli, apps/api, apps/ui
   → hatch applies design preferences (minimalism, colors...)
   → hatch uses user preferences where specified, falls back to own opinions
   → hatch sets up docker, CI/CD

7. Result: Project ready to develop
```

### Development Phase (Autonomous)

```
1. Agent reads grove/ at session start
   → Gets mission context
   → Understands agent responsibilities (Product/UX/Dev model)
   → Sees roadmap and priorities

2. Agent picks up work from grove/mission/

3. Agent implements features
   → Follows patterns from grove/patterns/
   → Dev agent reviews code quality
   → Product agent ensures feature alignment

4. Agent uses "wrap up" command
   → Commits, tags, pushes
   → Activity logged to grove/docs/activity-log.md

5. CI runs tests (Playwright E2E)

6. grove tracks progress toward mission metrics
```

### Full Lifecycle

```
Idea
  ↓
seed init --story "..."
  ↓
┌──────────────────────────────────────┐
│  Bootstrap                            │
│  • prefs/ (your preferences)         │
│  • grove/ (mission + agents)         │
│  • hatch/ (scaffolded code)         │
└──────────────────────────────────────┘
  ↓
Autonomous Development (grove)
  • Agents work on mission items
  • Product/UX/Dev model assigns responsibilities
  • Activity logged
  ↓
Growth
  • Metrics tracked
  • Mission updated
  • New features from vision
```

---

## Boundaries

| What | Belongs To | Why |
|------|-----------|-----|
| Intent | User | It's their idea |
| Preferences | prefs/ (submodule) | Personal/team config - overrides hatch |
| Mission/Strategy | grove/ (submodule) | Project governance |
| Code Templates | hatch/ (submodule) | Executable scaffolding - provides defaults |
| Generated Code | Project repo | Per-project output |

### Preference Override Rules

**Hatch is opinionated but prefs wins**:

| Setting | prefs/ | Hatch | Result |
|---------|--------|-------|--------|
| UI library | shadcn | shadcn | shadcn (agrees) |
| UI library | vue | shadcn | **User wins** |
| TypeScript | not specified | required | Hatch's opinion |
| Build order | cli→api→ui | not specified | User wins |
| Package manager | pnpm | npm | **User wins** |

### What Each Tool Doesn't Do

- **prefs**: Doesn't generate code or governance. Just preferences.
- **grove**: Doesn't execute or scaffold. Pure information.
- **seed**: Doesn't store preferences or templates. Just interprets.
- **hatch**: Provides defaults but respects user preferences when specified.

---

## Commands

### seed init

```bash
seed init "As an AI fan, I want..."
# Or with options
seed init --prefs derrybirkett/prefs --story "..."
seed init --no-llm --story "..."
seed init --no-grove    # Skip grove generation
seed init --no-hatch    # Skip hatch scaffolding
```

**What it does**:
1. Adds prefs/, grove/, hatch/ as submodules
2. Parses intent (LLM)
3. Generates grove/ (governance)
4. Calls hatch (code) with prefs/ preferences
5. Creates project with:
   - `prefs/` (preferences)
   - `grove/` (governance)
   - `apps/` (CLI, API, UI)
   - Docker, CI/CD configured

### seed check

```bash
seed check
```

**What it does**:
- Compares code to grove/ mission
- Flags drift
- Suggests realignment

### wrap up

```bash
wrap up
```

**What it does** (from prefs/prefs.yaml):
1. `git add -A`
2. `git commit` (conventional commits)
3. `git tag` (semver patch++)
4. `git push --tags`
5. Update activity log

---

## Rationale

### Why separate?

| Reason | Explanation |
|--------|-------------|
| **Separation of concerns** | Intent ≠ preferences ≠ governance ≠ code |
| **Reusability** | grove templates work with different preferences |
| **Flexibility** | Change hatch templates without affecting governance |
| **Clarity** | Clear what each piece does |

### Why together?

| Reason | Explanation |
|--------|-------------|
| **Cohesion** | seed orchestrates the flow |
| **Consistency** | All parts understand the same concepts |
| **Power** | LLM bridges intent → execution |

---

## File Structure

```
~/.bloom/                 # Global seed config (optional)
└── prefs-repo: default   # Default prefs repo URL

project/
├── .gitmodules          # Submodule definitions
│
├── prefs/              # Preferences (SUBMODULE → github.com/you/prefs)
│   ├── prefs.yaml      # Your preferences
│   ├── CLAUDE.md       # AI agent instructions
│   ├── hooks/          # Git hooks (pre-push, etc.)
│   └── product-surfaces/ # Surface patterns
│
├── grove/              # Governance (SUBMODULE → github.com/derrybirkett/grove)
│   ├── mission/
│   ├── agent-manifest.md
│   ├── workflows/
│   ├── patterns/
│   └── docs/
│
├── hatch/              # Code templates (SUBMODULE → github.com/derrybirkett/hatch)
│   ├── templates/
│   ├── docker/
│   └── ci/
│
├── apps/               # Generated code (by hatch)
│   ├── cli/
│   ├── api/
│   └── ui/
│
└── libs/               # Shared code
```

### Submodule Lifecycle

1. **seed init** → adds prefs/ + grove/ + hatch/ as submodules
2. **Development** → work in apps/, grove/, prefs/ normally
3. **Update prefs** → edit prefs/ and push, then `git submodule update --remote prefs`
4. **Update grove/hatch** → `git submodule update --remote`
5. **Commit** → commit submodule updates to lock versions

---

## Glossary

| Term | Definition |
|------|------------|
| **Intent** | What the user wants to build (user story format) |
| **Preferences** | How the builder likes to work (tools, patterns, aesthetics) - in prefs/ |
| **Governance** | How the team aligns (mission, agents, decisions) - in grove/ |
| **Scaffolding** | Initial code structure from templates - via hatch/ |
| **Bloom** | The integrated system (seed + prefs + grove + hatch) |
| **Submodule** | Git submodule - external repo embedded in project |
