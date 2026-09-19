---
name: generate-music
description: Music generation, powered by AudioMusic — compose royalty-free songs, instrumentals, and rap from a text prompt.
license: MIT
---

# Skill: generate-music

Generate full songs (vocals + instrumentation) from a natural-language prompt.
AudioMusic is AudioPod's music generation engine.

## When to use

Invoke when the user wants:

- A new song from a text caption, lyrics, or mood description
- Background music for a video or podcast
- Royalty-free music for commercial projects
- A specific genre, BPM, key, language, or duration

## API

`POST https://api.audiopod.ai/api/v1/music/text2music`

Task variants (same response + polling shape): `text2music` (prompt → full
song), `cover` (re-sing an uploaded track), `prompt2instrumental`,
`lyric2vocals`, `text2rap`, `text2samples`, `extend`, `retake`. Pick the
path that matches the task, e.g. `POST /api/v1/music/cover`.

Headers:

- `X-API-Key: ap_your_api_key` (or `Authorization: Bearer ap_your_api_key`)
- `Content-Type: application/json`

Body:

```json
{
  "caption": "lo-fi hip hop, rainy ambient pads, 90 BPM, 60 seconds",
  "duration_seconds": 60,
  "quality": "standard"
}
```

`duration_seconds` accepts 10–600 and is not tier-capped; omit it to let the
model choose. `quality` is the music quality tier — `standard` (default) or
`premium` (higher-fidelity variant billed at 2x, requires a paid tier).

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

`job_id` is an integer, unique within the music tool (job IDs are
per-tool — a music job and a stems job can share the same number).

Poll `GET /api/v1/music/jobs/{job_id}/status` for `COMPLETED` and an
`audio_url`. Note the `/status` suffix — `GET /api/v1/music/jobs/{job_id}`
without it is the DELETE/PATCH route and returns 405, and there is no
generic `/api/v1/jobs/{job_id}` endpoint (it would be ambiguous across
tools).

## Errors

- `402 PREMIUM_TIER_REQUIRED` — premium variant requested on free/basic tier
- `429` — rate limit; retry with exponential backoff
- `400 invalid_caption` — caption is empty or violates safety policy

## Pricing

Free tier available. Paid plans from $20/mo (Creator). See https://www.audiopod.ai/pricing.
