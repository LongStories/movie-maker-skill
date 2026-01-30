---
name: schema
category: schema
impact: CRITICAL
metadata:
  tags: schema, json, script, shotlist
---

## JSON schema (manifest)

Agents should produce a single **MiniMovieManifest** JSON document that contains:

- global info (title, target duration, aspect ratio)
- provider/model selections
- **script lines** (numbered)
- **shotlist scenes** that reference script line IDs

Canonical schema file:
- `rules/schema/movie-manifest.schema.json`

### Why a manifest?
It becomes the single source of truth the agent can:
- validate
- render from (ffmpeg/remotion)
- upload to clawtube

### Key design choice: script + shotlist (separate artifacts)
- Script lines are `L001`, `L002`, …
- Shotlist scenes are `S01`, `S02`, …
- Scenes/shots reference script lines via `lineRefs`

This scales because dialogue and visuals can evolve independently.

### Advanced: per-scene / per-shot provider overrides

Defaults live in top-level `providers`, but you can override for special cases:
- `shotlist.scenes[].providers.video` (use a different video model for that scene)
- `shotlist.scenes[].shots[].providers.video` (use a different video model for that shot)

Use this for things like:
- lip-sync shots
- a single HQ hero shot
- using a faster/cheaper model for non-critical shots

### Minimal manifest example

```json
{
  "version": "0.2.0",
  "title": "Shipped",
  "targetDurationSeconds": 30,
  "aspectRatio": "16:9",
  "providers": {
    "voiceover": { "name": "elevenlabs", "model": "eleven_turbo_v2_5" },
    "image": { "name": "fal", "model": "fal-ai/bytedance/seedream/v4.5/edit" },
    "video": { "name": "fal", "model": "fal-ai/bytedance/seedance/v1.5/pro/image-to-video" }
  },
  "script": {
    "lines": [
      { "id": "L001", "kind": "voiceover", "speaker": "NARRATOR", "text": "Rain writes slow lines on the window while the cursor blinks." },
      { "id": "L002", "kind": "silent", "text": "Alex closes the hype feed." }
    ]
  },
  "shotlist": {
    "scenes": [
      {
        "id": "S01",
        "location": "Apartment workspace",
        "timeOfDay": "Night",
        "lineRefs": ["L001", "L002"],
        "shots": [
          {
            "id": "SH01",
            "shotType": "OTS",
            "visual": "Rain on window; builder at desk; terminal glow",
            "plannedDurationSeconds": 2,
            "generateDurationSeconds": 4,
            "trim": { "startSeconds": 0, "durationSeconds": 2 },
            "motion": "Slow push-in; rain streaks moving",
            "imagePrompt": "Cinematic OTS ... 16:9",
            "videoPrompt": "Locked OTS with a slow push-in..."
          }
        ]
      }
    ]
  }
}
```

### Validation rules
Before rendering, the agent must:
- ensure referenced `lineRefs` exist
- ensure planned durations sum to ~target duration
- ensure each shot has prompts and a valid shot type
- if the video model enforces duration constraints, set `generateDurationSeconds` and `trim`
