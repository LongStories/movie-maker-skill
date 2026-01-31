---
name: runbook-start-here
category: runbook
impact: CRITICAL
metadata:
  tags: runbook, intake, questions, operator
---

## Start here: how to run the skill (one clear path)

### Input
A user request like:

> "Create a movie about X in Y style."

### Rule: ask questions unless explicitly told not to
Unless the user explicitly says *"don't ask questions"*, you MUST do an intake and WAIT for answers (or explicit confirmation of defaults) before generating media.
If your agent environment provides an "ask the user questions" tool, use it for the intake and any clarifying questions.

### Intake (copy/paste)
Ask these in one message:

1) **Length**: 30s / 45s / 60s?
2) **Aspect ratio**: 16:9 or 9:16?
3) **Voice**: narrator-only (default) or dialogue?
4) **Characters**: names + 1-2 sentence vibe each (we'll lock a canonical descriptor; see `rules/visuals/characters/character-descriptors.md`)
5) **Music**: none (default) or background music? (optional; see `rules/providers/elevenlabs/music.md`)
6) **Style**: pick a preset (cinematic / 3D animated / anime / 2D / photoreal)
7) **Quality mode**: Draft (480p), Standard (720p), HQ (1080p)
8) **Reference images**: any character/style refs? (if yes, ask user to paste/attach and say you'll save to `assets/refs/...`)

If the user doesn't answer, propose defaults and ask:

> "If I don't hear back, I'll do 30s, 16:9, narrator-only, 3D animated style, Draft (480p). Confirm?"

### Execute after confirmation
Once confirmed:

1) Create a fresh run dir
2) Create numbered `script.txt` + `shotlist.txt`
3) Build `manifest.json` (validate against schema)
4) Generate voiceover per line (preferred)
5) Compute timings → planned durations → generate durations → trims
6) Generate images → QA images (fix prompts and retry if needed)
7) Generate videos → trim → stitch locally

Follow the canonical runbook:
- `rules/start/end-to-end.md`
