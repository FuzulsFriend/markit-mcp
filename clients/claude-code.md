# Claude Code

## Sign in (OAuth, no key)

```bash
claude mcp add --transport http markit https://mark-it.co/api/mcp
```

Then inside a session run `/mcp`, pick **markit**, and choose **Authenticate**.

## API key

```bash
claude mcp add --transport http markit https://mark-it.co/api/mcp \
  --header "Authorization: Bearer mkt_your_key_here"
```

Add `--scope user` to either command to make MarkIt available in every
project instead of just the current one.

Get your key (shown once) in
[MarkIt Settings > Integrations](https://mark-it.co/settings#integrations).
