---
name: text-to-speech
description: Text to speech, powered by AudioSonic — synthesize natural speech from 200+ voices across 200+ languages, with inline directing and optional word-level timestamps.
license: MIT
---

# Skill: text-to-speech

Turn text into natural, expressive speech. AudioSonic is AudioPod's
text-to-speech engine — 200+ stock voices, custom clones, or designed voices,
across 200+ languages.

## When to use

Invoke when the user wants:

- A voiceover, narration, or spoken version of some text
- Expressive delivery (emotion, emphasis, pauses, non-verbal sounds)
- A follow-along / karaoke transcript with word timing
- Speech in a specific voice, language, or speaking rate

## API

`POST https://api.audiopod.ai/api/v1/voice/voices/{voice_id}/generate`

Headers:

- `X-API-Key: ap_your_api_key` (or `Authorization: Bearer ap_your_api_key`)
- `Content-Type: multipart/form-data`

Body (form-data):

- `text`: the text to speak (up to 20,000 characters; supports inline directing below)
- `language`: ISO 639-1 (default `en`)
- `speed`: 0.5–2.0 (default 1.0)
- `generation_params`: optional JSON string — `delivery_mode` (`stable` | `balanced` | `creative`), `timestamp_type` (`word` | `character`), `text_normalization` (bool)

List voices with `GET /api/v1/voice/voices`. Use the `clone-voice` or
`design-voice` skill to create a custom `voice_id`.

## Inline directing (all tiers)

Write directing directly inside `text`:

- **Emotion / delivery** — a leading bracket per segment: `[whispering, tense] Did you hear that?`
- **Non-verbal sounds** — `[laugh] [sigh] [clear throat] [breathe] [cough] [yawn] [chuckle] [gasp] [groan]`
- **Pauses** — `<break time="500ms"/>` (≤10s each, ≤20 per request)
- **Pronunciation** — inline IPA between slashes: `the city of Worcester /ˈwʊstər/`

## Timestamps

Set `generation_params.timestamp_type` to `word` (or `character`). The job
response then includes a presigned `timestamps_url` sidecar (word/segment
start + end times) for follow-along or karaoke UIs.

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/voice/tts-jobs/{job_id}/status` until `COMPLETED`, then
download the audio (and `timestamps_url` if requested).

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
