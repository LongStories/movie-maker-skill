---
name: fal-api-queue
category: providers
impact: HIGH
metadata:
  tags: fal, api, queue, curl, seedream, seedance
---

## FAL queue API (curl reference)

Use this to make model calls without a SDK.

### Auth
- `FAL_API_KEY`

```bash
export FAL_API_KEY="..."
```

### Base URL
- `https://queue.fal.run`

### Enqueue → Poll → Result pattern

1) **Enqueue** a request (POST)
2) Use returned `status_url` until `status` becomes `COMPLETED`
3) Fetch `response_url` for final output

---

## Example: Seedream (image)

Model (edit):
- `fal-ai/bytedance/seedream/v4.5/edit`

```bash
MODEL="fal-ai/bytedance/seedream/v4.5/edit"

curl -sS "https://queue.fal.run/$MODEL" \
  -H "Content-Type: application/json" \
  -H "Authorization: Key $FAL_API_KEY" \
  -d '{
    "prompt": "Cinematic OTS of a builder at a desk, rain on window, 35mm film grain, 16:9",
    "image_urls": [],
    "image_size": "landscape_16_9",
    "num_images": 1
  }' > enqueue.json

STATUS_URL=$(jq -r '.status_url' enqueue.json)
RESPONSE_URL=$(jq -r '.response_url' enqueue.json)

# Poll
while true; do
  curl -sS "$STATUS_URL" -H "Authorization: Key $FAL_API_KEY" > status.json
  STATUS=$(jq -r '.status' status.json)
  echo "status=$STATUS"
  if [ "$STATUS" = "COMPLETED" ]; then break; fi
  if [ "$STATUS" = "FAILED" ]; then cat status.json; exit 1; fi
  sleep 2
done

# Fetch result
curl -sS "$RESPONSE_URL" -H "Authorization: Key $FAL_API_KEY" > result.json

# Grab first image URL
IMG_URL=$(jq -r '.. | .url? // empty' result.json | head -n 1)

curl -L "$IMG_URL" -o image.jpg
```

Notes:
- Result shapes vary; using `jq '.. | .url? // empty'` is a pragmatic way to find the first URL.

---

## Example: Seedance (video)

Model:
- `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`

```bash
MODEL="fal-ai/bytedance/seedance/v1.5/pro/image-to-video"

# You need an image URL (either a hosted image or from Seedream result)
IMAGE_URL="https://..."

curl -sS "https://queue.fal.run/$MODEL" \
  -H "Content-Type: application/json" \
  -H "Authorization: Key $FAL_API_KEY" \
  -d "$(jq -n --arg image_url "$IMAGE_URL" '{
    image_url: $image_url,
    prompt: "Locked OTS with a slow push-in. Rain trails down the window.",
    duration: "4",
    resolution: "720p",
    aspect_ratio: "16:9",
    generate_audio: false
  }')" > enqueue.json

STATUS_URL=$(jq -r '.status_url' enqueue.json)
RESPONSE_URL=$(jq -r '.response_url' enqueue.json)

while true; do
  curl -sS "$STATUS_URL" -H "Authorization: Key $FAL_API_KEY" > status.json
  STATUS=$(jq -r '.status' status.json)
  echo "status=$STATUS"
  if [ "$STATUS" = "COMPLETED" ]; then break; fi
  if [ "$STATUS" = "FAILED" ]; then cat status.json; exit 1; fi
  sleep 4
done

curl -sS "$RESPONSE_URL" -H "Authorization: Key $FAL_API_KEY" > result.json

VID_URL=$(jq -r '.. | .url? // empty' result.json | head -n 1)

curl -L "$VID_URL" -o shot.mp4
```

---

## Operational tips

- Prefer requesting the **minimum supported duration** and trimming down (see `rules/timing/clamp-and-trim.md`).
- Keep prompts stable across shots to reduce flicker.
- Save every JSON response (`enqueue.json`, `status.json`, `result.json`) for debugging.
