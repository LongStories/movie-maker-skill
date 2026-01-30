---
name: mini-movie
description: Voiceover-first workflow for AI mini movies using ElevenLabs + FAL + ffmpeg.
metadata:
  tags: ai, video, fal, elevenlabs, ffmpeg, voiceover
---

# Mini Movie Skill

Create short AI movies (10-60s) with synchronized voiceover. **Voiceover-first** ensures perfect audio-visual sync.

## When to use

Use when creating a short movie with narration or dialogue from a script.

## Pipeline (Voiceover-First)

```
1. Script → 2. Voiceover (TTS) → 3. Scene Timing → 4. Images → 5. Videos → 6. Stitch
```

**Critical:** Generate voiceover FIRST to know exact timing, then match visuals to audio.

---

## Step 1: Write Script

Break into lines/sentences. Each line = one scene.

```
NARRATOR: The old radio crackled to life after forty years of silence.
ELENA: Father? Can you hear me? I found it.
MARCUS: Elena, step away from that console.
```

---

## Step 2: Generate Voiceover with Timestamps

Use ElevenLabs TTS with timestamps endpoint. Different voice IDs for different characters.

**Common Voice IDs:**
- `N2lVS1w4EtoT3dr4eOWO` - Callum (deep male narrator)
- `EXAVITQu4vr4xnSDxMaL` - Bella (young female)
- `ErXwobaYiN019PkySvjV` - Antoni (older male)

**API Call (per line):**
```bash
curl -sS "https://api.elevenlabs.io/v1/text-to-speech/{VOICE_ID}/with-timestamps" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Your line here.",
    "model_id": "eleven_turbo_v2_5",
    "voice_settings": {"stability": 0.5, "similarity_boost": 0.75}
  }' --output line_N.json
```

**Extract audio:**
```bash
cat line_N.json | jq -r '.audio_base64' | base64 -d > audio_N.mp3
```

**Get duration:**
```bash
ffprobe -v quiet -show_format audio_N.mp3 | grep duration
```

**Concatenate all lines:**
```bash
echo "file 'audio_1.mp3'
file 'audio_2.mp3'
..." > audio_concat.txt
ffmpeg -y -f concat -safe 0 -i audio_concat.txt -c copy voiceover.mp3
```

---

## Step 3: Calculate Scene Durations

Map each line's audio duration to a video duration (Seedance supports 4-12 sec).

```
Line 1: 4.27s audio → 4s video
Line 2: 5.12s audio → 5s video
Line 3: 3.95s audio → 4s video (minimum 4s)
```

---

## Step 4: Generate Images (Seedream 4.5)

**Model:** `fal-ai/bytedance/seedream/v4.5/text-to-image`

**MCP Tool:**
```json
{
  "model": "fal-ai/bytedance/seedream/v4.5/text-to-image",
  "parameters": {
    "prompt": "Cinematic shot of [SUBJECT], [ACTION], [SETTING], 35mm film grain, 16:9",
    "image_size": "landscape_16_9",
    "seed": 100
  },
  "queue": true
}
```

**Prompt Template:**
```
Cinematic [SHOT TYPE] of [SUBJECT], [ACTION/EMOTION], [SETTING], [LIGHTING], 35mm film grain, rich contrast, [MOOD], 16:9 aspect ratio
```

Generate all images in parallel, then poll for results.

---

## Step 5: Generate Videos (Seedance v1.5 Pro)

**Model:** `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`

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
