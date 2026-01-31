---
name: movie-maker-skill
description: Docs-first AgentSkill for generating 10-60s mini-movies locally. Provides rules for scripts, shotlists, timing, prompting, providers, rendering (ffmpeg/remotion), and sharing to clawtube.
metadata:
  tags: ai, video, fal, elevenlabs, ffmpeg, remotion, voiceover
---

# Movie Maker Skill

This repo is **docs-first**: it ships guidance (rules + schemas), not an executable pipeline.

## What the agent should produce

1) A **numbered script** (lines `L001…`)
2) A **shotlist** divided into scenes (`S01…`), referencing script lines
3) A **manifest JSON** that combines both and is ready to render

## Start here (MUST do intake)

### Rule: ask questions unless explicitly told not to
Unless the user explicitly says *"don't ask questions"*, you MUST do an intake and WAIT for answers (or explicit confirmation of defaults) before generating media.

### Intake (copy/paste)
Ask these in one message:

1) **Length**: 30s / 45s / 60s?
2) **Aspect ratio**: 16:9 or 9:16?
3) **Voice**: narrator-only (default) or dialogue?
4) **Characters**: names + 1-2 sentence vibe each (we'll lock a canonical descriptor)
5) **Music**: none (default) or background music?
6) **Style**: pick a preset (cinematic / 3D animated / anime / 2D / photoreal)
7) **Quality mode**: Draft (480p), Standard (720p), HQ (1080p)
8) **Reference images**: any character/style refs? (if yes, ask user to paste/attach and say you'll save to `assets/refs/...`)

If the user doesn't answer, propose defaults and ask:

> "If I don't hear back, I'll do 30s, 16:9, narrator-only, 3D animated style, Draft (480p). Confirm?"

### Execute after confirmation
1) Create a fresh run dir
2) Create numbered `script.txt` + `shotlist.txt`
3) Build `manifest.json` (validate against schema)
4) Generate voiceover per line (preferred)
5) Compute timings -> planned durations -> generate durations -> trims
6) Generate images -> QA images (fix prompts and retry if needed)
7) Generate videos -> trim -> stitch locally

Full runbook: `rules/start/end-to-end.md`

## Core pipeline (voiceover-first)

```text
Script -> Voiceover timing -> Finalize shot durations -> Generate images -> Generate videos -> Render locally -> Share
```

## Rules index

Ordered discovery path: `rules/index.md`

Provider-specific guidance lives under `rules/providers/`. All other rules should remain provider-agnostic, with defaults listed in `rules/providers/defaults.md`.

### Start
- [rules/start/start-here.md](rules/start/start-here.md)
- [rules/start/requirements.md](rules/start/requirements.md)
- [rules/start/pipeline-voiceover-first.md](rules/start/pipeline-voiceover-first.md)
- [rules/start/end-to-end.md](rules/start/end-to-end.md)

### Planning (script to shotlist)
- [rules/planning/script-writing.md](rules/planning/script-writing.md)
- [rules/planning/word-count.md](rules/planning/word-count.md)
- [rules/planning/scenes.md](rules/planning/scenes.md)
- [rules/planning/shots.md](rules/planning/shots.md)
- [rules/planning/quality-and-cost.md](rules/planning/quality-and-cost.md)

### Manifest
- [rules/manifest/schema.md](rules/manifest/schema.md)
- [rules/manifest/script-and-shotlist.md](rules/manifest/script-and-shotlist.md)

### Audio (provider-agnostic)
- [rules/audio/pauses-and-roomtone.md](rules/audio/pauses-and-roomtone.md)
- [rules/audio/captions-srt.md](rules/audio/captions-srt.md)

### Visuals (provider-agnostic)
- [rules/visuals/image-prompting.md](rules/visuals/image-prompting.md)
- [rules/visuals/video-prompting.md](rules/visuals/video-prompting.md)
- [rules/visuals/style-presets.md](rules/visuals/style-presets.md)
- [rules/visuals/characters/character-descriptors.md](rules/visuals/characters/character-descriptors.md)
- [rules/visuals/characters/consistency.md](rules/visuals/characters/consistency.md)

### Timing
- [rules/timing/from-timestamps.md](rules/timing/from-timestamps.md)
- [rules/timing/map-lines-to-timestamps.md](rules/timing/map-lines-to-timestamps.md)
- [rules/timing/clamp-and-trim.md](rules/timing/clamp-and-trim.md)

### Providers
- [rules/providers/defaults.md](rules/providers/defaults.md)
- [rules/providers/fal/overview.md](rules/providers/fal/overview.md)
- [rules/providers/fal/api.md](rules/providers/fal/api.md)
- [rules/providers/elevenlabs/voiceover.md](rules/providers/elevenlabs/voiceover.md)
- [rules/providers/fal/voiceover-elevenlabs.md](rules/providers/fal/voiceover-elevenlabs.md) (makes ElevenLabs optional)
- [rules/providers/elevenlabs/transcription.md](rules/providers/elevenlabs/transcription.md)
- [rules/providers/elevenlabs/music.md](rules/providers/elevenlabs/music.md)

### Rendering
- [rules/render/rendering.md](rules/render/rendering.md)
- [rules/render/assets.md](rules/render/assets.md)

### QA + Troubleshooting
- [rules/qa/image-review.md](rules/qa/image-review.md)
- [rules/qa/video-spotcheck.md](rules/qa/video-spotcheck.md)
- [rules/troubleshooting/common-errors.md](rules/troubleshooting/common-errors.md)

### Sharing
- [rules/sharing/clawtube.md](rules/sharing/clawtube.md)

## Examples
- `examples/shipped/` (script + shotlist + manifest)

### Advanced
- [rules/advanced/overrides.md](rules/advanced/overrides.md) (per-scene/per-shot model overrides)

## Next rules to add
- Provider/model matrix docs
- Safety: secrets, prompt injection, and content policies
