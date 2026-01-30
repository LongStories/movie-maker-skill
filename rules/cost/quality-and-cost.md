---
name: quality-and-cost
category: cost
impact: HIGH
metadata:
  tags: cost, quality, resolution, budgeting
---

## Quality vs cost (defaults)

Agents should proactively offer a quality/cost choice **before generating media**.

### Recommended modes

**Draft (cheap)**
- Seedance: `resolution=480p`
- Keep scenes minimal (5–7 for 30s)
- Prefer shortest supported durations + trim
- Seedream: use `landscape_16_9` (avoid `auto_2K/auto_4K`)

**Standard (default)**
- Seedance: `resolution=720p`
- Seedream: `landscape_16_9`

**HQ (expensive)**
- Seedance: `resolution=1080p`
- Only when user explicitly asks

### Biggest cost levers (in order)
1) Number of shots/videos generated (most expensive step)
2) Video duration requested (generate 4s when possible, then trim)
3) Seedance resolution (480p vs 720p vs 1080p)
4) Number of images per shot (keep `num_images=1`)
5) Re-runs/iterations (avoid regenerating unless necessary)

### Rule
When uncertain, default to **Draft** for the first pass and offer an upgrade rerun at 720p/1080p.
