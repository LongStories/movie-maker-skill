---
name: script-and-shotlist
category: structure
impact: CRITICAL
metadata:
  tags: script, shotlist, numbering, references
---

## Canonical artifacts

To scale the workflow, keep **two separate artifacts** that reference each other:

1) **Script** = what’s said (dialogue/voiceover) + optional action lines
2) **Shot list** = what’s seen (shots grouped into scenes) + timing plan

### Why two artifacts?
- You can rewrite visuals without rewriting dialogue.
- You can re-time shots after TTS without touching the script.
- You can generate captions/transcripts directly from the script audio.

## Script format (line-number friendly)

Use a plain text/Markdown file where each speakable line is numbered.
This makes `grep`, diffs, and references trivial.

Recommended format:

```text
# SCRIPT

L001 NARRATOR: Rain writes slow lines on the window while the cursor blinks.
L002 (SILENT): Alex scrolls a hype feed, then closes the tab.
L003 MIRA: Three steps: fix the test, update config, run release.
L004 ALEX: Okay.
```

Rules:
- Prefix every line with `L###` (zero-padded).
- Use `(SILENT)` lines for non-voice beats.
- Keep one sentence per line unless you intentionally want a fast montage.

## Shot list format (scene → shots)

The shot list is divided into scenes. Each scene can reference script lines.

Recommended format:

```text
# SHOTLIST

S01 (Night, apartment workspace)
  Lines: L001-L002
  Shot 01 (OTS, 5s): Rain on window, builder at desk, terminal glow.
  Motion: slow push-in, rain streaks moving.

S02 (Desk, dual monitors)
  Lines: L003-L004
  Shot 01 (CU, 5s): Hands hover, checklist reorganizes.
  Motion: minimal drift, cursor blinking.
```

Rules:
- Scenes are `S##`.
- Each scene includes `Lines:` referencing script line IDs.
- Each shot includes `shotType`, target duration, a visual, and motion.

## After TTS: timing becomes authoritative
Once voiceover exists (or you have transcript timestamps), use that to finalize:
- per-line durations
- per-scene durations
- per-shot durations

Word-count targets are only for planning.
