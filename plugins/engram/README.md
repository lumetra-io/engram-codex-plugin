# engram (Codex plugin)

Persistent, explainable memory for Codex via the hosted Engram MCP server.

The plugin wires Codex into `https://mcp.lumetra.io/mcp/sse`. Codex prompts for `ENGRAM_API_KEY` on install (`policy.authentication: ON_INSTALL`) and forwards it as a Bearer token to the MCP endpoint via the `bearer_token_env_var` mechanism. Your agent then has access to Engram's memory tools — `store_memory`, `query_memory`, `list_memories`, `list_buckets`, `delete_memory`, `clear_memories`.

## Folder layout

```
plugins/engram/
├── .codex-plugin/
│   └── plugin.json          # Codex plugin manifest
├── .codex-mcp.json          # MCP server config (referenced from plugin.json)
└── README.md
```

## Get an API key

1. Sign up at [lumetra.io](https://lumetra.io).
2. Create an API key in the dashboard. It looks like `eng_live_...`.
3. Configure your LLM provider key (OpenAI / Anthropic / Groq / Together / Fireworks) on the [models page](https://lumetra.io/models). Engram is bring-your-own-key end-to-end — skip this and every `store_memory` / `query_memory` call returns HTTP 412.

## Install

See the top-level [README](../../README.md) for the marketplace install steps.
