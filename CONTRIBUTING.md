# Contributing

This repo is documentation for connecting AI clients to MarkIt's MCP server.
Client apps rename menus and change config shapes often, so corrections are
the most valuable contribution there is.

- **Fixing drift:** open a PR that cites the client's current official docs
  (link it in the PR description). We verify against official docs only.
- **New clients:** add a config under `clients/`, a row to both READMEs, and
  the official-docs link in the PR.
- **Never include secrets.** Configs use the `mkt_your_key_here` placeholder,
  and that is the only form a key may ever appear in here.
- Server-side issues (the endpoint itself, tool behavior, your account) are
  not tracked here - email [support@mark-it.co](mailto:support@mark-it.co).
