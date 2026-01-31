---
name: provider-defaults
category: providers
impact: HIGH
metadata:
  tags: providers, defaults, integration
---

# Provider defaults

This repo keeps **provider-specific details isolated** under `rules/providers/`.
Non-provider rules should remain provider-agnostic, with **one default mapping** here.

## Default mapping (current)

- **Voiceover:** fal Eleven v3 TTS (no ElevenLabs key needed)  
  See `rules/providers/fal/voiceover-elevenlabs.md`
- **Alt voiceover:** direct ElevenLabs API (requires `ELEVENLABS_API_KEY`)  
  See `rules/providers/elevenlabs/voiceover.md`
- **Image:** fal (Seedream)  
  See `rules/providers/fal/overview.md`
- **Video:** fal (Seedance)  
  See `rules/providers/fal/overview.md`
- **Music (optional):** ElevenLabs  
  See `rules/providers/elevenlabs/music.md`
- **Transcription:** ElevenLabs Scribe  
  See `rules/providers/elevenlabs/transcription.md`

Model combos (image + video) are defined in:
- `rules/providers/model-presets.md`

Pricing lookup:
- `rules/providers/pricing.md`

## Switching providers (e.g., Mire)

1) Add docs under `rules/providers/<provider>/`.
2) Update this file to point to the new defaults.
3) Keep the rest of the rules provider-agnostic.
4) Use the manifest `providers.*` fields to override per project/scene/shot.
