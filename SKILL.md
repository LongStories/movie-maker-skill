---
name: movie-maker-skill
description: Docs-first AgentSkill for generating 10–60s mini-movies locally. Provides rules for scripts, shotlists, timing, prompting, providers, rendering (ffmpeg/remotion), and sharing to clawtube.
metadata:
  tags: ai, video, fal, elevenlabs, ffmpeg, remotion, voiceover
---

# Movie Maker Skill

This repo is **docs-first**: it ships guidance (rules + schemas), not an executable pipeline.

## What the agent should produce

1) A **numbered script** (lines `L001…`)
2) A **shotlist** divided into scenes (`S01…`), referencing script lines
3) A **manifest JSON** that combines both and is ready to render

## Core pipeline (voiceover-first)

```text
Script → Voiceover timing → Finalize shot durations → Generate images → Generate videos → Render locally → Share
```

## Rules index

### Structure
- [rules/requirements.md](rules/requirements.md)
- [rules/assets.md](rules/assets.md)
- [rules/schema/schema.md](rules/schema/schema.md)
- [rules/schema/script-and-shotlist.md](rules/schema/script-and-shotlist.md)
- [rules/scenes.md](rules/scenes.md)
- [rules/shots.md](rules/shots.md)

### Script
- [rules/script-writing.md](rules/script-writing.md)
- [rules/word-count.md](rules/word-count.md)

### Timing
- [rules/timing/from-timestamps.md](rules/timing/from-timestamps.md)
- [rules/timing/map-lines-to-timestamps.md](rules/timing/map-lines-to-timestamps.md)
- [rules/timing/clamp-and-trim.md](rules/timing/clamp-and-trim.md)

### Prompting
- [rules/image/prompting.md](rules/image/prompting.md)
- [rules/video/prompting.md](rules/video/prompting.md)

### Providers
- [rules/providers/fal.md](rules/providers/fal.md)
- [rules/providers/fal-api.md](rules/providers/fal-api.md)
- [rules/voiceover/elevenlabs.md](rules/voiceover/elevenlabs.md)
- [rules/transcription/elevenlabs-scribe.md](rules/transcription/elevenlabs-scribe.md)

### Captions
- [rules/captions/srt.md](rules/captions/srt.md)

### Rendering
- [rules/render/rendering.md](rules/render/rendering.md)

### Runbook
- [rules/runbook/end-to-end.md](rules/runbook/end-to-end.md)

### Sharing
- [rules/sharing/clawtube.md](rules/sharing/clawtube.md)

## Examples
- `examples/shipped/` (script + shotlist + manifest)

## Next rules to add
- Music (ElevenLabs music + mixing)
- Provider/model matrix docs
- Safety: secrets, prompt injection, and content policies
