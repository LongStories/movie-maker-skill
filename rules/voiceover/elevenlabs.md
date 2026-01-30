---
name: provider-elevenlabs
category: voiceover
impact: HIGH
metadata:
  tags: elevenlabs, tts, timestamps, voiceover
---

## Voiceover provider: ElevenLabs

Default TTS provider.

### Auth
- `ELEVENLABS_API_KEY`

### Recommended pacing
- Assume ~137 WPM narration (adjust after you measure actual audio durations).

### API: with timestamps
Use the `with-timestamps` endpoint so the agent can:
- compute per-line duration
- (optionally) generate captions later

Example request:

```bash
curl -sS "https://api.elevenlabs.io/v1/text-to-speech/{VOICE_ID}/with-timestamps" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{
    "text": "Your line here.",
    "model_id": "eleven_turbo_v2_5",
    "voice_settings": {"stability": 0.5, "similarity_boost": 0.75}
  }' \
  --output line_01.json
```

Extract audio:

```bash
cat line_01.json | jq -r '.audio_base64' | base64 -d > line_01.mp3
```

### Multi-character dialogue
- One line per speaker.
- Generate each line with the speaker’s voice.
- Concatenate mp3s in script order.
