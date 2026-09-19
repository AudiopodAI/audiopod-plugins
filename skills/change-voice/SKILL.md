---
name: change-voice
description: Convert speech from one voice into another while preserving the words and timing (voice-to-voice).
license: MIT
---

# Skill: change-voice

Convert an existing speech recording so it sounds like a different voice —
same words and timing, new timbre. Useful for re-voicing narration,
dubbing, or anonymizing a speaker.

## When to use

Invoke when the user wants to:

- Re-voice an existing recording in a different (stock, cloned, or designed) voice
- Swap a narrator without re-recording
- Change the perceived speaker of a clip

## API

`POST https://api.audiopod.ai/api/v1/voice/voice-convert`

Headers:

- `X-API-Key: ap_your_api_key`
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: source speech audio (or `url` to a public file)
- `voice_id`: target voice to convert into (from `GET /api/v1/voice/voices`, or a clone/design)

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/voice/convert/{job_id}/status` until `COMPLETED`, then
download the converted audio.

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
