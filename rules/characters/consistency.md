---
name: character-consistency
category: characters
impact: HIGH
metadata:
  tags: characters, consistency, prompting
---

## Character consistency

### Goal
Minimize "character drift" across shots.

### Strategy A (preferred): character sheet + references
1) Generate a single **character sheet** image for each character.
2) Use Seedream **edit** and include that reference in `image_urls` for every shot.

### Strategy B (fallback): stable character descriptor string
If you cannot use reference images (or the model/provider doesn’t support them):
- create a single canonical descriptor per character
- paste it into **every** prompt (near the top)

Example descriptor (Alex):
"Alex: young adult, tired eyes, dark hoodie, short dark hair, faint stubble, slightly hunched posture"

Where to place it:
- first 1–2 lines of the prompt, before shot/setting detail

### Rule
Never re-describe a character differently across shots (hair, age, clothing). If you change clothing, make it an explicit story beat.
