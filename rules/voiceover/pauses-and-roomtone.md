---
name: voiceover-pauses-and-roomtone
category: voiceover
impact: HIGH
metadata:
  tags: voiceover, pauses, pacing, ffmpeg
---

## Voiceover pacing: pauses + room tone (default)

If you concatenate voice lines back-to-back, the narration will feel rushed.

### Creative override (this rule is ignorable)
Skip or alter pauses if the user explicitly wants a different feel, e.g.
- rapid-fire dialogue
- intentional run-on narration
- uncomfortable silence / no room tone
- comedic timing that requires tight cuts

### Default rule
- Add **0.25s of silence after every voiced line**.
- Add an extra **0.5s pause at scene boundaries** (optional but recommended).

### Where the rule comes from
It’s the same padding rule described in `rules/timing/from-timestamps.md`, but this file makes it operational.

### Practical implementation (ffmpeg)

Use a consistent sample rate/channel layout for all audio concatenation.

Recommended:
- Convert per-line mp3 to WAV (mono, 24k) before concatenation.
- Generate silence WAV clips with `anullsrc`.

Example (in a run dir):

```bash
# Create silence files
ffmpeg -y -f lavfi -i "anullsrc=channel_layout=mono:sample_rate=24000" -t 0.25 tts/silence_025.wav
ffmpeg -y -f lavfi -i "anullsrc=channel_layout=mono:sample_rate=24000" -t 0.50 tts/silence_050.wav

# Convert each voiced line mp3 to wav
for f in tts/line_L*.mp3; do
  base=$(basename "$f" .mp3)
  ffmpeg -y -i "$f" -ac 1 -ar 24000 "tts/${base}.wav"
done

# Build a concat list in script order (example)
cat > tts/concat.txt <<'EOF'
file 'line_L001.wav'
file 'silence_025.wav'
file 'line_L003.wav'
file 'silence_025.wav'
file 'line_L004.wav'
file 'silence_025.wav'
file 'line_L005.wav'
file 'silence_050.wav'
file 'line_L006.wav'
file 'silence_025.wav'
file 'line_L007.wav'
EOF

# Concatenate
ffmpeg -y -f concat -safe 0 -i tts/concat.txt -c copy tts/voiceover.wav

# Encode to mp3 for final mux
ffmpeg -y -i tts/voiceover.wav -c:a libmp3lame -b:a 128k tts/voiceover.mp3
```

### Manifest tie-in (optional)
If you want it explicit, store a `gapAfterSeconds` per script line in your workflow.
(We don’t require it in the schema yet.)
