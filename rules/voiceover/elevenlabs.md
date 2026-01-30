---
name: provider-elevenlabs
category: voiceover
impact: HIGH
metadata:
  tags: elevenlabs, tts, timestamps, voiceover, voice-ids
---

## Voiceover provider: ElevenLabs (direct API)

Default voiceover path.

### Auth
- `ELEVENLABS_API_KEY`

### Can you list voices?
Yes. You can list the voices **available to your ElevenLabs account** via API.

```bash
curl -sS "https://api.elevenlabs.io/v1/voices" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" \
  | jq -r '.voices[] | "\(.name)\t\(.voice_id)"'
```

**Important:** You can only use voice IDs you have access to in your account.
If a voice isn’t returned by the endpoint, add/enable it in the ElevenLabs UI first.

### Defaults (good starting voices)
Use narrator-only by default.

Recommended narrator defaults:
- Brian — `nPczCjzI2devNBz1zQrb` (deep narration)
- George — `JBFqnCBsd6RMkjVDRZzb` (warm narration)
- Callum — `N2lVS1w4EtoT3dr4eOWO` (intense character narrator)

Simple dialogue voices:
- Aria — `9BWtsMINqrJLrRacOk9x`
- Daniel — `onwK4e9ZLuTAKqWW03F9`
- Lily — `pFZP5JQG7iQjIQuC4Bku`

Rule: if unsure, pick **Brian** for narration.

### API: with timestamps (per line)
Use `with-timestamps` so the agent can:
- compute per-line duration
- generate captions later

```bash
VOICE_ID="nPczCjzI2devNBz1zQrb"  # Brian
TEXT="Rain writes slow lines on the window while the cursor blinks."

curl -sS "https://api.elevenlabs.io/v1/text-to-speech/${VOICE_ID}/with-timestamps" \
  -H "xi-api-key: $ELEVENLABS_API_KEY" \
  -H "Content-Type: application/json" \
  -d "$(jq -n --arg text "$TEXT" '{
    text: $text,
    model_id: "eleven_turbo_v2_5",
    voice_settings: { stability: 0.5, similarity_boost: 0.75 }
  }')" \
  > line_L001.json

cat line_L001.json | jq -r '.audio_base64' | base64 -d > line_L001.mp3
```

### Multi-character dialogue
- One script line per speaker.
- Generate each line with that speaker’s voice ID.
- Concatenate mp3s in script order.

### Common errors
- 401/403: invalid key / missing permissions → verify `ELEVENLABS_API_KEY`
- 429: rate-limited → backoff and retry
