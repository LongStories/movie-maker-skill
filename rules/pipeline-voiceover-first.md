---
title: Voiceover-first pipeline
impact: CRITICAL
why: Precise timing + clean sync between narration and visuals.
tags: voiceover, timing, pipeline
---

## Voiceover-first pipeline

The entire mini-movie workflow should be **voiceover-first**:

1) Script (scene-by-scene)
2) Voiceover generation (per scene, or concatenated)
3) **Scene timing** (derive durations from audio, clamp to model constraints)
4) Image prompts
5) Video generation (match duration)
6) Stitch

### Rule
Never generate video durations before audio exists.

### Practical guidance
- If your video model supports fixed durations (e.g. 4–12s), clamp audio-derived durations to supported values.
- Prefer 5–6 scenes for a 30s output.
