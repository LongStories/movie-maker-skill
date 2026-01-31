---
name: model-presets
category: providers
impact: HIGH
metadata:
  tags: providers, presets, models, cost, quality
---

# Model presets (image + video combos)

Use presets to keep model selection simple at intake. These presets are **provider-specific** and map to fal models by default.

## Presets

### Cheap and Fast (default for speed/cost)
Image:
- `fal-ai/z-image/turbo`

Video:
- `fal-ai/bytedance/seedance/v1/pro/fast/image-to-video`

Why:
- Lower cost and faster turnaround for drafts or high volume.

### High Quality but Slower (default for quality)
Image:
- `fal-ai/bytedance/seedream/v4.5/text-to-image`
- `fal-ai/bytedance/seedream/v4.5/edit` (if using reference images)

Video:
- `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`

Why:
- Better fidelity and motion at higher cost.

## Pricing notes (fal UI)

These prices can change; confirm in the fal UI when budgeting.
For the latest rates, see `rules/providers/pricing.md`.

Image:
- Z-Image Turbo: **$0.005 per megapixel**
- Seedream 4.5 text-to-image: **$0.04 per image**
- Seedream 4.5 edit: **$0.04 per image**

Video:
- Seedance v1/pro/fast/image-to-video:
  - Each **1080p 5 second** video costs roughly **$0.243**
  - For other resolutions: **1 million video tokens = $1.0**
- Seedance v1.5/pro/image-to-video:
  - Each **720p 5 second** video **with audio** costs roughly **$0.26**
  - For other resolutions: **1 million video tokens with audio = $2.4**
  - Without audio: **1 million video tokens = $1.2**

Token formula (video):
`tokens = (height x width x FPS x duration) / 1024`

## How to use

1) Ask the user to choose a preset during intake.
2) Set `providers.image.model` and `providers.video.model` accordingly in the manifest.
3) Use quality mode (resolution) separately; presets do not change resolution.
