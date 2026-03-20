# Foundation Architecture

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

These are usually disconnected. Foundations connects them.

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

### 2. pip (Submodule)

**What**: Governance layer for a specific project.

**Why**: Teams need alignment. Mission, decision rights, patterns.

**Important**: pip is a separate, mature repo used as a **git submodule**. This allows:
- Independent versioning and development
- Easy updates via `git submodule update`
- Clear ownership of governance patterns

**Contents**:
```
pip/                    # Submodule from github.com/derrybirkett/pip
├── mission/           # Why we exist
│   └── mission.md    # Problem, solution, vision
├── ia/               # Governance structure
│   ├── agent_manifest.yml
│   └── agents/       # CEO, CTO, CPO, etc.
├── graph/            # Information architecture
│   ├── product-app.md
│   └── marketing.md
├── patterns/         # Agentic patterns
└── docs/             # Living documentation
```

**Reads**: seed (when generating)
**Writes**: seed (per-project, from intent)

---

### 3. seed (Intent Bridge)

**What**: LLM-powered interpreter that reads intent + preferences → generates governance + scaffolds code.

**Why**: Translation layer between human intent and machine execution.

**Flow**:
```
User: "I want an AI news site with subscriptions"
         ↓
    seed parses intent
    (extracts users, problems, solutions, metrics)
         ↓
    ├──→ pip (generates mission, agents, graph)
    └──→ hatch (applies seed.config preferences)
```

**Reads**: seed.config, pip (as templates)
**Writes**: pip (per-project), hatch config

---

### 4. hatch (Submodule)

**What**: Executable scaffolding for applications.

**Why**: Don't repeat setup. Code once, use everywhere.

**Important**: hatch is a separate, mature repo used as a **git submodule**. This allows:
- Independent versioning and development
- Easy updates via `git submodule update`
- Clear ownership of code templates

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

**Reads**: seed.config (preferences)
**Writes**: apps/, libs/

---

## Integration

```
                         ┌─────────────────────────────────────┐
                         │             .prefs/                  │
                         │  (SUBMODULE - your preferences)    │
                         │                                     │
                         │  github.com/derrybirkett/prefs       │
                         │  or: github.com/you/your-prefs       │
                         └──────────────┬──────────────────────┘
                                        │
                         ┌──────────────┴──────────────────────┐
                         │                 seed                 │
                         │   (agnostic tool - reads prefs)      │
                         └──────────────┬──────────────────────┘
                                        │
         ┌───────────────────────────────┼───────────────────────────────┐
         │                               │                               │
         ▼                               ▼                               ▼
┌─────────────────┐            ┌─────────────────┐            ┌─────────────────┐
│  prefs (SUB)   │            │  pip (SUBMODULE)│            │ hatch (SUBMODULE)
│                 │            │                 │            │                 │
│ - prefs.yaml    │            │  github.com/    │            │  github.com/    │
│ - CLAUDE.md     │            │  derrybirkett/  │            │  derrybirkett/  │
│                 │            │  pip            │            │  hatch          │
│ (your prefs)    │            │                 │            │                 │
└─────────────────┘            │  - mission.md    │            │  - apps/        │
                               │  - agents/      │            │  - libs/        │
                               │  - graph/       │            │  - docker/      │
                               │  - patterns/    │            │  - ci/          │
                               └────────┬────────┘            └────────┬────────┘
                                        │                               │
                                        └───────────────┬───────────────┘
                                                        │
                                                        ▼
                                              ┌─────────────────┐
                                              │   Generated     │
                                              │    Project      │
                                              │                 │
                                              │  .prefs/        │
                                              │  .pip/          │
                                              │  hatch/         │
                                              │  apps/          │
                                              └─────────────────┘
```

**Submodule Management**:
```bash
# Add all submodules (seed init does this)
git submodule add https://github.com/derrybirkett/prefs .prefs
git submodule add https://github.com/derrybirkett/pip .pip
git submodule add https://github.com/derrybirkett/hatch hatch

# Use your own prefs
git submodule add https://github.com/you/your-prefs .prefs

# Update to latest
git submodule update --remote

# After update, commit the new versions
git add .prefs .pip hatch
git commit -m "chore: update submodules"
```

---

## Data Flow

### Bootstrap Phase

