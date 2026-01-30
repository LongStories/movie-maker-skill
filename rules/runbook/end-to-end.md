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

Environment variables:
- `FAL_API_KEY`
- `ELEVENLABS_API_KEY`

Local tools:

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
RUN_DIR="./runs/$(date +%Y-%m-%d_%H-%M-%S)"
mkdir -p "$RUN_DIR"/{script,tts,image,video,edit,export}
```

The agent should end with:
- `export/movie.mp4`
- `export/manifest.json`
- `export/captions.srt` (optional)

---

## Step 0 — Write script + shotlist

Write:
- `script/script.txt` using numbered lines (`L001…`) (see `rules/schema/script-and-shotlist.md`)
- `script/shotlist.txt` with scenes (`S01…`) referencing line ranges

Then convert these into a single `manifest.json` matching `rules/schema/movie-manifest.schema.json`.

Tip: Start simple.
- 30s movie → 6 scenes
- 1 shot per scene

---

## Step 1 — Generate voiceover per line (preferred)

Preferred because it makes timing/alignment deterministic.

For each line where `kind` is `dialogue` or `voiceover`:

1) Call ElevenLabs `with-timestamps`
2) Extract mp3 audio
3) Record duration into `script.lines[].timing.durationSeconds`

See:
- `rules/voiceover/elevenlabs.md`
- `rules/timing/map-lines-to-timestamps.md`

Concatenate per-line mp3s to make a single `tts/voiceover.mp3` (optional but recommended for final mux).

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

## Step 3 — Generate images (FAL)

For each shot:
- generate an image using Seedream
- save to `image/SHxx.jpg`

See:
- `rules/providers/fal.md`
- `rules/image/prompting.md`

---

## Step 4 — Generate videos (FAL)

For each shot:
- generate video using Seedance from the image
- request `generateDurationSeconds`
- save to `video/SHxx.mp4`

If trimming is required:
- trim into `edit/SHxx.trim.mp4`

See:
- `rules/video/prompting.md`
- `rules/timing/clamp-and-trim.md`

---

## Step 5 — Stitch locally (ffmpeg)

1) Concat shot videos in timeline order.
2) Mux `tts/voiceover.mp3` as the final audio track.

See:
- `rules/render/rendering.md`

---

## Step 6 — Captions (optional)

If you have word timestamps:
- generate `export/captions.srt`

See:
- `rules/captions/srt.md`

---

## Step 7 — Export + share

Write final:
- `export/movie.mp4`
- `export/manifest.json`

Optional:
- `export/captions.srt`

Sharing target:
- clawtube upload API (see `rules/sharing/clawtube.md`).
