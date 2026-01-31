---
name: music-elevenlabs
category: music
impact: MEDIUM
metadata:
  tags: music, elevenlabs, soundtrack, optional
---

# Music (optional): ElevenLabs

Music is **optional** and should be treated as a stylistic choice.

## When to use
Use background music if it helps the intended vibe (mood, momentum, emotion).
Skip music if:
- the user wants a raw / documentary / “dry” feel
- dialogue needs maximum clarity
- the pacing needs intentional uncomfortable silence

## Requirements
- `ELEVENLABS_API_KEY` (direct ElevenLabs only)

If you want to avoid an ElevenLabs key, use fal ElevenLabs Music via `FAL_API_KEY`.

See also:
- `rules/start/requirements.md`

## Creative control / override rule
Default behavior:
- Add subtle background music at low volume.

But this rule is **ignorable** if the user asks for:
- silence
- dynamic pacing (e.g. comedic timing, interruption, breathy realism)
- any other creative direction

## Suggested workflow

1) Pick a music prompt
- Describe genre + instrumentation + tempo + mood.
- Avoid lyrics unless explicitly requested.

2) Generate a music bed (duration)
- Target the **full movie duration** (or slightly longer).

3) Mix it under voiceover
- Keep music quiet.
- Use ducking or sidechain compression if needed.

## Mixing guidance (ffmpeg)

### Baseline: quiet bed under narration
Assume you have:
- `tts/voiceover.mp3`
- `music/music.mp3`

Mix at a conservative level:

```bash
ffmpeg -y \
  -i tts/voiceover.mp3 \
  -i music/music.mp3 \
  -filter_complex "\
    [1:a]volume=0.15[m];\
    [0:a][m]amix=inputs=2:duration=first:dropout_transition=2[a]\
  " \
  -map 0:v? -map "[a]" \
  -c:v copy -c:a aac -b:a 192k \
  export/movie_with_music.mp4
```

Notes:
- `volume=0.15` is a starting point — adjust by ear.
- If your video is separate, mux the mixed audio into it.

### Optional: duck music when voice is present
This keeps narration intelligible:

```bash
ffmpeg -y \
  -i tts/voiceover.mp3 \
  -i music/music.mp3 \
  -filter_complex "\
    [1:a]volume=0.25,sidechaincompress=threshold=0.03:ratio=8:attack=50:release=300[ducked];\
    [0:a][ducked]amix=inputs=2:duration=first:dropout_transition=2[a]\
  " \
  -map 0:v? -map "[a]" \
  -c:v copy -c:a aac -b:a 192k \
  export/movie_with_music.mp4
```

## API note
Use ElevenLabs “Music” capability.
Docs: https://elevenlabs.io/docs/overview/capabilities/music

Keep provider details lightweight here; the main requirement is: generate a bed, then mix it.
