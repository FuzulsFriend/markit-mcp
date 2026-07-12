# MarkIt MCP - connect your AI to your library

**Bring your own AI. MarkIt is the memory.**

[MarkIt](https://mark-it.co) saves the links, posts, and notes you'd otherwise lose, and makes them searchable in plain language. This repo is the official guide for connecting your own AI - Claude, ChatGPT, Cursor, and friends - to your MarkIt library over the open [Model Context Protocol](https://modelcontextprotocol.io).

Once connected, your AI can search your library, read what it finds, save new things into it, and set reminders. In your words, not keywords.

```
Server URL:  https://mark-it.co/api/mcp
Transport:   Streamable HTTP
Auth:        OAuth 2.1 (sign in) or a personal API key (mkt_...)
```

> Lazy path: paste this repo's URL to your AI and say "connect me to MarkIt". It will read [llms.txt](llms.txt) and walk you through the rest.

---

## Pick your path

**Option 1 - Sign in (OAuth).** No key to handle. Paste the server URL where your app asks for a custom connector or MCP server, and sign in to MarkIt when the browser opens. Best for Claude, ChatGPT web, Cursor, VS Code.

**Option 2 - API key.** Generate a key in [Settings > Integrations](https://mark-it.co/settings#integrations) (shown once, keep it secret). Best for CLIs, scripts, and the ChatGPT desktop app.

---

## Option 1 - sign in from your app

### One-click installs

| App | Install |
|---|---|
| Cursor | [Add to Cursor](https://cursor.com/install-mcp?name=markit&config=eyJ1cmwiOiJodHRwczovL21hcmstaXQuY28vYXBpL21jcCJ9) |
| VS Code | [Add to VS Code](https://vscode.dev/redirect/mcp/install?name=markit&config=%7B%22type%22%3A%22http%22%2C%22url%22%3A%22https%3A%2F%2Fmark-it.co%2Fapi%2Fmcp%22%7D) |

### Everyone else

| App | Steps |
|---|---|
| **Claude** (claude.ai + Claude Desktop) | Settings > Connectors (under Customize) > **Add custom connector** > paste `https://mark-it.co/api/mcp` > authorize. Available on all plans (Free allows one custom connector). **Known issue:** an [open Anthropic-side bug](https://github.com/anthropics/claude-ai-mcp/issues/582) can fail the authorization right after you approve - if that happens, use **Claude Code** (below) until Anthropic ships the fix. |
| **ChatGPT** (web) | Settings > **Security and login** > turn on **Developer mode**, then Settings > **Plugins** (or chatgpt.com/plugins) > **+** > paste the server URL > connect with OAuth. |
| **Claude Code** | `claude mcp add --transport http markit https://mark-it.co/api/mcp` then run `/mcp` in a session and pick **Authenticate**. Add `--scope user` to use it in every project. |
| **Codex CLI** | Add the server (see [clients/codex.toml](clients/codex.toml), skip the token line) then `codex mcp login markit`. |
| **Zed** | Add a `context_servers` entry with just the `url` (see [clients/zed.json](clients/zed.json)); Zed prompts you through OAuth when there is no Authorization header. |

## Option 2 - use your API key

1. In [MarkIt Settings > Integrations](https://mark-it.co/settings#integrations), open **Connect Your AI** and generate your key. It starts with `mkt_` and is shown **once**.
2. Grab your app's config from [`clients/`](clients/) and replace `mkt_your_key_here`:

| App | Config |
|---|---|
| Claude Code | [clients/claude-code.md](clients/claude-code.md) |
| Cursor | [clients/cursor.json](clients/cursor.json) |
| VS Code | [clients/vscode.json](clients/vscode.json) |
| Codex CLI + ChatGPT desktop app | [clients/codex.toml](clients/codex.toml) (they share `~/.codex/config.toml`) |
| Windsurf | [clients/windsurf.json](clients/windsurf.json) |
| Cline | [clients/cline.json](clients/cline.json) |
| Zed | [clients/zed.json](clients/zed.json) |
| Anything stdio-only | [clients/mcp-remote.json](clients/mcp-remote.json) |

Rules of the key: one per account, treat it like a password, never paste it into a chat, revoke or regenerate anytime in Settings.

---

## What your AI gets

| Tool | What it does |
|---|---|
| `search` | Plain-language search over your library, with optional filters (category, platform, type, author, tags, dates). |
| `fetch` | Full stored content of one item by id. |
| `list_categories` | Your categories with item counts. |
| `save_item` | Save a URL or a text note into your library. MarkIt scrapes, categorizes, and indexes it. Each save spends one monthly capture; re-saving a URL you already have is a free no-op. |
| `create_reminder` | Set or reschedule a reminder on an item (email for everyone; WhatsApp/Telegram on Pro). |
| `list_reminders` | Your pending reminders. |

What it can NOT do, by design: delete items, edit your account, touch anyone else's data. Saved page content is treated as data, never as instructions.

## Privacy, in one breath

Your AI talks to MarkIt with **your** identity and only ever sees **your** items. Whatever AI you connect (Claude, ChatGPT, ...) is your own tool under its own terms - MarkIt does not send your library anywhere on its own. OAuth grants are revocable from your AI app; the API key is revocable in Settings. Full details: [Privacy Policy](https://mark-it.co/privacy).

## Troubleshooting

- **401 right after connecting with a key**: the header must be exactly `Authorization: Bearer mkt_...` - check for a lost space or a truncated key (regenerating in Settings invalidates old keys).
- **Windsurf**: the remote-server field is `serverUrl`, not `url`.
- **Cline**: set `"type": "streamableHttp"` explicitly.
- **VS Code says it cannot start the server**: an entry with a `url` also needs `"type": "http"`.
- **Claude says "Authorization with MarkIt failed" right after you approve**: known [Anthropic-side bug](https://github.com/anthropics/claude-ai-mcp/issues/582) in claude.ai custom connectors (our server completes the whole OAuth flow; Claude drops it after the token). Use Claude Code or Cursor until it's fixed.
- **Sign-in window never comes back**: finish the login in the browser tab it opened, then retry from the app; some apps need a retry after the first authorize.
- **`mcp-remote` shim**: keep it current (`mcp-remote@latest`; versions before 0.1.16 have a known critical vulnerability). No space around the colon in `Authorization:${AUTH_HEADER}` on Windows.
- Still stuck? [support@mark-it.co](mailto:support@mark-it.co).

## For your AI

Point your assistant at [llms.txt](llms.txt) - a plain-text playbook that teaches it to connect you and to use the tools well.

## License

[MIT](LICENSE). The MarkIt name and logo belong to MarkIt.
