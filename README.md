# GetRun MCP Server

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for
[GetRun](https://getrun.id) — generate images, video, audio and chat with
**86 public AI models from 18 providers** from inside any MCP client. At the
time of this update, 81 are runnable and 5 are explicitly marked coming soon.

```
https://getrun.id/mcp
```

No API key to paste. The server speaks OAuth 2.1 with dynamic client
registration, so a client is authorised once through a consent screen and
billed against the same Rupiah balance as the web app and the REST API.

| | |
|---|---|
| **Endpoint** | `https://getrun.id/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.1 + dynamic client registration ([RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)) |
| **Protocol versions** | `2025-06-18`, `2025-03-26`, `2024-11-05` |
| **Scopes** | `read`, `generate`, `manage` |
| **Tools** | 21 |
| **Pricing** | Per use, in Indonesian Rupiah. No subscription. Failed tasks are not charged. |

This repository holds the connector's public metadata and setup guide. The
service itself is hosted — there is nothing to install and no server to run.

## Install

### Claude (claude.ai)

Settings → Connectors → **Add custom connector**. Name it `GetRun`, paste the
URL above, save, then press **Connect** and approve the permissions.

### Claude Code

```bash
claude mcp add --transport http getrun https://getrun.id/mcp
```

Then run `/mcp` inside a session, pick `getrun`, and complete the browser
consent flow once.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "getrun": {
      "url": "https://getrun.id/mcp"
    }
  }
}
```

### Any other MCP client

Point it at the URL. Clients that only speak stdio can bridge with
[`mcp-remote`](https://www.npmjs.com/package/mcp-remote):

```json
{
  "mcpServers": {
    "getrun": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://getrun.id/mcp"]
    }
  }
}
```

## Tools

| Tool | What it does |
|---|---|
| `getrun_generate_image` | Text → image, or image-to-image with a reference |
| `getrun_generate_video` | Text or image → video clip |
| `getrun_generate_audio` | Text → song, sound effect, or speech |
| `getrun_chat` | Chat models, billed per token |
| `getrun_run_model` | Any catalogue model with its raw parameters |
| `getrun_list_models` | Catalogue with prices and JSON Schema per model |
| `getrun_estimate_price` | Cost of a generation before running it |
| `getrun_upload_image` | Upload or import an image for use as model input |
| `getrun_get_balance` | Available and reserved balance |
| `getrun_get_task` | Status and result of a running generation |
| `getrun_list_creations` | History with prompts, cost and output URLs |
| `getrun_hide_creation` | Tidy the gallery; billing history is kept |
| `getrun_list_characters` | Saved characters — a consistent face across generations |
| `getrun_save_character` | Create or update a character |
| `getrun_delete_character` | Delete a character; past generations remain |
| `getrun_blender_list_projects` | List the user's 3D Studio projects |
| `getrun_blender_create_project` | Create a new 3D Studio project |
| `getrun_blender_get_project` | Get a project's scene version, files, and render state |
| `getrun_blender_save_scene` | Save a versioned 3D scene draft |
| `getrun_blender_quote` | Quote a preview or final 3D render before spending balance |
| `getrun_blender_render` | Render a saved 3D scene using an approved quote |

## Catalogue

86 public models from 18 providers, as of October 2026. The status column in
the live API is authoritative; the counts below include coming-soon models.

| Category | Models | Examples |
|---|---|---|
| Video | 31 | Seedance, Kling, PixVerse, MiniMax |
| Image | 23 | Nano Banana, Seedream, GPT Image, Qwen |
| Text & chat | 24 | GPT, Claude, Gemini, Grok, Kimi, DeepSeek |
| Audio | 8 | Suno, ElevenLabs TTS / Music / Sound Effects |

The live list is always at [`GET /v1/models`](https://getrun.id/v1/models) —
this table is a snapshot, that endpoint is the source of truth.

## Billing

Identical to the REST API: every tool calls `/v1` in the same process, so
pricing, balance reservation, schema validation and rate limits are the same,
and results appear in the dashboard's usage history.

Generation tools spend real balance. `getrun_estimate_price` returns the cost
of a call before you make it, and a task that fails is never charged.

## Also available

- [REST API documentation](https://getrun.id/docs) — generation tasks plus OpenAI-compatible chat
- [`llms.txt`](https://getrun.id/llms.txt) — compact discovery map for coding agents
- [`llms-full.txt`](https://getrun.id/llms-full.txt) — complete API, MCP, catalogue, and FAQ context
- [OpenAPI 3.1 spec](https://getrun.id/openapi-v1.yaml)
- [Model catalogue](https://getrun.id/market) and [pricing](https://getrun.id/pricing)

## Registry metadata

[`server.json`](./server.json) is the manifest published to the
[official MCP Registry](https://modelcontextprotocol.io/registry/about) under
the DNS-verified namespace `id.getrun`.

## Support

`support@getcore.id` — include the `task_id` or `request_id` when reporting a
failed generation.

Operated by PT Core Digital Asia (GetCore), Indonesia.
