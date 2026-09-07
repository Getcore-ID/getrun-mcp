# GetRun MCP Server

Remote [Model Context Protocol](https://modelcontextprotocol.io) server for
[GetRun](https://getrun.id) — generate images, video, audio and chat with
**61 AI models from 16 providers** from inside any MCP client.

```
https://run.getcore.id/mcp
```

No API key to paste. The server speaks OAuth 2.1 with dynamic client
registration, so a client is authorised once through a consent screen and
billed against the same Rupiah balance as the web app and the REST API.

| | |
|---|---|
| **Endpoint** | `https://run.getcore.id/mcp` |
| **Transport** | Streamable HTTP |
| **Auth** | OAuth 2.1 + dynamic client registration ([RFC 7591](https://datatracker.ietf.org/doc/html/rfc7591)) |
| **Protocol versions** | `2025-06-18`, `2025-03-26`, `2024-11-05` |
| **Scopes** | `read`, `generate`, `manage` |
| **Tools** | 14 |
| **Pricing** | Per use, in Indonesian Rupiah. No subscription. Failed tasks are not charged. |

This repository holds the connector's public metadata and setup guide. The
service itself is hosted — there is nothing to install and no server to run.

## Install

### Claude (claude.ai)

Settings → Connectors → **Add custom connector**. Name it `GetRun`, paste the
URL above, save, then press **Connect** and approve the permissions.

### Claude Code

```bash
claude mcp add --transport http getrun https://run.getcore.id/mcp
```

Then run `/mcp` inside a session, pick `getrun`, and complete the browser
consent flow once.

### Cursor

`~/.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "getrun": {
      "url": "https://run.getcore.id/mcp"
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
      "args": ["-y", "mcp-remote", "https://run.getcore.id/mcp"]
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
| `getrun_get_balance` | Available and reserved balance |
| `getrun_get_task` | Status and result of a running generation |
| `getrun_list_creations` | History with prompts, cost and output URLs |
| `getrun_hide_creation` | Tidy the gallery; billing history is kept |
| `getrun_list_characters` | Saved characters — a consistent face across generations |
| `getrun_save_character` | Create or update a character |
| `getrun_delete_character` | Delete a character; past generations remain |

## Catalogue

61 models from 16 providers, as of September 2026:

| Category | Models | Examples |
|---|---|---|
| Video | 27 | Seedance 2.0, Kling 3.0, PixVerse V6, MiniMax H3 |
| Image | 14 | Nano Banana 2, Seedream 5.0 Pro, GPT Image 2, Qwen3 |
| Text & chat | 14 | Claude Opus 4.8, Claude Sonnet 5, GPT-5.6, Grok 4.6 |
| Audio | 6 | Suno V5, ElevenLabs TTS / Music / Sound Effects |

The live list is always at [`GET /v1/models`](https://getrun.id/v1/models) —
this table is a snapshot, that endpoint is the source of truth.

## Billing

Identical to the REST API: every tool calls `/v1` in the same process, so
pricing, balance reservation, schema validation and rate limits are the same,
and results appear in the dashboard's usage history.

Generation tools spend real balance. `getrun_estimate_price` returns the cost
of a call before you make it, and a task that fails is never charged.

## Also available

- [REST API documentation](https://getrun.id/docs) — one `POST /v1/tasks` for every model
- [`llms.txt`](https://getrun.id/llms.txt) — the whole API as compact markdown, for coding agents
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
