---
name: provider-fal
category: providers
impact: HIGH
metadata:
  tags: fal, providers, image, video
---

## Provider: fal

Use fal for image and video generation.

### Auth
Environment variables:
- `FAL_KEY` (preferred)
- `FAL_API_KEY` (common alias)

### Models (initial)
Image:
- `fal-ai/bytedance/seedream/v4.5/edit`
- `fal-ai/bytedance/seedream/v4.5/text-to-image`

Video:
- `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`

### Notes
- Prefer **edit** variant when you need reference images for consistency.
- Keep prompts compact; include shot + subject + setting + lighting.
- Generate images first, then videos.