```
1. User writes intent
   "As an AI fan, I want a site that researches and publishes AI news..."

2. seed adds submodules
   → Adds .prefs/ (from prefs repo)
   → Adds .pip/ (from pip repo)
   → Adds hatch/ (from hatch repo)

3. seed reads intent
   → Parses with LLM
   → Extracts: users, problems, solutions, suggested metrics

4. seed reads .prefs/prefs.yaml
   → Loads preferences: NX, Playwright, shadcn, build order...

5. seed generates .pip
   → Creates .pip/mission/mission.md
   → Creates .pip/ia/agents/*
   → Creates .pip/graph/*
   → Maps features to metrics

6. seed calls hatch
   → Passes .prefs/prefs.yaml preferences
   → hatch scaffolds apps/cli, apps/api, apps/ui
   → hatch applies design preferences (minimalism, colors...)
   → hatch sets up docker, CI/CD

7. Result: Project ready to develop
```

### Development Phase

```
1. Agent reads .pip/ for governance
2. Agent reads .prefs/ for preferences
3. Agent implements features
4. Agent uses "wrap up" command → commits, tags, pushes
5. CI runs tests (Playwright E2E)
6. Activity logged to .pip/docs/activity-log.md
```

---

## Boundaries

| What | Belongs To | Why |
|------|-----------|-----|
| Intent | User | It's their idea |
| Preferences | .prefs/ (submodule) | Personal/team config |
| Mission/Strategy | .pip/ (submodule) | Project governance |
| Code Templates | hatch/ (submodule) | Executable scaffolding |
| Generated Code | Project repo | Per-project output |

### What Each Tool Doesn't Do

- **prefs**: Doesn't generate code or governance. Just preferences.
- **pip**: Doesn't execute or scaffold. Pure information.
- **seed**: Doesn't store preferences or templates. Just interprets.
- **hatch**: Doesn't read intent. Just applies prefs to templates.

---

## Commands

### seed init

```bash
seed init "As an AI fan, I want..."
# Or with options
seed init --prefs derrybirkett/prefs --story "..."
```

**What it does**:
1. Adds .prefs/, .pip/, hatch/ as submodules
2. Parses intent (LLM)
3. Generates .pip/ (governance)
4. Calls hatch (code) with .prefs/ preferences
5. Creates project with:
   - `.prefs/` (preferences)
   - `.pip/` (governance)
   - `hatch/` (templates)
   - `apps/` (CLI, API, UI)
   - Docker, CI/CD configured

### seed check

```bash
seed check
```

**What it does**:
- Compares code to .pip/ mission
- Flags drift
- Suggests realignment

### wrap up

```bash
wrap up
```

**What it does** (from .prefs/prefs.yaml):
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
| **Reusability** | pip templates work with different preferences |
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
~/.foundation/              # Global seed config (optional)
└── prefs-repo: default     # Default prefs repo URL

project/
├── .gitmodules            # Submodule definitions
│
├── .prefs/               # Preferences (SUBMODULE → github.com/you/prefs)
│   ├── prefs.yaml        # Your preferences
│   ├── CLAUDE.md         # AI agent instructions
│   ├── hooks/            # Git hooks (pre-push, etc.)
│   └── product-surfaces/ # Surface patterns
│
├── .pip/                 # Governance (SUBMODULE → github.com/derrybirkett/pip)
│   ├── mission/
│   ├── ia/
│   ├── graph/
│   └── patterns/
│
├── hatch/                # Code templates (SUBMODULE → github.com/derrybirkett/hatch)
│   ├── templates/
│   ├── docker/
│   └── ci/
│
├── apps/                 # Generated code (by hatch)
│   ├── cli/
│   ├── api/
│   └── ui/
│
└── libs/                 # Shared code
```

### Submodule Lifecycle

1. **seed init** → adds .prefs/ + .pip/ + hatch/ as submodules
2. **Development** → work in apps/, .pip/, .prefs/ normally
3. **Update prefs** → edit .prefs/ and push, then `git submodule update --remote .prefs`
4. **Update pip/hatch** → `git submodule update --remote`
5. **Commit** → commit submodule updates to lock versions

---

## Roadmap

- [x] prefs as submodule structure (this update)
- [x] seed supports multiple LLM providers (OpenRouter, Anthropic, OpenAI)
- [ ] seed adds submodules during init (.prefs/, .pip/, hatch/)
- [ ] seed reads .prefs/prefs.yaml when generating
- [ ] hatch reads .prefs/prefs.yaml when scaffolding
- [ ] pip.graph derived from seed intent
- [ ] Activity log links features → metrics
- [ ] seed check validates alignment
- [ ] seed update restores alignment

---

## Glossary

| Term | Definition |
|------|------------|
| **Intent** | What the user wants to build (user story format) |
| **Preferences** | How the builder likes to work (tools, patterns, aesthetics) - in .prefs/ |
| **Governance** | How the team aligns (mission, agents, decisions) - in .pip/ |
| **Scaffolding** | Initial code structure from templates - via hatch/ |
| **Foundation** | The integrated system (seed + prefs + pip + hatch) |
| **Submodule** | Git submodule - external repo embedded in project |
