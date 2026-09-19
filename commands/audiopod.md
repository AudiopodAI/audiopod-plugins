---
description: Show what AudioPod can do, check the MCP connection, and route the request to the right audio skill.
---

You have the AudioPod plugin installed. It provides ten MCP tools over
`https://mcp.audiopod.ai` and fourteen task skills.

If the user gave an argument after `/audiopod`, treat it as the audio task they
want and go straight to the matching skill. Otherwise:

1. List what is available, grouped by job to be done:
   - **Speech** — `text-to-speech`, `design-voice`, `clone-voice`, `change-voice`
   - **Long-form** — `narrate-audiobook`, `create-podcast`, `read-article`
   - **Music** — `generate-music`
   - **Cleanup and separation** — `denoise-audio`, `separate-stems`, `separate-speakers`
   - **Understanding** — `transcribe-audio`, `translate-audio`
   - **Utility** — `convert-media`
2. Verify the connection by calling the `check_job_status` tool's server, or by
   listing the available `audiopod` MCP tools. If the server is unreachable or
   returns 401, tell the user to set `AUDIOPOD_API_KEY` to a key from
   <https://www.audiopod.ai/dashboard/account/api-keys> (first key includes $1
   of free API credit).
3. Ask which task they want, then invoke the matching skill.

Long-running tools return a `job_id` — poll `check_job_status` until
`COMPLETED`. Polling is free.
