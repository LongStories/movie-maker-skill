---
name: scenes
category: structure
impact: CRITICAL
metadata:
  tags: scenes, structure, pacing
---

## Scene rules

### 1) 1 scene = 1 intention
Each scene should do exactly one job:
- establish (where/when)
- reveal (new info)
- escalate (raise stakes)
- turn (change direction)
- resolve (payoff)

### 2) Scene count guidance
Typical ranges:
- **30s**: 5–7 scenes
- **60s**: 8–12 scenes

### 3) Scene spec (what the agent should output)
For each scene, the agent should produce:
- `durationSeconds`
- `narration` (or explicit silent)
- `visual` (what must be seen)
- `shot` (camera framing)
- `motion` (subtle motion for image-to-video)
- `audio` (if any beyond voiceover)

### 4) Explicit silent scenes
Silent scenes must be declared as such, and still need a clear visual.
Example:

```
(SILENT 2s): Cursor blinks. Rain taps the window.
```

### 5) Don’t fight model duration constraints
If the video model supports only fixed durations (e.g. 4–12s), clamp scene durations to supported values.
If you need 2s beats, merge them into a 4s scene with minimal motion.
