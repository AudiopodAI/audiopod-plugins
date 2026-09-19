---
name: denoise-audio
description: Remove background noise, hiss, and hum from a recording while preserving the voice.
license: MIT
---

# Skill: denoise-audio

Clean up a noisy recording — remove hiss, hum, traffic, room tone, and
background chatter — without harming the voice character.

## When to use

Invoke when the user wants to:

- Clean a voice memo, interview, or field recording before transcription
- Remove hum or hiss from a podcast or voiceover
- Improve audio quality prior to stem separation or narration

## API

`POST https://api.audiopod.ai/api/v1/denoiser/denoise`

Headers:

- `X-API-Key: ap_your_api_key`
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio file (or `url` to a public file)
- `mode`: `balanced` (default) · `studio` · `ultra`

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/denoiser/jobs/{job_id}` until `COMPLETED`, then download
the cleaned audio.

## Tip

Running `denoise-audio` first noticeably improves `transcribe-audio`
accuracy on noisy field recordings.
