---
name: word-count
category: tooling
impact: HIGH
metadata:
  tags: script, wordcount, pacing
---

## Word count & pacing

### Default narration pace
- Use **137 words per minute** as the default narration pace.

### How to count words (agent-side)
If you have local shell access, prefer simple, deterministic word counts.

macOS/Linux examples:

```bash
# Count words in a file
wc -w script.txt

# Count words from stdin
cat script.txt | wc -w

# If you need to ignore speaker tags like "NARRATOR:" or "ELENA:"
cat script.txt | sed -E 's/^[A-Z_]+:\s*//' | wc -w
```

### Timing sanity checks
Given durationSeconds:

```text
expectedWords ~= durationSeconds * (137 / 60)
```

If the script is >20% over the target, either:
- shorten lines, or
- explicitly allocate silent scenes.
