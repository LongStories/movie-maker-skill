---
name: rendering
category: render
impact: CRITICAL
metadata:
  tags: render, ffmpeg, remotion
---

## Rendering rules (local-first)

The agent should render **locally**.

### Choose based on what’s installed
Agents should decide based on local availability:

- If `ffmpeg` is installed → use ffmpeg (most common)
- If Remotion is installed/configured → use Remotion when you need typography/captions/layout precision

Both should be supported.

### Preconditions
The agent should check what is installed:

```bash
command -v ffmpeg
command -v ffprobe
node -v
```

If Remotion is needed:
- verify Node + package manager
- ensure Remotion project exists and can render

### ffmpeg stitching (baseline)
Concatenate scene videos:

```bash
# concat.txt
# file 'scene-01.mp4'
# file 'scene-02.mp4'

ffmpeg -y -f concat -safe 0 -i concat.txt -c copy video_silent.mp4
```

Mux voiceover:

```bash
ffmpeg -y -i video_silent.mp4 -i voiceover.mp3 \
  -c:v copy -c:a aac -b:a 192k \
  -map 0:v -map 1:a -shortest \
  movie.mp4
```

### Remotion rendering (supported)
Use Remotion when:
- you need captions with precise typography
- you need complex layout/UI overlays
- you want deterministic visuals

Rule: Remotion is **not required**. If Remotion isn’t installed/configured, fall back to ffmpeg.
