---
name: map-lines-to-timestamps
category: timing
impact: CRITICAL
metadata:
  tags: timing, alignment, timestamps, script
---

## Mapping timestamps back to script lines (L001…)

Goal: populate `script.lines[].timing.{startSeconds,endSeconds,durationSeconds}` so shots/scenes can be timed deterministically.

### Preferred approach (recommended): **TTS per line**
If you generate voiceover **one script line at a time** using ElevenLabs `with-timestamps`, mapping is trivial and reliable.

Workflow:
1) For each speakable line (`kind: dialogue|voiceover`), call ElevenLabs `with-timestamps` using that line’s text.
2) Compute the line’s duration from:
   - the returned alignment timestamps (preferred), or
   - `ffprobe` of the per-line mp3.
3) Build a global timeline by concatenating in script order.

Rule:
- `startSeconds` for line `Lnnn` = sum of all prior line durations + optional gap.
- Default gap: `0.10s` between lines (set to `0` if you want tighter speech).

This method avoids any need for speech-to-text and is robust to transcription errors.

### Alternative approach: STT transcript → align to lines (use only if you must)
If you only have **one combined audio file** and a word-level transcript with timestamps (e.g. ElevenLabs Scribe words with `start`/`end`), align it to the script sequentially.

#### Alignment rules
- Normalize both transcript and script:
  - lowercase
  - strip punctuation
  - collapse whitespace
- Tokenize into words.
- Align **in order** (no reordering). Short movies are linear.

#### Simple greedy alignment algorithm (works surprisingly well)
For each script line in order:
1) Let `lineWords` be the tokenized words of that line.
2) From the transcript word stream, take the next `k` words where `k ≈ len(lineWords)`.
3) If the overlap is good (e.g. ≥ 80% exact match), accept.
4) Otherwise, expand the window a little (±3–8 words) and pick the best match.
5) Assign:
   - `startSeconds` = start time of first matched word
   - `endSeconds` = end time of last matched word

If alignment fails for a line:
- regenerate TTS per line instead (preferred), OR
- split the audio with silence detection/VAD and retry per segment.

### What to store
In the manifest:
- Store per-line timing relative to the start of the **full voiceover track**.
- For silent/action lines, omit timing or set to `null`.

### Padding
After alignment, you can add optional padding:
- +0.2s after a line (breath)
- but don’t exceed model constraints; shots can absorb padding.
