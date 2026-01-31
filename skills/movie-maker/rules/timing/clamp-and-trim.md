---
name: timing-clamp-and-trim
category: timing
impact: CRITICAL
metadata:
  tags: timing, trimming, seedance, ffmpeg
---

## Timing: clamp to model constraints, then trim

### Principle
- Voiceover timing is the ground truth.
- Video generation models often require durations like **4–12s**.

When a beat is shorter (e.g. 2s), you can:
1) Generate the minimum allowed duration (e.g. 4s)
2) **Trim** the output down to the required duration

### Why trimming works
You get stable generation while still controlling the final edit.

### Trimming with ffmpeg
Trim the first N seconds:

```bash
# Keep first 2 seconds
ffmpeg -y -i shot.mp4 -t 2 -c copy shot_trimmed.mp4
```

Trim from an offset:

```bash
# Start at 1.5s and keep 2s
ffmpeg -y -i shot.mp4 -ss 1.5 -t 2 -c copy shot_trimmed.mp4
```

### Audio alignment
If you trim video, trim/match audio correspondingly, or mux voiceover after stitching.

Rule of thumb:
- Prefer generating/assembling the **final voiceover track** and muxing at the end.
- Use per-shot voice only when you need perfect local lip/timing alignment.
