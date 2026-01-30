---
title: FAL key env compatibility
impact: HIGH
why: Avoid brittle setups where env var naming differs.
tags: fal, env
---

## FAL key env compatibility

Standardize on `FAL_KEY`, but accept common variants:
- `FAL_API_KEY`

Implementation recommendation:
- In code, set `process.env.FAL_KEY ||= process.env.FAL_API_KEY` at startup.
- In docs, instruct users to provide **either**.
