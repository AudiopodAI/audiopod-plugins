---
name: translate-audio
description: Translate or dub spoken audio into another language.
license: MIT
---

# Skill: translate-audio

Translate speech in a recording into another language — for dubbing,
localization, or cross-language accessibility.

## When to use

Invoke when the user wants to:

- Dub a video or podcast into another language
- Produce a localized voiceover from a source recording
- Make spoken content accessible across languages

## API

OpenAI-compatible translation endpoint — transcribes speech and translates it
into the target language in one call:

`POST https://api.audiopod.ai/api/v1/audio/translations`

Headers:

- `X-API-Key: ap_your_api_key` (or `Authorization: Bearer ap_your_api_key`)
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio or video file (or `url` to a public file)
- `target_language`: ISO code of the language to translate into
- `source_language`: ISO code (auto-detected if omitted)

Behaves like its OpenAI counterpart — set the client base URL to
`https://api.audiopod.ai/api/v1` and call `/audio/translations`.

## Response

Returns the translated transcript in the response body.

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
