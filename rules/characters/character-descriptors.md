---
name: character-descriptors
category: characters
impact: CRITICAL
metadata:
  tags: characters, descriptors, prompting, consistency
---

# Character descriptors (CRITICAL for consistency)

If you don’t lock character appearance early, image models will drift (age, hair, face shape, wardrobe).

## Rule 1 — Create a canonical descriptor per character
For every named character, create a **canonical descriptor string**.

### Minimum specificity requirement
- **At least 50 words per character** (more is fine).
- Include concrete, visual, filmable details.

This is a *default rule* — it is ignorable only if you are using **reference images via Seedream edit** for every shot (see `rules/characters/consistency.md`).

## Rule 2 — Descriptor checklist (copy/paste)
Use this template and fill it fully:

- **Name + age range** (e.g. “late 20s”, “mid 40s”)
- **Gender presentation** (if relevant)
- **Ethnicity / skin tone** (if relevant)
- **Face shape** (oval/square/heart), **jawline**, **cheekbones**
- **Eyes** (shape + placement), **brows**, **eyelids**, **crow’s feet**
- **Nose** (shape/size), **mouth/smile**, **teeth** (distinctive)
- **Hair** (color, cut, texture), **facial hair** (if any)
- **Body** (height impression, build, posture)
- **Wardrobe (locked)**: top/bottom/shoes, materials, fit
- **Distinguishing marks**: scars, tattoos, freckles, mole, glasses, piercings
- **Props** they consistently have (watch, ring, backpack, laptop stickers)
- **Expression baseline** (tired intensity, calm focus, etc.)

## Rule 3 — Good vs bad examples

### Bad (too vague)
- “Kind face, nice hair, casual clothes.”
- “Young woman, friendly smile.”

### Good (specific, consistent)
- “Deep-set hooded eyes with prominent crow’s feet, slightly crooked smile revealing a subtle overbite, narrow nose with a small bump on the bridge, short wavy dark hair with a messy side part, faint stubble along the jaw, lean build with slightly hunched posture, charcoal hoodie with frayed cuffs over a faded band tee, worn black jeans, scuffed white sneakers, thin silver ring on the left hand.”

## Rule 4 — Prompt placement (verbatim, not paraphrased)

When using descriptors (Strategy B), the descriptor must be pasted **verbatim** into every image prompt:
- put it in the **first 2–3 lines** of the prompt
- do **not** paraphrase
- do **not** reorder details

Example prompt structure:

```text
CHARACTER: <paste canonical descriptor verbatim>
CHARACTER: <paste canonical descriptor verbatim>

SHOT: Cinematic OTS ...
SETTING: ...
LIGHTING: ...
STYLE: ...
```

Why: paraphrasing causes drift (the model treats each variation as a new person).

## Rule 5 — Wardrobe changes must be story beats
If clothing changes, it must be explicit in the script/shotlist as an intentional beat.
Otherwise: keep wardrobe locked.
