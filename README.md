# Movie Maker Skill

Docs-first AgentSkill for generating 10-60s mini-movies locally.

Built by LongStories.ai (Uri Riera)

## What this is

This repo helps an agent produce:

1) A numbered script (lines `L001...`)
2) A shotlist divided into scenes (`S01...`) that references script lines
3) A render-ready `manifest.json` that ties everything together

## Quick start

- Read `SKILL.md` (entrypoint)
- Follow the intake and runbook in `rules/start/`
- Use `rules/index.md` as the ordered discovery path

## What's included

- `SKILL.md` - skill entrypoint and rules index
- `rules/` - modular guidance (intake, planning, prompts, timing, providers, rendering)
- `examples/` - sample script, shotlist, and manifest

## Providers and pricing

Provider-specific docs live under `rules/providers/`. Defaults are defined in:
- `rules/providers/defaults.md`
- `rules/providers/model-presets.md`

For current pricing, see:
- `rules/providers/pricing.md`

## No pipeline

This repo does not include a runnable media pipeline. It assumes you will connect your own local tools and provider keys.
