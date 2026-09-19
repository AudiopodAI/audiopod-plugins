# AudioPod AI

AudioPod is a hosted audio-AI platform. This extension connects the agent to
AudioPod's MCP server at `https://mcp.audiopod.ai` (Streamable HTTP, MCP
`2025-06-18`), authenticated with the `X-API-Key` header.

## Tools

| Tool | Does |
|---|---|
| `text_to_speech` | Speech in 85+ languages, 500+ voices and custom clones |
| `clone_voice` | Clone a voice from a 5–30s reference clip |
| `change_voice` | Convert a recording to a different target voice |
| `generate_music` | Songs, instrumentals, rap, or vocal stems from a text prompt |
| `separate_stems` | Split a track into stems (vocals, drums, bass, …); two-stem mode = karaoke |
| `separate_speakers` | Isolate each speaker into a separate track |
| `transcribe_audio` | Transcribe with word-level timestamps and speaker diarization |
| `denoise_audio` | Remove background noise while preserving voice character |
| `convert_media` | Convert audio/video formats (mp3/wav/flac/ogg/m4a, mp4/mov) |
| `check_job_status` | Poll the status/result of a long-running job (free) |

## How to use them

Long-running tools return a `job_id` immediately. Poll `check_job_status` with
that id until the status is `COMPLETED`, then read the output URL from the
result. Polling is free.

Prefer the bundled skills in `skills/` for task-shaped requests (narrate an
audiobook, build a podcast, make a karaoke track) — they describe the full
multi-step flow, including the REST endpoints for anything not exposed as a
tool.

## Getting a key

Create one at <https://www.audiopod.ai/dashboard/account/api-keys>. The first
key comes with $1 of free API credit. Pricing:
<https://www.audiopod.ai/developers/pricing>.
