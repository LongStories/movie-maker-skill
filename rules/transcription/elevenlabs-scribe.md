---
name: transcription-elevenlabs-scribe
category: transcription
impact: MEDIUM
metadata:
  tags: transcription, elevenlabs, scribe, timestamps
---

## Transcription (ElevenLabs Scribe)

Use Scribe when you need timestamps from **external audio** (not generated via ElevenLabs TTS).

### Auth
- `ELEVENLABS_API_KEY`

### Realtime (WebSocket) API (word timestamps)
ElevenLabs provides a realtime Speech-to-Text WebSocket endpoint:

- `wss://api.elevenlabs.io/v1/speech-to-text/realtime`

In the session config you can request word timestamps (`timestamps_granularity: "word"`) and `include_timestamps: true`.

Reference: ElevenLabs docs → Speech to Text → Realtime.

### Output to use
Prefer the message type that includes word timestamps (example from docs):
- `committed_transcript_with_timestamps`

Which includes a `words` array with per-word `start`/`end` times.

### How to use in this workflow
- Use the word timestamps to compute per-line and per-scene durations.
- Generate SRT captions using the caption rules.

### Note
For generated voiceover, prefer ElevenLabs TTS `with-timestamps` instead of STT.
