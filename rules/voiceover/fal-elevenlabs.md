---
name: provider-fal-elevenlabs
category: voiceover
impact: MEDIUM
metadata:
  tags: fal, elevenlabs, tts, timestamps
---

## Voiceover via fal (Eleven v3)

Use this when:
- you want to avoid direct ElevenLabs credentials, or
- you want a unified provider surface (everything through fal).

### Auth
- `FAL_API_KEY`

### Model
- `fal-ai/elevenlabs/tts/eleven-v3`

This model supports:
- `voice` (defaults to "Rachel")
- `timestamps: true` to return per-word timestamps

### Queue API (curl)

```bash
MODEL="fal-ai/elevenlabs/tts/eleven-v3"
TEXT="Rain writes slow lines on the window while the cursor blinks."

curl -sS "https://queue.fal.run/$MODEL" \
  -H "Content-Type: application/json" \
  -H "Authorization: Key $FAL_API_KEY" \
  -d "$(jq -n --arg text "$TEXT" '{
    text: $text,
    voice: "Brian",
    timestamps: true
  }')" > enqueue.json

STATUS_URL=$(jq -r '.status_url' enqueue.json)
RESPONSE_URL=$(jq -r '.response_url' enqueue.json)

while true; do
  curl -sS "$STATUS_URL" -H "Authorization: Key $FAL_API_KEY" > status.json
  STATUS=$(jq -r '.status' status.json)
  echo "status=$STATUS"
  if [ "$STATUS" = "COMPLETED" ]; then break; fi
  if [ "$STATUS" = "FAILED" ]; then cat status.json; exit 1; fi
  sleep 2
done

curl -sS "$RESPONSE_URL" -H "Authorization: Key $FAL_API_KEY" > result.json

# Audio URL
AUDIO_URL=$(jq -r '.. | .url? // empty' result.json | head -n 1)

curl -L "$AUDIO_URL" -o line_L001.mp3
```

### Voice selection
The fal Eleven v3 model takes a `voice` string (e.g. "Rachel", "Aria").
Pick a narrator voice and keep it stable across the whole movie.
