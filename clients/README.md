# Ready-to-copy client configs

Every file here is copy-paste ready with one edit: replace `mkt_your_key_here`
with your real key from [MarkIt Settings > Integrations](https://mark-it.co/settings#integrations).
Prefer signing in (OAuth) instead of a key where your app supports it - see the
[main README](../README.md#option-1---sign-in-from-your-app).

| File | Goes to | Notes |
|---|---|---|
| [claude-code.md](claude-code.md) | terminal commands | OAuth and key variants |
| [cursor.json](cursor.json) | `~/.cursor/mcp.json` (global) or `.cursor/mcp.json` (project) | drop the `headers` block to use OAuth instead |
| [vscode.json](vscode.json) | `.vscode/mcp.json` | prompts for the key on first start (never hardcoded) |
| [codex.toml](codex.toml) | `~/.codex/config.toml` | shared by Codex CLI and the ChatGPT desktop app |
| [windsurf.json](windsurf.json) | `~/.codeium/windsurf/mcp_config.json` | note the field is `serverUrl` |
| [cline.json](cline.json) | `~/.cline/mcp.json` or the Remote Servers tab | `type: streamableHttp` is required |
| [zed.json](zed.json) | merge into Zed `settings.json` | omit `headers` and Zed walks you through OAuth |
| [mcp-remote.json](mcp-remote.json) | any stdio-only client | keep `mcp-remote` at `@latest` |

Never commit a real key to any repo, and never paste one into a chat.
