---
name: rendering
category: render
impact: CRITICAL
metadata:
  tags: render, ffmpeg, remotion
---

## Rendering rules (local-first)

The agent should render **locally**.

### Preferred order
1) **ffmpeg** (fastest, simplest)
2) Remotion (when you need layout/typography precision)

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

### Remotion rendering (optional)
Use Remotion when you need:
- captions with perfect typography
- complex layout or UI overlays
- deterministic visuals

Rule: the agent must fall back to ffmpeg if Remotion setup is missing.
