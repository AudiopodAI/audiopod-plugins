<!--
customer-comms-allow-file: Gemini

Scope note for the AudioPod comms gate: the assistant names in this README are
INSTALL TARGETS — the catalogs a user installs this package from. They are not
claims about any engine AudioPod runs. The banned-vendor rule exists to stop us
surfacing the stack under our own hood; naming the client an integration ships
for is the entire point of an integration README, and omitting it would make
the install instructions unusable.
-->

# AudioPod agent plugin

One installable package that gives any plugin-capable coding agent **AudioPod's
audio AI** — ten MCP tools over a single hosted endpoint, plus fourteen task
skills that describe the full multi-step flows (narrate an audiobook, build a
podcast, cut a karaoke track).

No install, no local models, no GPU.

| | |
|---|---|
| **MCP endpoint** | `https://mcp.audiopod.ai` |
| **Transport** | Streamable HTTP (MCP `2025-06-18`) |
| **Auth** | `X-API-Key: ap_*` (or OAuth2 / JWT) — [get a free key](https://www.audiopod.ai/dashboard/account/api-keys) |
| **Docs** | https://docs.audiopod.ai/sdks/mcp |
| **Pricing** | https://www.audiopod.ai/developers/pricing |
| **Registry** | `ai.audiopod/audiopod` on the official [MCP Registry](https://registry.modelcontextprotocol.io) |
| **Server repo** | [AudiopodAI/audiopod-mcp](https://github.com/AudiopodAI/audiopod-mcp) — the standalone MCP listing (`server.json` + server card) |

Your first API key comes with **$1 of free API credit**.

## Set your key first

Every client below reads the key from the `AUDIOPOD_API_KEY` environment
variable — except Gemini CLI, which prompts for it and stores it in your system
keychain. No key is ever written into this repo.

```bash
export AUDIOPOD_API_KEY=ap_your_key_here
```

## Install

### Claude Code

```bash
claude plugin marketplace add https://github.com/AudiopodAI/audiopod-plugins
claude plugin install audiopod@audiopod
```

Then `/audiopod` in a session to see what is available and check the
connection. The `AudiopodAI/audiopod-plugins` shorthand also works if your git
is configured for GitHub SSH; the full `https://` URL above works either way.

### Gemini CLI

```bash
gemini extensions install https://github.com/AudiopodAI/audiopod-plugins
```

Gemini reads `gemini-extension.json`, prompts for the AudioPod API key on
install and stores it as a sensitive setting.

### Codex

Browse and install from the plugin directory inside Codex CLI:

```
/plugins
```

To try it before it lands in the directory, clone the repo and add it to your
personal marketplace at `~/.agents/plugins/marketplace.json`:

```bash
git clone https://github.com/AudiopodAI/audiopod-plugins.git ~/plugins/audiopod
```

```json
{
  "plugins": [
    {
      "name": "audiopod",
      "source": { "source": "local", "path": "~/plugins/audiopod" }
    }
  ]
}
```

### Cursor

**Customize → Plugins →** search for *AudioPod* → **Install**, and pick project
or user scope. To test before it is listed, clone the repo into
`~/.cursor/plugins/local`:

```bash
git clone https://github.com/AudiopodAI/audiopod-plugins.git ~/.cursor/plugins/local/audiopod
```

### Kiro

**Powers panel → Add Custom Power → Import power from GitHub**, enter
`https://github.com/AudiopodAI/audiopod-plugins`, then **Install**.

### Any other MCP client

Skip the plugin layer and point your client straight at the server:

```json
{
  "mcpServers": {
    "audiopod": {
      "type": "streamable-http",
      "url": "https://mcp.audiopod.ai",
      "headers": { "X-API-Key": "ap_YOUR_KEY" }
    }
  }
}
```

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

Long-running tools return a `job_id` immediately — poll `check_job_status`
until `COMPLETED`. Polling is free.

## Skills

Fourteen skills ship in `skills/`, each a `SKILL.md` the agent loads on demand.
They cover the tasks that need more than one call, and document the REST
endpoints for the ones that are not exposed as MCP tools.

| Skill | For |
|---|---|
| `text-to-speech` | Voiceover and narration, with inline directing and word timestamps |
| `design-voice` | Build a brand-new voice from a text description |
| `clone-voice` | Clone a voice from a reference recording |
| `change-voice` | Re-voice an existing recording |
| `narrate-audiobook` | Manuscript → ACX-spec audiobook with chapters and retail sample |
| `create-podcast` | Script or source material → multi-voice podcast episode |
| `read-article` | Article or URL → listenable audio |
| `generate-music` | Songs, instrumentals and stems from a prompt |
| `separate-stems` | Stems and karaoke tracks |
| `separate-speakers` | One track per speaker |
| `transcribe-audio` | Transcripts with timestamps and diarization |
| `translate-audio` | Translate and re-voice into another language |
| `denoise-audio` | Clean up noisy recordings |
| `convert-media` | Format and container conversion |

## What is in this repo

| Path | Read by |
|---|---|
| `plugin.json`, `mcp.json` | Agent Plugins layout — Codex, Cursor, Kiro |
| `.claude-plugin/plugin.json`, `.claude-plugin/marketplace.json` | Claude Code |
| `.cursor-plugin/plugin.json` | Cursor |
| `gemini-extension.json`, `GEMINI.md` | Gemini CLI |
| `skills/<name>/SKILL.md` | all of the above |
| `commands/audiopod.md` | Claude Code — the `/audiopod` slash command |

The manifests are deliberately duplicated rather than merged: the formats agree
on `skills/` and on the shape of an MCP server entry, but disagree on where the
manifest lives and on the key that carries the endpoint URL
(`url` vs `httpUrl`).

## Building a startup on this?

Apply to **[AudioPod for Startups](https://www.audiopod.ai/startups)** — free
Pro for 3 months plus developer API credits for eligible early-stage teams.

## License

MIT — see [LICENSE](./LICENSE). (Covers this plugin package; the AudioPod
service itself is governed by the [AudioPod Terms](https://www.audiopod.ai/terms).)
