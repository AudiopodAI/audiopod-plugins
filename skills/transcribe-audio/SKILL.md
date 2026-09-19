---
name: transcribe-audio
description: Transcribe audio with speaker diarization and word-level timestamps.
license: MIT
---

# Skill: transcribe-audio

Transcribe podcasts, meetings, interviews, or any speech audio with
speaker diarization and word-level timestamps. AudioTranscribe is AudioPod's
speech-to-text engine.

## When to use

Invoke when the user wants:

- A text transcript of a podcast, meeting, lecture, or interview
- Speaker-separated transcripts ("Speaker 1: …, Speaker 2: …")
- Subtitles / captions for a video

## API

`POST https://api.audiopod.ai/api/v1/transcription/transcribe` (URL) or
`POST /api/v1/transcription/transcribe-upload` (multipart file).

Headers:

- `X-API-Key: ap_your_api_key` (or `Authorization: Bearer ap_your_api_key`)
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio or video file (up to 2 GB) — or `url` to a public file
- `language`: ISO 639-1 code (or `auto`)
- `model_type`: `standard` (default) or `premium` / `premium-diarized` — premium adds higher accuracy, punctuation, per-word confidence, and native speaker labels
- `diarize`: `true` to enable speaker diarization; `min_speakers` / `max_speakers` (1–20)
- `output_format`: `srt` | `vtt` | `txt` | `json`

Need live captions? Real-time streaming transcription is available over
WebSocket (interim + final results).

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

`job_id` is an integer, unique within the transcription tool.

Poll `GET /api/v1/transcription/jobs/{job_id}` for `COMPLETED`, then fetch
the transcript via `GET /api/v1/transcription/jobs/{job_id}/transcript`.
(Job IDs are per-tool — there is no generic `/api/v1/jobs/{job_id}`
endpoint.)

## Accuracy

99.8% word accuracy on clean studio audio (English); 96–98% on noisy
field recordings after running the `denoise-audio` skill first.
