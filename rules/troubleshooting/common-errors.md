---
name: troubleshooting-common-errors
category: troubleshooting
impact: MEDIUM
metadata:
  tags: troubleshooting, errors, rate-limits
---

## Common errors + fixes

### fal queue API
- **401/403**: invalid `FAL_API_KEY` → verify key and header: `Authorization: Key $FAL_API_KEY`
- **429**: rate limited → exponential backoff (sleep 2, 4, 8…)
- **FAILED status**: inspect `status.json` error field; retry with simpler prompt

### ElevenLabs direct API
- **401/403**: invalid `ELEVENLABS_API_KEY`
- **429**: rate limit → backoff and retry

### Shell state issues
Agent shells may not persist `cd` or variables between commands.
- Prefer absolute paths
- Or prefix every command: `cd "$RUN_DIR" && ...`

### Duration mismatch
If you need 2s but model only supports ≥4s:
- generate 4s
- trim to 2s

See `rules/timing/clamp-and-trim.md`.
