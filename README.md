# Movie Maker Skill

Docs-first AgentSkill for generating 10-60s mini-movies locally.

Built by Uri Riera (LongStories.ai founder).

## What this is

This repo helps an agent produce:

1) A numbered script (lines `L001...`)
2) A shotlist divided into scenes (`S01...`) that references script lines
3) A render-ready `manifest.json` that ties everything together

## Install

Install the skill into your agent workspace:

```
npx skills add https://github.com/longstories/movie-maker-skill --skill movie-maker
```

## Quick start

- Read `skills/movie-maker/SKILL.md` (entrypoint)
- Follow the intake and runbook in `skills/movie-maker/rules/start/`
- Use `skills/movie-maker/rules/index.md` as the ordered discovery path

## What's included

- `skills/movie-maker/SKILL.md` - skill entrypoint and rules index
- `skills/movie-maker/rules/` - modular guidance (intake, planning, prompts, timing, providers, rendering)
- `skills/movie-maker/examples/` - sample script, shotlist, and manifest

## Providers and pricing

Provider-specific docs live under `skills/movie-maker/rules/providers/`. Defaults are defined in:
- `skills/movie-maker/rules/providers/defaults.md`
- `skills/movie-maker/rules/providers/model-presets.md`

For current pricing, see:
- `skills/movie-maker/rules/providers/pricing.md`

## No pipeline

This repo does not include a runnable media pipeline. It assumes you will connect your own local tools and provider keys.

---

[LongStories.ai](https://longstories.ai) - Long AI video stories and music videos of up to 15 minutes.
