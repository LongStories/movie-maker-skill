# movie-maker-skill

Voiceover-first workflow for generating 10–60s mini movies.

## Structure

- `SKILL.md` – skill entrypoint + how to use
- `rules/` – small, composable guidance files (mirrors Vercel/Remotion patterns)
- `moviemaker-ai/` – CLI implementation (Node + ffmpeg + FAL)

## Quickstart

```bash
cd moviemaker-ai
npm install

# Provide either of these:
export FAL_KEY="..."   # preferred
# or
export FAL_API_KEY="..."  # alias

export OPENAI_API_KEY="..."

node src/cli.js --config examples/config.json --prompt examples/story.txt
```

Output is written to `moviemaker-ai/runs/<run-id>/`.
