---
name: assets
category: assets
impact: HIGH
metadata:
  tags: assets, files, organization, paths, rendering
---

# Assets (local files) rules

This workflow produces and consumes a set of local artifacts (audio, images, clips, captions, manifest). These rules keep it reproducible and easy to debug.

## 1) Canonical asset types

- **Script** (text): `script.txt` or `script.md`
- **Shotlist** (text): `shotlist.txt` or `shotlist.md`
- **Manifest** (json): `manifest.json` (required)
- **Voiceover** (audio): `voiceover.mp3` (or per-line mp3s)
- **Images** (jpg/png): one per shot
- **Clips** (mp4): one per shot or per scene
- **Captions** (srt): `captions.srt` (optional)
- **Final** (mp4): `movie.mp4`
- **Thumbnail** (jpg/png): optional

## 2) Organize by run id

Always write outputs into a unique run folder so work is reproducible.

Recommended structure:

```
runs/
  2026-01-30T17-19-00Z-<slug>/
    manifest.json
    script.txt
    shotlist.txt
    audio/
      voiceover.mp3
      L001.mp3
      L002.mp3
    images/
      SH01.jpg
      SH02.jpg
    videos/
      SH01.mp4
      SH02.mp4
    captions/
      captions.srt
    final/
      movie.mp4
      thumb.jpg
```

## 3) Naming rules

### Script line ids
If you write per-line audio files, name them by line id:
- `L001.mp3`, `L002.mp3`, …

### Shot ids
Name generated images/videos by shot id:
- `SH01.jpg`, `SH01.mp4`, …

This matches the schema (`movie-manifest.schema.json`) and makes debugging trivial.

## 4) Paths inside the manifest

The manifest may include local paths under `shot.assets.*`.
Rule: paths should be **relative to the run folder** when possible.

Example:

```json
{
  "assets": {
    "imagePath": "images/SH01.jpg",
    "videoPath": "videos/SH01.mp4",
    "voicePath": "audio/L001.mp3"
  }
}
```

## 5) Keep heavy outputs out of git

This repo is docs-first. If you implement a pipeline alongside it:
- add `runs/` to `.gitignore`
- never commit generated media

## 6) Rendering compatibility

For ffmpeg rendering, keep:
- consistent resolution/aspect ratio across all shot videos
- consistent fps (or explicitly convert)

If you must normalize:
- transcode clips to a shared codec/fps before concatenation.
