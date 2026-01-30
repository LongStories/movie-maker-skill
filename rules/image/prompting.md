---
name: image-prompting
category: image
impact: HIGH
metadata:
  tags: image, prompting, seedream, consistency
---

## Image prompting rules

### Always include the shot
Start prompts with the shot type (WS/MS/CU/OTS/etc.).

### Always include the “filmable nouns”
- Subject (who/what)
- Setting (where)
- Lighting (how it looks)

### Consistency recipe
To keep characters consistent across scenes:
- keep a stable character description string
- optionally supply reference images (seedream edit)

### Prompt template

```
Cinematic {SHOT} of {SUBJECT}, {ACTION/EMOTION}, {SETTING}, {LIGHTING}, 35mm film grain, rich contrast, subtle film grain, {MOOD}, 16:9
```

### Avoid
- brand names (unless required)
- overly long prompts
- contradictory style cues
