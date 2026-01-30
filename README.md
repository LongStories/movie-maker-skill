# movie-maker-skill

A **documentation-first AgentSkill** for creating 10–60s mini movies.

This repo intentionally ships **no executable pipeline**. The goal is to provide enough rules + templates so an agent can generate mini-movies using local tools (ffmpeg) and model APIs (FAL, TTS) in your existing environment.

## Structure

- `SKILL.md` – skill entrypoint + how to use
- `rules/` – small, composable guidance files (mirrors Vercel/Remotion patterns)
