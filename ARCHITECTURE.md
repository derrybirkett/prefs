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

### 1. seed.config (renamed from prefs)

**What**: Declarative configuration for how you like to build.

**Why**: Personal/team preferences shouldn't be re-explained. Store once, use everywhere.

**Contents**:
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

**Reads**: Nobody - it's consumed by hatch
**Writes**: You (or your AI assistant)

---

### 2. pip (Project Intelligence & Process)

**What**: Governance layer for a specific project.

**Why**: Teams need alignment. Mission, decision rights, patterns.

**Contents**:
```
pip/
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

### 4. hatch (Code Templates)

**What**: Executable scaffolding for applications.

**Why**: Don't repeat setup. Code once, use everywhere.

**Templates**:
```
hatch/
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
                    │           seed.config                │
                    │  (preferences: how I like to build)  │
                    └──────────────┬──────────────────────┘
                                   │
                    ┌──────────────┴──────────────────────┐
                    │                 seed                 │
                    │     (interprets intent + prefs)     │
                    └──────────────┬──────────────────────┘
                                   │
              ┌────────────────────┴────────────────────┐
              │                                         │
              ▼                                         ▼
    ┌─────────────────┐                     ┌─────────────────┐
    │      pip        │                     │     hatch       │
    │   (governance)   │                     │   (code)        │
    │                 │                     │                 │
    │  - mission.md    │                     │  - apps/         │
    │  - agents/      │                     │  - libs/         │
    │  - graph/       │                     │  - docker/       │
    │  - patterns/    │                     │  - ci/           │
    └────────┬────────┘                     └────────┬────────┘
             │                                       │
             └───────────────┬───────────────────────┘
                             │
                             ▼
                   ┌─────────────────┐
                   │   Generated     │
                   │    Project      │
                   │                 │
                   │  .pip/          │
                   │  apps/          │
                   │  libs/          │
                   │  seed.config    │
                   └─────────────────┘
```

---

## Data Flow

### Bootstrap Phase

```
1. User writes intent
   "As an AI fan, I want a site that researches and publishes AI news..."

2. seed reads intent
   → Parses with LLM
   → Extracts: users, problems, solutions, suggested metrics

3. seed reads seed.config
   → Loads preferences: NX, Playwright, shadcn, build order...

4. seed generates pip
   → Creates .pip/mission/mission.md
   → Creates .pip/ia/agents/*
   → Creates .pip/graph/*
   → Maps features to metrics

5. seed calls hatch
   → Passes seed.config preferences
   → hatch scaffolds apps/cli, apps/api, apps/ui
   → hatch applies design preferences (minimalism, colors...)
   → hatch sets up docker, CI/CD

6. Result: Project ready to develop
```

### Development Phase

```
1. Agent reads .pip/ for governance
2. Agent reads seed.config for preferences
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
| Preferences | seed.config | Personal/team config |
| Mission/Strategy | pip | Project governance |
| Code Templates | hatch | Executable scaffolding |
| Generated Code | Project repo | Per-project output |

### What Each Tool Doesn't Do

- **seed.config**: Doesn't generate code or governance. Just preferences.
- **pip**: Doesn't execute or scaffold. Pure information.
- **seed**: Doesn't store preferences or templates. Just interprets.
- **hatch**: Doesn't read intent or governance. Just applies preferences to templates.

---

## Commands

### seed init

```bash
seed init "As an AI fan, I want..."
```

**What it does**:
1. Reads seed.config
2. Parses intent (LLM)
3. Generates pip (governance)
4. Calls hatch (code)
5. Creates project with:
   - `.pip/` (governance)
   - `apps/` (CLI, API, UI)
   - `seed.config` (preferences)
   - Docker, CI/CD configured

### seed check

```bash
seed check
```

**What it does**:
- Compares code to pip mission
- Flags drift
- Suggests realignment

### wrap up

```bash
wrap up
```

**What it does** (from seed.config):
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
~/.foundation/              # Global config (optional)
├── seed.config            # Your preferences (can be project-local too)
└── templates/            # Custom overrides
    ├── pip/               # Custom mission templates
    └── hatch/             # Custom code templates

project/
├── seed.config           # Project preferences (copied from ~/.foundation or scaffolded)
├── .pip/                 # Governance (generated by seed)
│   ├── mission/
│   ├── ia/
│   ├── graph/
│   └── patterns/
├── apps/                 # Generated code (by hatch)
│   ├── cli/
│   ├── api/
│   └── ui/
└── libs/                 # Shared code
```

---

## Roadmap

- [ ] Consolidate prefs → seed.config (rename)
- [ ] seed reads seed.config when generating
- [ ] hatch reads seed.config when scaffolding
- [ ] pip.graph derived from seed intent
- [ ] Activity log links features → metrics

---

## Glossary

| Term | Definition |
|------|------------|
| **Intent** | What the user wants to build (user story format) |
| **Preferences** | How the builder likes to work (tools, patterns, aesthetics) |
| **Governance** | How the team aligns (mission, agents, decisions) |
| **Scaffolding** | Initial code structure from templates |
| **Foundation** | The integrated system (seed + pip + hatch + seed.config) |
