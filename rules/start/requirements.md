---
name: requirements
category: setup
impact: CRITICAL
metadata:
  tags: requirements, setup, env, ffmpeg
---

# Requirements (local use)

This repo is **docs-first**: it does not ship a runnable pipeline.
However, if you want to *execute* the workflow locally (generate media + render), you’ll need a few tools and credentials.

Defaults assume the provider mapping in `rules/providers/defaults.md`. If you swap providers (e.g., Mire), substitute the equivalent keys below.

## 0) Capability matrix (what you want to do)

| Capability | Needed | Notes |
|---|---|---|
| Write script / shotlist / manifest | none | Pure planning; no tools or keys required |
| Generate voiceover w/ timestamps | `ELEVENLABS_API_KEY` | Recommended: ElevenLabs `with-timestamps` |
| Generate images/videos | `FAL_API_KEY` | Used for Seedream + Seedance models |
| Render final mp4 locally | `ffmpeg` (+ `ffprobe`) | Baseline renderer; Remotion optional |
| Upload to clawtube | depends on clawtube | Keys/config live in the `clawtube` repo |

## 1) Local tools

### ffmpeg (required for rendering)
This workflow assumes you can stitch generated clips and mux audio.

Preflight checks:

```bash
command -v ffmpeg
command -v ffprobe
```

If missing, install ffmpeg (e.g. Homebrew):

```bash
brew install ffmpeg
```

### Node (optional)
Only required if you’re using a Node-based pipeline or Remotion.

```bash
node -v
```

## 2) Environment variables (by provider)

### ElevenLabs (voiceover + timestamps)
Required if you want TTS or Scribe transcription:

- `ELEVENLABS_API_KEY`

Used by:
- `rules/providers/elevenlabs/voiceover.md`
- `rules/providers/elevenlabs/transcription.md`

### fal (image + video generation)
Required if you want to generate images/videos via fal:

- `FAL_API_KEY`

Used by:
- `rules/providers/fal/overview.md`

## 3) What is intentionally *not* required here

- **OpenAI keys**: not required by this repo’s workflow rules.
- **Cloudflare / R2 / Supabase keys**: not required for planning or rendering locally.
  Upload configuration is owned by the `clawtube` repository.

## 4) Suggested `.env` pattern

If you’re implementing your own pipeline, keep secrets out of git.

Example:

```bash
export ELEVENLABS_API_KEY="..."
export FAL_API_KEY="..."
```

Or in a local `.env` (untracked).
