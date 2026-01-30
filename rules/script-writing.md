---
name: script-writing
category: script
impact: CRITICAL
metadata:
  tags: script, narration, pacing, wpm
---

## Script writing rules

### 1) Write for **voiceover-first**
The script is not prose — it is **time**.
- Assume narrated voiceover pace of **~137 words per minute** as a default.
- Silent scenes are allowed, but they must be explicit.

### 2) Target words from duration
Use this as a baseline:

- **TargetWords ≈ (DurationSeconds / 60) × 137**

Examples:
- 30s → ~69 words
- 45s → ~103 words
- 60s → ~137 words

If you include silent scenes, subtract those seconds from the narrated duration.

### 3) Scene-by-scene script
Write the script as **one line per scene**.
Keep each scene’s line to **one sentence max** (unless you intentionally want a fast montage).

Suggested format:

```
NARRATOR: ...
# or for silence
(SILENT): ...
# optional: include on-screen text
ONSCREEN: ...
```

### 4) Keep language filmable
Avoid abstract concepts unless you attach a concrete visual.
Bad: “He feels behind.”
Good: “He scrolls a hype feed, then closes the tab and opens the terminal.”

### 5) End with a visual payoff
Every short movie should have a final, unambiguous visual beat.
Examples:
- “shipped”
- email sent
- calendar booked
- build succeeded
