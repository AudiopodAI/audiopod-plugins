---
name: create-podcast
description: Turn source documents or a topic into a multi-speaker podcast episode with a public RSS feed.
license: MIT
---

# Skill: create-podcast

Generate a multi-speaker podcast episode — grounded in source documents you
provide, or from a topic — with an editable outline and script, then a
distributable audio episode and public RSS feed.

## When to use

Invoke when the user wants to:

- Turn a report, article, or set of documents into a two-host podcast
- Generate an episode from a topic prompt
- Produce a distributable podcast with an RSS feed

## Workflow

### 1. Create a project

`POST https://api.audiopod.ai/api/v1/podcast/projects`

Headers: `X-API-Key: ap_your_api_key`

Body: `title`, and either source documents (upload/attach) or a `topic`.

### 2. Configure cast + generate the outline and script

- Set speaker voices (stock, premium, or cloned) per speaker profile:
  `PUT /api/v1/podcast/projects/{project_id}/speakers/{speaker_id}`
- Generate/edit the outline, then the dialogue script.

### 3. Estimate + generate the episode audio

- `GET /api/v1/podcast/projects/{project_id}/audio/estimate`
- `POST /api/v1/podcast/projects/{project_id}/audio/generate` (episodes up to ~60 min)
- Poll the project's progress endpoint until `COMPLETED`.

### 4. Publish an RSS feed

- `POST /api/v1/podcast/feed` returns a signed feed URL
- `GET /api/v1/podcast/feed/{token}.xml` serves the public RSS

## Pricing

Start free. See https://www.audiopod.ai/developers/pricing for per-minute rates.
