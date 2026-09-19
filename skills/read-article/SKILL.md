---
name: read-article
description: Turn an article or long text into narrated audio with a shareable player.
license: MIT
---

# Skill: read-article

Convert an article, blog post, or long block of text into natural narrated
audio — with a shareable, embeddable player.

## When to use

Invoke when the user wants to:

- Listen to an article, newsletter, or document
- Add an audio version of written content for accessibility
- Share a narrated read via a public link

## API

Estimate first (optional):

`POST https://api.audiopod.ai/api/v1/reader/estimate` — returns word/character/credit estimate.

Generate:

`POST https://api.audiopod.ai/api/v1/reader/generate`

Headers: `X-API-Key: ap_your_api_key`

Body:

- `text` (or a source URL/document): the content to narrate
- `voice_id`: stock, premium, or a cloned/custom voice

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

Poll `GET /api/v1/reader/{job_id}` until `COMPLETED`. Shareable player is at
`https://audiopod.ai/r/{id}`; the public read is served by
`GET /api/v1/reader/public/{id}`.

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
