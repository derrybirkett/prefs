# Claude Code Instructions

This project contains user preferences for digital product development. Use these preferences when assisting with any product development tasks.

## Core Principles

1. **Iterative Development**: Small, incremental changes against a visionary model (lean methodology)
2. **Learning Organization**: Continuous improvement and knowledge sharing
3. **Design Thinking**: User-centered approach to problem solving
4. **End-to-End Testing**: Always validate the complete user flow

## Current Session Context (2026-03-20)

### Bloom System v2

The user is building a system called "Bloom" consisting of:

- **seed** - CLI that bootstraps projects from intent → calls prefs + grove + hatch
- **prefs** - User preferences as a git submodule (this repo)
- **grove** - Lightweight agentic layer (mission, Product/UX/Dev agents, patterns)
- **hatch** - Code templates (apps, libs, docker)

**Key decisions:**
- grove replaces pip (simpler: Product/UX/Dev vs C-suite)
- Nature metaphors: seed, hatch, grove, bloom
- Swappable components: can use supastarter/makerkit instead of hatch
- prefs overrides grove and hatch defaults

**CLI flags:**
- `--story`, `--vision`, `--metric` for intent
- `--no-llm` to skip LLM parsing
- `--prefs <url>` to specify custom prefs repo
- `--no-grove`, `--no-hatch`, `--no-prefs` to skip components

### Working Repos

- `~/Projects/labs/bloom/` - Bloom system (seed, grove, hatch, prefs as subfolders)
- `~/Projects/labs/testseed/` - Test project

### Commands

```bash
# Bootstrap project (without LLM)
seed init --no-llm --story "..." --vision "..." --metric "..."

# Skip components
seed init --no-prefs ...
seed init --no-grove ...
seed init --no-hatch ...

# Custom prefs repo
seed init --prefs user/prefs ...
```

## Standard Tech Stack

- **Monorepo**: NX workspace architecture
- **Testing**: Playwright for end-to-end tests
- **UI Components**: shadcn/ui
- **Version Control**: Git with GitHub (issues, PRs, branch-based workflow)

## Required Product Surfaces

Every product should include:
1. Marketing/landing page
2. Authentication flow
3. Dashboard with user profile dropdown and logout
4. Blog
5. Documentation site

## Development Workflow

### Git Workflow
- Always work on a branch (never commit directly to main)
- Use GitHub issues for tracking work
- Use pull requests for code review

### Automation & Documentation
- **On commit**: Update activity log
- **On push**: Update changelog
- **On release**: Create blog post

### Core User Flow
The primary validation path for any product:
1. Landing page
2. Login
3. Dashboard
4. Logout

All features should be built on top of this working foundation.

## Agent Stack Approach

Development should be conducted by a logical stack of product development agents, each responsible for specific concerns (design, implementation, testing, documentation, etc.).

Use Git repository as the source of truth to model the system state.
