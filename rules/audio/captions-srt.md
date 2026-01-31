---
name: captions-srt
category: captions
impact: HIGH
metadata:
  tags: captions, srt, timestamps
---

## Captions (SRT) rules

### Source of truth
Use **word-level timestamps** from:
- your TTS provider's word-level output (default: ElevenLabs `with-timestamps`)
- or your transcription provider's word timestamps (default: ElevenLabs Scribe)

### Grouping
Convert word timestamps into readable subtitle chunks:
- target 1–2 lines per caption
- ~30–45 characters per line
- prefer phrase boundaries (punctuation) over fixed word counts

### Timing
- A caption's start = first word start
- A caption's end = last word end
- Add small lead/lag padding only if needed (keep minimal)

### SRT format

```
1
00:00:00,000 --> 00:00:02,120
First caption line.

2
00:00:02,120 --> 00:00:04,900
Second caption line.
```

### Safety
Do not caption secrets. If the script contains sensitive strings, redact before rendering captions.
