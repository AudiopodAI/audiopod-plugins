---
name: separate-speakers
description: Diarize a recording and split it into per-speaker audio tracks.
license: MIT
---

# Skill: separate-speakers

Identify who spoke when in a multi-speaker recording, and optionally split
it into one clean audio track per speaker. AudioDiarize is AudioPod's speaker
separation engine.

## When to use

Invoke when the user wants to:

- Label a conversation by speaker ("Speaker 1", "Speaker 2", …)
- Extract a single participant's audio from an interview or meeting
- Prepare per-speaker tracks for editing or transcription

## API

Diarization (labels + timeline):

`POST https://api.audiopod.ai/api/v1/speaker/diarize`

Per-speaker audio tracks:

`POST https://api.audiopod.ai/api/v1/speaker/separate`

Headers:

- `X-API-Key: ap_your_api_key`
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio or video file (or `url` to a public file)
- `min_speakers` / `max_speakers`: optional bounds (1–20)

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/speaker/jobs/{job_id}` until `COMPLETED` for the
per-speaker segments and download URLs.

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
