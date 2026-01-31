---
name: advanced-overrides
category: advanced
impact: MEDIUM
metadata:
  tags: advanced, providers, overrides, lipsync
---

## Advanced: provider/model overrides per scene/shot

### Default behavior
Most movies should use a single provider/model per media type (voiceover/image/video) to keep consistency and simplify debugging.

### Why overrides exist
Some shots need special handling:
- lip-sync / talking head shots may require a specialized video model
- a single "hero" shot might be worth higher quality
- fast/cheap drafts may use lower resolution or a faster model for non-critical shots

### Where overrides live
You can override at:

1) **Scene-level**: affects all shots in the scene
- `shotlist.scenes[].providers.video`

2) **Shot-level**: affects only one shot
- `shotlist.scenes[].shots[].providers.video`

Rule: shot-level overrides take precedence over scene-level overrides.

### Practical guidance
- Use overrides sparingly (<= 10% of shots)
- When using overrides, add a `notes` field in the providerRef explaining why
- Keep style prompts stable to reduce flicker
