---
name: claude-ai-connectors
description: The owner had the claude.ai MCP connectors disabled (2026-11-17); do not rely on them unless asked
metadata:
  type: feedback
---

On 2026-11-17, in the session working in the bun checkout, the owner typed "disclade all claude.ai" (disable) over the `/mcp` listing of the eight claude.ai connectors: Claude Docs, Figma, Gmail, Google Calendar, Google Docs, Google Drive, Google Sheets, Google Slides. That session applied it as a per-project `disabledMcpServers` list in `~/.claude.json`.

**Why:** not stated. My interpretation: they add many tools to every session and are not part of the owner's workflow; the plugin MCP servers (relay, bridge, OVH, Hostinger, memory) and claude-in-chrome were left enabled.

On 2026-11-17 the owner asked, in the aysh session, to keep only the servers that project really uses: for `C:/src/aysh` the eight claude.ai connectors and every OVH and Hostinger server are disabled; relay, claude-bridge, memory and claude-in-chrome (needed to prove the web front end) stay.

**How to apply:** the choice is per project, so other projects still load the connectors. Do not reach for a claude.ai connector by default (for Figma or Google work, use the relay tools or ask); if the owner repeats the request in another project, add the same eight names to that project's `disabledMcpServers`. Whether the owner wants this in every project is not confirmed.
