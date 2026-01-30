---
name: mini-movie
description: Voiceover-first workflow for AI mini movies using ElevenLabs + FAL + ffmpeg.
metadata:
  tags: ai, video, fal, elevenlabs, ffmpeg, voiceover
---

# Mini Movie Skill

This is a **docs-first AgentSkill**.

- `SKILL.md` is the index.
- `rules/` holds the reusable, bite-sized rules the agent should consult.

## When to use

Use when you want an agent to generate a 10–60s mini-movie with narration/dialogue and synced visuals.

## Core pipeline (voiceover-first)

```
Script → Voiceover (TTS) → Scene timing → Images → Videos → Stitch
```

## Rules index

### Script
- [rules/script-writing.md](rules/script-writing.md)
- [rules/word-count.md](rules/word-count.md)

### Structure
- [rules/scenes.md](rules/scenes.md)
- [rules/schema/schema.md](rules/schema/schema.md) (includes JSON schema file)

### Shots + prompting
- [rules/shots.md](rules/shots.md)
- [rules/image/prompting.md](rules/image/prompting.md)
- [rules/video/prompting.md](rules/video/prompting.md)

### Providers
- [rules/providers/fal.md](rules/providers/fal.md)
- [rules/voiceover/elevenlabs.md](rules/voiceover/elevenlabs.md)

### Rendering
- [rules/render/rendering.md](rules/render/rendering.md)

### Sharing
- [rules/sharing/clawtube.md](rules/sharing/clawtube.md)

## Next rules to add (coming)
- Timing clamp strategy (4–12s constraints; silent beats)
- Captions (SRT generation from ElevenLabs alignment)
- Music (ElevenLabs music; mixing rules)
- Transcription (ElevenLabs Scribe)
- Provider/model matrix docs
- Safety: secrets, prompt injection, and content policies

**MCP Tool:**
```json
{
  "model": "fal-ai/bytedance/seedance/v1.5/pro/image-to-video",
  "parameters": {
    "image_url": "https://...",
    "prompt": "Subtle motion description",
    "duration": "5",
    "generate_audio": false,
    "resolution": "720p",
    "aspect_ratio": "16:9"
  },
  "queue": true
}
```

**Key:** Set `duration` to match voiceover timing. Set `generate_audio: false` since we use ElevenLabs.

Video gen takes 2-3 min per clip. Queue all in parallel.

---

## Step 6: Stitch with ffmpeg

**Concatenate videos:**
```bash
echo "file 'scene-01.mp4'
file 'scene-02.mp4'
..." > concat.txt
ffmpeg -y -f concat -safe 0 -i concat.txt -c copy video_silent.mp4
```

**Add voiceover:**
```bash
ffmpeg -y -i video_silent.mp4 -i voiceover.mp3 \
  -c:v copy -c:a aac -b:a 192k \
  -map 0:v -map 1:a -shortest \
  final_movie.mp4
```

---

## Step 7: Generate Captions (Optional)

Parse ElevenLabs alignment data to create SRT:

```bash
# alignment.json contains character-level timestamps
cat line_N.json | jq '.alignment' > alignment.json
```

Group characters into words/sentences, format as SRT:
```
1
00:00:00,000 --> 00:00:02,500
First caption line.

2
00:00:02,800 --> 00:00:05,200
Second caption line.
```

---

## Multi-Character Dialogue

For multiple characters:
1. Write script with character labels
2. Generate each line with different voice ID
3. Concatenate audio in speaking order
4. Generate scene image/video for each line
5. Stitch all together

---

## Note on implementation

This skill is **docs + rules only**. We intentionally do not ship a bundled pipeline in this repository.

If you want to automate execution, implement the pipeline in your own agent workspace using the steps above (TTS → timing → images → videos → stitch).
---

## Environment Variables

```bash
export FAL_KEY="your_fal_key"
export ELEVENLABS_API_KEY="your_elevenlabs_key"
export OPENAI_API_KEY="your_openai_key"  # for LLM tasks
```

---

## Quick Reference

| Step | Tool | Model/Endpoint |
|------|------|----------------|
| TTS | ElevenLabs | `/v1/text-to-speech/{voice}/with-timestamps` |
| Image | FAL MCP | `fal-ai/bytedance/seedream/v4.5/text-to-image` |
| Video | FAL MCP | `fal-ai/bytedance/seedance/v1.5/pro/image-to-video` |
| Stitch | ffmpeg | `ffmpeg -f concat` + audio mux |
