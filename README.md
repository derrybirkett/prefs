# Preferences

**Philosophy and preferences for product development.**

Part of the **Bloom** system.

## What is Prefs?

Prefs contains your personal philosophy and preferences for building products:

- **Design values** - Minimalism, colors, typography
- **Tech stack preferences** - NX, Playwright, shadcn
- **Product surfaces** - Marketing, auth, dashboard
- **Commands** - Custom workflow commands
- **Build order** - CLI → API → UI

## How It Works

Prefs is embedded in projects via `seed init` and guides:

- **grove** - Inherits design values and philosophy
- **hatch** - Overrides stack defaults where specified

## Structure

```
prefs/
├── prefs.yaml         # Main configuration
├── CLAUDE.md          # AI agent instructions
├── ARCHITECTURE.md    # System architecture
├── automation.md       # Git hooks and automation
├── hooks/             # Git hooks (pre-push, etc.)
└── product-surfaces/  # Surface patterns
```

## Bloom System

Prefs works with other Bloom pieces:

| Component | Role |
|-----------|------|
| **seed** | Bootstrap - seeds intent |
| **prefs** | Philosophy - your values |
| **hatch** | Stack - scaffolds code |
| **grove** | Agents - autonomous development |

See [Bloom README](../README.md) for full context.
