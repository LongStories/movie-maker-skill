---
name: runbook-end-to-end
category: runbook
impact: CRITICAL
metadata:
  tags: runbook, end-to-end, ffmpeg, fal, elevenlabs
---

## End-to-end runbook (docs-only)

This repo does **not** ship an executable pipeline. This runbook is the canonical sequence an agent should follow to generate a mini-movie locally.

### Preconditions (agent must verify)

#### API keys
Defaults require:
- `FAL_API_KEY` (fal)
- `ELEVENLABS_API_KEY` (optional if using direct ElevenLabs; not required if using fal Eleven v3 TTS)

If you swap providers, update this list based on `rules/providers/defaults.md`.

How to get keys:
- fal: https://fal.ai/
- ElevenLabs: https://elevenlabs.io/

#### Loading keys from `.env.local`
If keys are in a file, source them explicitly (agent shells may not persist state):

```bash
set -a
[ -f .env.local ] && source .env.local
set +a
```

Security rule: **never print full API keys**. If you need to verify they exist:

```bash
echo "FAL_API_KEY: ${FAL_API_KEY:+set}"
echo "ELEVENLABS_API_KEY: ${ELEVENLABS_API_KEY:+set}"
```

#### Local tools

```bash
command -v ffmpeg
command -v ffprobe
command -v curl
command -v jq
command -v base64
```

### Files to produce (suggested local run dir)

Create a working folder (anywhere):

```bash
RUN_DIR="$(pwd)/runs/$(date +%Y-%m-%d_%H-%M-%S)"
mkdir -p "$RUN_DIR"/{script,tts,image,video,edit,export}
```

Shell state note: many agent environments execute commands in separate shells. Prefer:

```bash
cd "$RUN_DIR" && <command>
```

or use absolute paths.

The agent should end with:
- `export/movie.mp4`
- `export/manifest.json`
- `export/captions.srt` (optional)
- `export/movie_with_music.mp4` (optional)

---

## Step 0 — Intake (MUST)

**Do not proceed** until you have either:
- user answers to the intake questions, or
- user explicitly confirms defaults.

Use the canonical intake template:
- `rules/start/start-here.md`

## Step 0 — Write script + shotlist

Write:
- `script/script.txt` using numbered lines (`L001…`) (see `rules/manifest/script-and-shotlist.md`)
- `script/shotlist.txt` with scenes (`S01…`) referencing line ranges

Then convert these into a single `manifest.json` matching `rules/manifest/movie-manifest.schema.json`.

Tip: Start simple.
- 30s movie → 6 scenes
- 1 shot per scene

---

## Step 1 — Generate voiceover per line (preferred)

Preferred because it makes timing/alignment deterministic.

Choose a provider:

**Option A — Direct ElevenLabs** (default)
- See `rules/providers/elevenlabs/voiceover.md`

**Option B — fal Eleven v3 TTS** (makes ElevenLabs optional)
- See `rules/providers/fal/voiceover-elevenlabs.md`

For each line where `kind` is `dialogue` or `voiceover`:

1) Generate audio with timestamps
2) Save `tts/line_L###.mp3`
3) Record duration into `script.lines[].timing.durationSeconds`

See:
- `rules/timing/map-lines-to-timestamps.md`

Concatenate per-line audio to make a single `tts/voiceover.mp3` (recommended for final mux).

**Important:** add pauses/room tone between voiced lines so pacing doesn’t feel rushed.
See:
- `rules/audio/pauses-and-roomtone.md`

---

## Step 2 — Finalize timing + shot durations

1) Use timestamps to compute per-line timing (start/end).
2) Compute scene duration from referenced lines + small padding.
3) Choose each shot’s `plannedDurationSeconds`.

If the video model requires durations (e.g. 4–12s):
- set `generateDurationSeconds` to a supported value
- set `trim.durationSeconds = plannedDurationSeconds` when planned < generated

See:
- `rules/timing/from-timestamps.md`
- `rules/timing/clamp-and-trim.md`

---

## Step 3 — Generate images (default provider)

For each shot:
- generate an image using the default image provider
- save to `image/SHxx.jpg`

See:
- `rules/providers/fal/overview.md`
- `rules/visuals/image-prompting.md`

### Step 3.5 — Image QA before video (recommended)

Before generating any videos, do a quick QA pass on the images.
If the agent is multimodal, it should visually inspect each image.

If an image doesn’t match the intent:
- update the manifest (`shot.imagePrompt` and/or character/style fields)
- regenerate **only** that image

See:
- `rules/qa/image-review.md`

---

## Step 4 — Generate videos (default provider)

For each shot:
- generate video using the default video provider from the image
- request `generateDurationSeconds`
- save to `video/SHxx.mp4`

If trimming is required:
- trim into `edit/SHxx.trim.mp4`

See:
- `rules/visuals/video-prompting.md`
- `rules/timing/clamp-and-trim.md`

---

## Step 5 — Stitch locally (ffmpeg)

1) Concat shot videos in timeline order.
2) Mux `tts/voiceover.mp3` as the final audio track.

See:
- `rules/render/rendering.md`

---

## Step 5.5 — Music (optional)

If the user wants background music, generate a music bed and mix it under the voiceover.

Rule: Music is **optional by default** and must be **skipped** if the user asks for silence or maximum dialogue clarity.

See:
- `rules/providers/elevenlabs/music.md`

---

## Step 6 — Captions (optional)

If you have word timestamps:
- generate `export/captions.srt`

See:
- `rules/audio/captions-srt.md`

---

## Step 7 — Export + share

Write final:
- `export/movie.mp4`
- `export/manifest.json`

Optional:
- `export/captions.srt`

Sharing target:
- clawtube upload API (see `rules/sharing/clawtube.md`).
