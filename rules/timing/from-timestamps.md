---
name: timing-from-timestamps
category: timing
impact: CRITICAL
metadata:
  tags: timing, timestamps, tts, transcript
---

## Timing from timestamps (authoritative)

Word count is only for planning. Final timing should come from **timestamps**.

### Preferred sources of timestamps
1) **TTS provider word-level timestamps** (default: ElevenLabs `with-timestamps`)  
   Best when you generated the voiceover.
2) **Transcription provider word-level timestamps** (default: ElevenLabs Scribe)  
   Best for external audio.

### Workflow
1) Generate voiceover per script line (or per scene)
2) Extract timing:
   - per-line duration
   - optional word-level times (for captions)
3) Compute scene and shot durations:
   - Scene duration = sum of referenced lines + padding
   - Shot durations should cover the spoken lines in that scene
4) Apply model constraints:
   - If the video model requires durations (e.g. 4–12s), clamp with `generateDurationSeconds`
   - If a beat is shorter (e.g. 2s), generate minimum and trim (see `clamp-and-trim.md`)

### Padding rule (default)
Add small padding for breath/room tone:
- **+0.25s after each voiced line** (default)
- and/or **+0.5s at scene boundaries**

This is the default because it prevents run-on narration.
However, it is **ignorable** if the user explicitly wants a different pacing style.

If you concatenate voiced lines without padding, narration will feel rushed.
See `rules/audio/pauses-and-roomtone.md` for a concrete implementation.

### Manifest update rule
After timestamps exist, the agent should update the manifest:
- `script.lines[].timing.durationSeconds`
- `shotlist.scenes[].plannedDurationSeconds` (optional)
- `shots[].plannedDurationSeconds` + `generateDurationSeconds` + `trim`
