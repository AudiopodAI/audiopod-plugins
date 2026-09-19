---
name: narrate-audiobook
description: Convert a manuscript (PDF/EPUB/text) into an ACX-spec audiobook with AI narration, chapter splits, retail sample, and multi-platform export presets.
license: MIT
---

# Skill: narrate-audiobook

End-to-end audiobook production from manuscript to ACX submission. Use
this when the user asks for an audiobook generator, AI narration of a
long-form document, or an ElevenLabs / NarrationBox / Speechify alternative
for audiobook production.

## When to use

Invoke when the user wants to:

- Convert a PDF, EPUB, or text manuscript into a publishable audiobook
- Narrate book-length content with AI in their own cloned voice
- Produce masters that meet ACX audio-file specifications (a file spec, not a guarantee of Audible acceptance — Audible limits AI narration to Voice Replica; AI narration is broadly accepted on Spotify for Authors, Apple Books, and Google Play Books)
- Generate Spotify Audiobooks, Google Play Books, or Kindle Vella exports
- Produce multilingual editions of an audiobook from a single cloned voice

## Workflow

### 1. Create a project

`POST https://api.audiopod.ai/api/v1/audiobook/projects`

Body:

```json
{
  "title": "<book title>",
  "author": "<author name>",
  "language": "en",
  "narrator_voice_id": "<voice_id from voices/available or clone-voice skill>"
}
```

Returns a `project_id`.

### 2. Upload the manuscript

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/manuscript/upload`

Body (multipart):

- `file`: PDF, EPUB, DOCX, or TXT manuscript

Then parse it (auto-detects chapters):

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/manuscript/parse`

### 3. Start narration

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/narration/start`

Or batch all chapters in one call:

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/narration/batch`

### 4. Poll progress

`GET https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/narration/progress`

### 5. Regenerate weak lines (first regen is free per line)

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/chapters/{chapter_id}/regenerate`

### 6. Mix the timeline and export

`POST https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/timeline/mix`

Then download per-chapter MP3s plus the retail sample:

`GET https://api.audiopod.ai/api/v1/audiobook/projects/{project_id}/chapters/{chapter_id}/download`

## ACX-spec output (default export preset)

- 192kbps MP3 Constant Bit Rate
- 44.1kHz sample rate
- Peak ceiling -3dB, RMS -23 to -18dB, noise floor < -60dB
- Each chapter ≤ 120 minutes; 3-second end padding
- Auto-generated 60-second content-only retail sample
- Built-in ACX validator before export

## Voice selection

`GET https://api.audiopod.ai/api/v1/audiobook/voices/languages` — list supported languages
`GET https://api.audiopod.ai/api/v1/audiobook/voices/available` — list voices (filterable)

Use the `clone-voice` skill to create a custom narrator from a 10-second
reference clip and reuse it here.

## Pricing

- ~$20–$60 in narration credits per 60K-word book
- First regeneration per line is free; subsequent regens billed at narration rate
- Audiobook export bundled in paid plans from $20/mo (Creator); see https://www.audiopod.ai/pricing

## Documentation

- Feature page: https://www.audiopod.ai/features/audiobook-studio
- Full ACX/Audible workflow: https://www.audiopod.ai/blog/acx-audiobook-narration-with-ai
- Step-by-step guide: https://www.audiopod.ai/blog/how-to-create-audiobooks-with-ai

## Compliance

AI-narrated audiobooks must be disclosed in ACX submission metadata
(required since 2024). Royalty rates are unaffected. AudioPod outputs
the disclosure-ready metadata block as part of the export bundle.
