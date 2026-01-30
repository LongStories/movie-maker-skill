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
Environment variable:
- `FAL_API_KEY`

### Models (initial)

Image:
- `fal-ai/bytedance/seedream/v4.5/text-to-image` (default when you have no reference images)
- `fal-ai/bytedance/seedream/v4.5/edit` (use when you DO have reference images)

Video:
- `fal-ai/bytedance/seedance/v1.5/pro/image-to-video`

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
