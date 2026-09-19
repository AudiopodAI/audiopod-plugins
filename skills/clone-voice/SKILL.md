---
name: clone-voice
description: Clone a voice from a 5–30s reference clip and reuse it for narration or TTS.
license: MIT
---

# Skill: clone-voice

Clone a human voice from a short reference clip, then synthesize new speech in
that voice. Voice cloning runs on AudioSonic, AudioPod's voice engine, and
reaches 200+ languages from a single clone.

## When to use

Invoke when the user wants to:

- Reuse their own voice for narration, audiobook chapters, or podcasts
- Clone a public-domain or licensed voice for a creative project
- Generate dubs in another language while preserving the speaker's timbre

## Two-step API

### 1. Clone the voice (instant)

`POST https://api.audiopod.ai/api/v1/voice/voice-clone`

Headers: `X-API-Key: ap_your_api_key`

Body (multipart):

- `name`: human-readable name for the clone
- `reference_audio`: 5–30s clean speech sample (WAV, MP3, FLAC)
- `consent_attestation`: `true` (required — must own or have rights to the voice)

Instant cloning — the clone is ready in seconds. Returns an integer
`voice_id`; if a status field is pending, poll
`GET /api/v1/voice/clone/{job_id}/status`.

### 2. Synthesize speech with the clone

`POST https://api.audiopod.ai/api/v1/voice/voices/{voice_id}/generate`

```json
{
  "text": "Welcome back to the show.",
  "language": "en"
}
```

Where `{voice_id}` is the integer returned in step 1.

## Safety

AudioPod blocks voice cloning of public figures and enforces a consent
attestation on every training request. Audio output is watermarked with
inaudible provenance metadata.
