# engram-codex-plugin

Codex plugin for [Engram](https://lumetra.io) — persistent, explainable memory for AI agents.

Wires Codex into the hosted Engram MCP server. The agent gets `store_memory`, `query_memory`, `list_memories`, and the rest of the Engram tool surface with no HTTP plumbing on your side.

## Install

Codex plugins are vendored into your own repo. The marketplace entry points at a local plugin folder, so the install is two steps:

### 1. Copy the plugin folder into your repo

```bash
mkdir -p plugins
curl -fsSL https://codeload.github.com/lumetra-io/engram-codex-plugin/tar.gz/refs/heads/main \
  | tar -xz --strip-components=2 -C plugins engram-codex-plugin-main/plugins/engram
```

(Or just `git clone` this repo and copy `plugins/engram/` over manually.)

After the copy, your repo has:

```
plugins/engram/
├── .codex-plugin/plugin.json   # the Codex manifest
├── .codex-mcp.json             # MCP server config (referenced from plugin.json)
└── README.md
```

### 2. Register it in `.agents/plugins/marketplace.json`

Add this entry to your repo's `.agents/plugins/marketplace.json` (create the file if it doesn't exist):

```json
{
  "name": "engram-plugins",
  "interface": { "displayName": "Engram Plugins" },
  "plugins": [
    {
      "name": "engram",
      "source": { "source": "local", "path": "./plugins/engram" },
      "policy": { "installation": "AVAILABLE", "authentication": "ON_INSTALL" },
      "category": "Productivity"
    }
  ]
}
```

Open the Codex UI, find Engram in the plugin list, and install. Codex prompts for `ENGRAM_API_KEY` once (per `authentication: ON_INSTALL`) and stores it in its secret store; the MCP server entry's `bearer_token_env_var: "ENGRAM_API_KEY"` field tells Codex to forward it as `Authorization: Bearer <key>` on every MCP request.

> **Repo-scoped vs personal install.** The path above is repo-scoped (`<repo-root>/.agents/plugins/marketplace.json`). For a personal-scoped install that follows you across repos, use `~/.agents/plugins/marketplace.json` instead.

## What you need before installing

- An [Engram account](https://lumetra.io) and an API key (`eng_live_...`).
- A BYOK provider key configured on the [models page](https://lumetra.io/models). Engram is bring-your-own-key end-to-end — without one, every `store_memory` / `query_memory` returns HTTP 412.

## Tools exposed

After install your agent can call:

| Tool | What it does |
|---|---|
| `store_memory(content, bucket?)` | Save a fact to a bucket |
| `query_memory(question, bucket?)` | Hybrid retrieval + synthesized answer |
| `list_memories(bucket, limit?)` | Paginated list |
| `list_buckets()` | All buckets in your tenant |
| `delete_memory(memory_id, bucket)` | Remove a single memory |
| `clear_memories(bucket)` | Empty a bucket |

Same surface as the [Claude Code plugin](https://github.com/lumetra-io/engram-plugin) and the official [TypeScript](https://github.com/lumetra-io/engram-js), [Python](https://github.com/lumetra-io/engram-py), and [Go](https://github.com/lumetra-io/engram-go) clients.

## Repository layout

This repo also keeps a sample `.agents/plugins/marketplace.json` at the root showing the exact entry you'd add — useful as a reference and as a smoke-test target when developing the plugin locally.

## License

MIT — see [LICENSE](./LICENSE).
