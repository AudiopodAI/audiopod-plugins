---
name: design-voice
description: Create a brand-new voice from a text description, preview candidates, and publish one as a reusable voice.
license: MIT
---

# Skill: design-voice

Design a voice that doesn't exist yet from a plain-text description
("a warm, gravelly late-night radio host with a slow cadence"), preview
candidates, then publish the one you like as a reusable voice for TTS.
Voice Design is part of the AudioSonic voice engine.

## When to use

Invoke when the user wants:

- An original character or brand voice, without a reference recording
- Several voice options to choose from before committing
- A reusable custom voice they can call from `text-to-speech`

## 1. Generate previews

`POST https://api.audiopod.ai/api/v1/voice/design`

Headers: `X-API-Key: ap_your_api_key`

Body:

```json
{
  "design_prompt": "a warm, gravelly late-night radio host, slow and intimate",
  "preview_text": "You're listening to the midnight hour.",
  "num_samples": 3
}
```

`design_prompt` is 30–250 chars, `preview_text` is 20–300 chars, and
`num_samples` is 1–3. Returns candidate previews. This is a **paid-tier**
feature, billed per preview delivered (and refunded on failure).

## 2. Publish a preview

`POST https://api.audiopod.ai/api/v1/voice/design/publish`

Publishes a chosen preview into a durable `voice_id` you can reuse with the
`text-to-speech` skill.

## Errors

- `402 PREMIUM_TIER_REQUIRED` — voice design requires a paid tier

## Pricing

Billed per preview (250 credits/preview). See
https://www.audiopod.ai/developers/pricing.
