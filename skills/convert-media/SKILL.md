---
name: convert-media
description: Convert audio between formats (MP3, WAV, FLAC, OGG, M4A, AAC).
license: MIT
---

# Skill: convert-media

Convert an audio file from one format to another, with selectable quality.

## When to use

Invoke when the user wants to:

- Convert between MP3, WAV, FLAC, OGG, M4A, or AAC
- Re-encode audio at a specific quality (e.g. lossless WAV → compressed MP3)
- Normalize inputs to a single format before further processing

## API

`POST https://api.audiopod.ai/api/v1/media-converter/convert`

Headers:

- `X-API-Key: ap_your_api_key`
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio file (or `url` to a public file)
- `output_format`: `mp3` | `wav` | `flac` | `ogg` | `m4a` | `aac`
- `quality`: `low` | `medium` | `high` (default) | `lossless`

Batch multiple files with `POST /api/v1/media-converter/bulk-convert`.

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/media-converter/jobs/{job_id}` until `COMPLETED`, then
download the converted file.
