---
name: separate-stems
description: Split a song into vocals, drums, bass and more, or isolate one instrument from a 45-instrument catalog.
license: MIT
---

# Skill: separate-stems

Split any song into isolated stems — vocals, drums, bass, guitar, piano — or
isolate a single instrument from a 45-instrument catalog, for remixing,
karaoke, or sampling. AudioStems is AudioPod's stem separation engine.

## When to use

Invoke when the user wants to:

- Remove vocals from a song (karaoke / instrumental track)
- Isolate a specific instrument (acapella, drum loop, bass-only)
- Remix or sample a track legally without a multitrack stem pack
- Build a karaoke file with an instrumental + lyric overlay

## API

`POST https://api.audiopod.ai/api/v1/stem-extraction/api/extract`

Headers:

- `X-API-Key: ap_your_api_key` (or `Authorization: Bearer ap_your_api_key`)
- `Content-Type: multipart/form-data`

Body (form-data):

- `file`: audio file (MP3, WAV, FLAC, OGG) — or `url` to a public audio file
- `mode`: `single` · `two` (vocals + instrumental) · `four` (default) · `six` ·
  `producer` · `studio` · `mastering`
- `stem`: only for `mode=single` — one of `vocals`, `drums`, `bass`, `guitar`,
  `piano`, `other`

The `producer`, `studio`, and `mastering` modes decompose the drum kit and
split the bass, yielding **up to** 8, 12, and 16 stems respectively — the exact
count depends on what the track actually contains (a song with no cymbals
yields no cymbal stem). Plan those as "up to N", never a fixed number.

**Plan gating:** `producer` needs Creator or above; `studio` and `mastering`
need Pro or above. `single`, `two`, `four`, and `six` work on every plan
including the free tier.

### Isolating one instrument from the full catalog

`mode=single` on the endpoint above accepts the six core stems. To reach the
full 45-instrument catalog (saxophone, violin, lead vocal, electric guitar, …),
use:

`POST https://api.audiopod.ai/api/v1/stem-extraction/extract/advanced`

- `single_stem`: a catalog id — returns that instrument plus an everything-else
  track
- `exclude_stems`: a JSON array with one id — returns the track without it

List the catalog with `GET /api/v1/stem-extraction/catalog`. Each entry carries
an `id`, a `label`, the `public_tier` it requires, and an `allowed` boolean for
the calling key — check `allowed` before dispatching rather than handling a
rejection. Catalog isolation beyond the seven core stems requires Creator or
above.

## Response

```json
{ "job_id": 12345, "status": "PENDING" }
```

`job_id` is an integer, unique within the stem-extraction tool.

Poll `GET /api/v1/stem-extraction/status/{job_id}` for the per-stem
download URLs. (Job IDs are per-tool — there is no generic
`/api/v1/jobs/{job_id}` endpoint.)

## Notes

- Input length caps by plan: Free/Basic 30 minutes, Creator 2 hours, Pro 5
  hours, Studio 10 hours
- Upload size caps by plan: Free/Basic 250 MB, Creator 500 MB, Pro and Studio 1 GB
- Stems are royalty-free for personal and commercial use on paid plans
