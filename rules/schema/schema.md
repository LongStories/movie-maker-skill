---
name: schema
category: schema
impact: CRITICAL
metadata:
  tags: schema, json, scenes, shots
---

## JSON schema (manifest)

Agents should produce a single **MiniMovieManifest** JSON document that contains:
- global info (title, duration, aspect ratio)
- provider/model selections
- scenes (each scene includes shot + prompts)

Canonical schema file:
- `rules/schema/movie-manifest.schema.json`

### Why a manifest?
It becomes the single source of truth the agent can:
- validate
- render from (ffmpeg/remotion)
- upload to clawtube

### Minimal manifest example

```json
{
  "version": "0.1.0",
  "title": "Shipped",
  "durationSeconds": 30,
  "aspectRatio": "16:9",
  "providers": {
    "tts": { "name": "elevenlabs", "model": "eleven_turbo_v2_5" },
    "image": { "name": "fal", "model": "fal-ai/bytedance/seedream/v4.5/edit" },
    "video": { "name": "fal", "model": "fal-ai/bytedance/seedance/v1.5/pro/image-to-video" }
  },
  "scenes": [
    {
      "id": 1,
      "durationSeconds": 5,
      "location": "Apartment workspace",
      "timeOfDay": "Night",
      "shot": "OTS",
      "visual": "Rain on window; builder at desk; terminal glow",
      "narration": "Rain writes slow lines on the window while the cursor blinks.",
      "motion": "Slow push-in; rain streaks moving",
      "imagePrompt": "Cinematic OTS ... 16:9",
      "videoPrompt": "Slow push-in ..."
    }
  ]
}
```

### Validation rule
Before rendering, the agent must:
- ensure total scene durations ~= target duration
- ensure each scene has both prompts
- ensure shots use the fixed vocabulary
