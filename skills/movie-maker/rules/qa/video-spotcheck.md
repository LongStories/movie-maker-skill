---
name: qa-video-spotcheck
category: qa
impact: MEDIUM
metadata:
  tags: qa, video, spotcheck
---

## QA: Spot-check generated videos (optional)

After generating videos (and trimming if needed), do a quick spot-check before stitching.

### Why
Catch:
- motion that breaks the scene
- character morphing
- major camera movement mistakes

### How
If the agent is multimodal:
- open the video or extract a few frames.

Useful commands:

```bash
# Extract 1 fps frames for a quick glance
ffmpeg -y -i shot.mp4 -vf fps=1 -frames:v 5 frames_%02d.jpg

# Quick metadata
ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 shot.mp4
```

If it fails:
- simplify `videoPrompt` (less motion)
- fix camera framing language
- regenerate at the same duration
