<p align="center">
  <img src="assets/banner.png" alt="MarkIt - everything you save, findable again" width="100%" />
</p>

# MarkIt MCP - connect your AI to your library

**Bring your own AI. MarkIt is the memory.**

> [!TIP]
> **The lazy path (recommended):** paste this repo's URL into your AI and say **"connect me to MarkIt"**. It reads [llms.txt](llms.txt) and handles everything below for you.

[MarkIt](https://mark-it.co) saves the links, posts, and notes you'd otherwise lose, and makes them searchable in plain language. This repo is the official guide for connecting your own AI - Claude, ChatGPT, Cursor, and friends - to your MarkIt library over the open [Model Context Protocol](https://modelcontextprotocol.io).

Once connected, your AI can search your library, read what it finds, save new things into it, and set reminders. In your words, not keywords.

```
Server URL:  https://mark-it.co/api/mcp
Transport:   Streamable HTTP
Auth:        OAuth 2.1 (sign in) or a personal API key (mkt_...)
```

---

## Pick your path

**Option 1 - Sign in (OAuth).** No key to handle. Works with **Claude, ChatGPT web and Claude Code** only: MarkIt refuses sign-in from any other app, so a look-alike app cannot trick you into signing in.

**Option 2 - API key.** Generate a key in [Settings > Integrations](https://mark-it.co/settings#integrations) (shown once, keep it secret). For every other app: Cursor, VS Code, Codex CLI, Windsurf, Cline, Zed, scripts, and the ChatGPT desktop app.

[![Listed on mcpservers.org](https://mcpservers.org/badge.svg)](https://mcpservers.org/servers/fuzulsfriend/markit-mcp)

[![MarkIt MCP server](https://mcprush.com/badge/markit-mcp/card.svg)](https://mcprush.com/to/markit-mcp?ref=badge)

---

## Option 1 - sign in from your app

| App | Steps |
|---|---|
| **Claude** (claude.ai + Claude Desktop) | Settings > Connectors (under Customize) > **Add custom connector** > paste `https://mark-it.co/api/mcp` > **Connect** > sign in to MarkIt > **Allow**. Available on all plans (Free allows one custom connector). |
| **ChatGPT** (web) | [chatgpt.com/plugins](https://chatgpt.com/plugins) > **+** > **Add custom MCP server**. **Icon:** upload the [MarkIt icon](assets/markit-icon-256.png) (optional, keeps the MarkIt logo in your plugin list instead of a puzzle piece). **Name:** `MarkIt`. **Server URL:** `https://mark-it.co/api/mcp`. **Authentication:** OAuth. Tick **I understand and want to continue** > **Create as a plugin** > sign in to MarkIt > **Allow**. |
| **Claude Code** | `claude mcp add --transport http markit https://mark-it.co/api/mcp` then run `/mcp` in a session and pick **Authenticate**. Add `--scope user` to use it in every project. |

Any other app: use Option 2.

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
| `cancel_reminder` | Cancel one reminder by id. Never touches the item itself; Google-Calendar-synced reminders stay managed on-site. |
| `send_feedback` | Send a short note straight to MarkIt's founder - only when you ask it to. A real person reads every one. Max 3 a day. |

What it can NOT do, by design: delete items, edit your account, touch anyone else's data. Saved page content is treated as data, never as instructions.

## Privacy, in one breath

Your AI talks to MarkIt with **your** identity and only ever sees **your** items. Whatever AI you connect (Claude, ChatGPT, ...) is your own tool under its own terms - MarkIt does not send your library anywhere on its own. OAuth grants are revocable from your AI app; the API key is revocable in Settings. Full details: [Privacy Policy](https://mark-it.co/privacy).

## Troubleshooting

- **401 right after connecting with a key**: the header must be exactly `Authorization: Bearer mkt_...` - check for a lost space or a truncated key (regenerating in Settings invalidates old keys).
- **Windsurf**: the remote-server field is `serverUrl`, not `url`.
- **Cline**: set `"type": "streamableHttp"` explicitly.
- **VS Code says it cannot start the server**: an entry with a `url` also needs `"type": "http"`.
- **Claude says "Authorization with MarkIt failed" right after you approve**: retry once. If it fails again, use Claude Code (Option 1) or any app with an API key (Option 2). An older [Anthropic-side bug](https://github.com/anthropics/claude-ai-mcp/issues/582) caused this; claude.ai sign-in worked again in our test on 2026-10-21.
- **"This app is not supported. Use an API key instead."**: sign-in is limited to Claude, ChatGPT web and Claude Code. Use an API key (Option 2).
- **Sign-in window never comes back**: finish the login in the browser tab it opened, then retry from the app; some apps need a retry after the first authorize.
- **`mcp-remote` shim**: keep it current (`mcp-remote@latest`; versions before 0.1.16 have a known critical vulnerability). No space around the colon in `Authorization:${AUTH_HEADER}` on Windows.
- Still stuck? [support@mark-it.co](mailto:support@mark-it.co).

## For your AI

Point your assistant at [llms.txt](llms.txt) - a plain-text playbook that teaches it to connect you and to use the tools well. Same file, branded URL: [mark-it.co/mcp/llms.txt](https://mark-it.co/mcp/llms.txt).

Claude Code users can go one step further: copy the [`skill/`](skill/) folder into `.claude/skills/markit/` and Claude picks up the full MarkIt usage playbook (when to search vs save, note etiquette, reminder handling) automatically.

## License

[MIT](LICENSE). The MarkIt name and logo belong to MarkIt.
