# Prefs — Tombstone

`prefs` has been retired as a standalone preferences repo. Its content has been redistributed across the [bloom](https://github.com/derrybirkett/bloom) system — methodology and philosophy to [soul](https://github.com/derrybirkett/soul), tech preferences to [stack](https://github.com/derrybirkett/stack), and tooling configuration to [shulkerbox](https://github.com/derrybirkett/shulkerbox).

## Where Each Piece Went

| Was in prefs | Now lives in | Notes |
|---|---|---|
| `philosophy:` block (lean, iterative, user-centered, minimalism) | [soul](https://github.com/derrybirkett/soul) | Codified as soul.yaml principles |
| `developmentModel:` (spec-first approach) | [soul](https://github.com/derrybirkett/soul) | Methodology is a soul concern |
| `taskModel:` (vision → mission → north-star → roadmap → tasks hierarchy) | [soul](https://github.com/derrybirkett/soul) | Traceability model lives in soul |
| `design:` (monochrome palette, minimalism principles) | [stack/stack.yaml](https://github.com/derrybirkett/stack) | ui.theme block |
| `techStack:` (Nx, pnpm, Playwright, shadcn) | [stack/stack.yaml](https://github.com/derrybirkett/stack) | Canonical tech defaults |
| `product-surfaces/` | [idea/templates/product-surfaces.md](https://github.com/derrybirkett/idea) | Surfaces live in idea |
| `hooks/pre-push` | [stack/configs/git/hooks/pre-push-no-main](https://github.com/derrybirkett/stack) | Installed by bloom-init |
| `CLAUDE.md` pattern | [shulkerbox/templates/](https://github.com/derrybirkett/shulkerbox) | Per-product CLAUDE.md template |
| `automation.md` | [shulkerbox/hooks/](https://github.com/derrybirkett/shulkerbox) | Hooks are a shulkerbox concern |

## Why Prefs Was Retired

`prefs` was a single-owner flat file that accumulated everything that didn't have a better home. It worked fine as a personal scratchpad, but it couldn't be versioned, composed, or consumed by multiple products without copy-paste. Bloom's five repos each own one slice of the same content — and they can be pulled in independently as submodules.

## What Was Not Worth Salvaging

The `.piprc`-style config format and the idea of a single yaml file as the source of truth for preferences. Preferences that live in one file accumulate drift; preferences distributed into single-purpose repos each evolve at their own pace.

## Final Version

`prefs` v1.0.0, released March 2026.

## Recovery

This repository remains accessible at [github.com/derrybirkett/prefs](https://github.com/derrybirkett/prefs) for archeological reference. It receives no further updates. New products pick up the equivalent content via the bloom system.
