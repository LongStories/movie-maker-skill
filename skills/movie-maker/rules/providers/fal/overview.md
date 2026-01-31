---
name: provider-fal
category: providers
impact: HIGH
metadata:
  tags: fal, providers, image, video, pricing
---

## Provider: fal

Use fal for image and video generation.

### Auth
Environment variable:
- `FAL_API_KEY`

### Models (initial)

Image:
- `fal-ai/bytedance/seedream/v4.5/text-to-image` (default when you have no reference images)
- `fal-ai/bytedance/seedream/v4.5/edit` (use when you DO have reference images)
- `fal-ai/z-image/turbo` (cost-efficient text-to-image for high-volume use)

Video:
- `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`
- `fal-ai/bytedance/seedance/v1/pro/fast/image-to-video` (faster, cost-efficient)

Music (via fal):
- `fal-ai/elevenlabs/music`

### Pricing (fal UI)

Prices can change; verify in the fal UI when budgeting.
For the latest rates, see `rules/providers/pricing.md`.

Image models:
- Seedream 4.5 text-to-image: **$0.04 per image**
- Seedream 4.5 edit: **$0.04 per image**
- Z-Image Turbo: **$0.005 per megapixel**

Video models:
- Seedance v1/pro/fast/image-to-video:
  - Each **1080p 5 second** video costs roughly **$0.243**
  - For other resolutions: **1 million video tokens = $1.0**
- Seedance v1.5/pro/image-to-video:
  - Each **720p 5 second** video **with audio** costs roughly **$0.26**
  - For other resolutions: **1 million video tokens with audio = $2.4**
  - Without audio: **1 million video tokens = $1.2**

Token formula (video):
`tokens = (height x width x FPS x duration) / 1024`

### When to use text-to-image vs edit

**Default:** use **text-to-image**.
- Best for starting a new movie when you don’t have reference images yet.

Use **edit** when you have reference images and want consistency:
- character sheets
- prior scene frames
- style reference images

Note: `edit` expects `image_urls` (can be empty, but it’s meant for using references).

### Notes
- Keep prompts compact; include shot + subject + setting + lighting.
- Generate images first, then videos.
