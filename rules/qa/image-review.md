---
name: qa-image-review
category: qa
impact: HIGH
metadata:
  tags: qa, vision, images, review, iteration
---

## QA: Review images before generating videos (recommended)

Video generation is the most expensive step. If your agent can see images (multimodal), do a quick QA pass **after images** and before generating videos.

### When to do this
- Always for a first run
- Always when you care about character consistency
- Skip only if you're doing a rapid cheap draft and are OK with throwaway results

### How to review
If the agent is multimodal:
- open each generated image file and verify it matches the shot intent.

If the agent is not multimodal:
- ask the user to approve the images (use an "ask the user questions" tool if available), or
- do a minimal heuristic check (file exists, dimensions, not blank), and proceed.

### Review checklist (per shot)
Confirm:
1) **Intent** is reflected (the image matches what this beat is trying to communicate)
2) **Character look** is as expected (face/body/clothes match your mental model)
3) **Shot type** matches (OTS/CU/WS/etc.)
4) **Subject** is correct (right character, count of characters)
5) **Setting** matches (location + time-of-day)
6) **Continuity** (same character clothing/hair/age across scenes)
7) **No unwanted text/logos** (unless explicitly requested)
8) **No obvious artifacts** (extra limbs, mangled hands/faces)
9) **Composition** supports the lineRefs (the spoken beat)

### If it fails: what to change
Modify the relevant fields in the manifest:
- `shot.imagePrompt`
- optionally `shot.motion` / `shot.videoPrompt` (if the failure is about motion intent)
- optionally `style` (global) if style drift is the cause
- optionally character descriptor/reference strategy

### Retry strategy
- Regenerate the image only for the failing shot.
- Keep `num_images=1` (don't fan out) unless you're stuck.
- Cap retries: **max 3 attempts per shot**.

### Logging
Always keep:
- the original prompt
- the regenerated prompt
- file paths to each attempt

This makes it easy to learn which prompt patterns work.
