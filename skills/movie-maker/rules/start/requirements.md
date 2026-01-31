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

**Minimum requirement:** `FAL_API_KEY` is required to generate images and videos. Without it, you can only produce planning artifacts (script, shotlist, manifest).

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

### ElevenLabs (voiceover + music + transcription)
Required if you want **direct** ElevenLabs voiceover, music, or Scribe transcription:

- `ELEVENLABS_API_KEY`

Used by:
- `rules/providers/elevenlabs/voiceover.md`
- `rules/providers/elevenlabs/transcription.md`
- `rules/providers/elevenlabs/music.md`

**Note:** If you use **fal Eleven v3 TTS** for voiceover, you do **not** need `ELEVENLABS_API_KEY` for narration.

### fal (image + video generation, optional voiceover)
Required if you want to generate images/videos via fal. Also supports voiceover via fal Eleven v3 TTS:

- `FAL_API_KEY`

Used by:
- `rules/providers/fal/overview.md`
- `rules/providers/fal/voiceover-elevenlabs.md`

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
