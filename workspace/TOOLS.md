# TOOLS.md - Local Notes

Skills define _how_ tools work. This file is for _your_ specifics — the stuff that's unique to your setup.

## Senpi MCP
- MCP server name: senpi (via mcporter)
- Always discover tools at runtime (list tools) and cache:
  - user profile tool (for display name)
  - trading context tool (product/rules)
  - operations tool (capabilities list)
- Never assume capability names; always read from MCP.

## mcporter (Senpi MCP CLI)

mcporter is pre-installed globally. The `senpi` MCP server is already configured.

### Quick reference

```bash
# List configured servers (plain text output, NOT JSON)
mcporter list

# Call a tool on the senpi server (returns JSON)
mcporter call senpi.<tool_name> [--params '{"key": "value"}']

# Examples:
mcporter call senpi.user_get_me
mcporter call senpi.account_get_portfolio
mcporter call senpi.market_get_prices --params '{"assets": ["BTC", "ETH"]}'
mcporter call senpi.strategy_list --params '{"status": "ACTIVE"}'
mcporter call senpi.discovery_get_top_traders --params '{"time_frame": "MONTHLY", "sort_by": "RETURN_ON_INVESTMENT", "limit": 10}'
```

### Important notes

- `mcporter list` outputs **plain text**, not JSON. Do not try to `JSON.parse()` it.
- `mcporter call` outputs **JSON**. Parse this normally.
- Tool names use the format `senpi.<tool_name>` (dot-separated server and tool).
- If a call fails with an auth error, the `SENPI_AUTH_TOKEN` may have expired.
  Ask the user for a fresh token and update it via:
  `curl -s -X POST http://127.0.0.1:8080/setup/api/senpi-token -H "Content-Type: application/json" -d '{"token": "NEW_TOKEN"}'`

## Telegram messaging

- Use numeric chat IDs, not @usernames. Check `USER.md` for the user's chat ID.
- Target format: `telegram:<chat_id>` (e.g. `telegram:123456789`).

## Shell tools

- `rg` (ripgrep) is available for fast file search. It is recursive by default — do NOT pass `-R` or `-r`.
- `node` is available for JSON processing. Use `node -e` for any JSON parsing or data transformation.
- `grep` is available as a fallback if needed.

### NOT installed (do not use)

- `jq` — NOT available. Use `node -e` instead for JSON processing.

### Also available

- `python` / `python3` — available for scripting if needed. Prefer `node -e` for simple JSON tasks.


## Why Separate?

Skills are shared. Your setup is yours. Keeping them apart means you can update skills without losing your notes, and share skills without leaking your infrastructure.

---

Add whatever helps you do your job. This is your cheat sheet.