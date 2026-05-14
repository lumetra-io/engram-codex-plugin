# engram (Codex plugin)

Persistent, explainable memory for Codex via the Engram MCP server.

The plugin wires Codex into the hosted Engram MCP endpoint at `https://mcp.lumetra.io/mcp/sse`. Codex prompts for `ENGRAM_API_KEY` on install (`policy.authentication: ON_INSTALL`) and stores it in its secret store; the plugin then exposes Engram's memory tools — `store_memory`, `query_memory`, `list_memories`, `list_buckets`, `delete_memory`, `clear_memories` — to your agent.

## Get an API key

1. Sign up at [lumetra.io](https://lumetra.io).
2. Create an API key in the dashboard. It looks like `eng_live_...`.
3. Configure your LLM provider key (OpenAI / Anthropic / Groq / Together / Fireworks) on the [models page](https://lumetra.io/models) — Engram is bring-your-own-key end-to-end. Skipping this step makes every `store_memory` / `query_memory` call return 412.

## Install

See the top-level [README](../../README.md) for the marketplace install.
