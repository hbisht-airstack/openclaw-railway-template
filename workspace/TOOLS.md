# TOOLS.md — Local Notes

This is your cheat sheet. Environment-specific stuff that doesn't belong in skills.

## Senpi MCP

- **Server name:** `senpi`
- **Auth:** JWT token (configured at setup)
- **Connection:** Pre-configured via OpenClaw, no manual setup needed
- The MCP server provides its own instructions and tool descriptions — read them at runtime

## Telegram

- **Numeric chat IDs only** — `@username` does NOT work
- Target format: `telegram:<chat_id>` (e.g. `telegram:123456789`)
- Check `USER.md` for the user's chat ID

## Shell tools

- `rg` (ripgrep) — recursive by default, do NOT pass `-R` or `-r`
- `node` — use `node -e` for JSON processing
- `python3` — available for scripting
- `grep` — fallback if needed
- **NOT installed:** `jq` — use `node -e` instead

## Token Refresh

If Senpi calls fail with an auth error, the token has expired. Tell the user to provide a fresh token, then run:
```bash
curl -s -X POST http://127.0.0.1:8080/setup/api/senpi-token \
  -H "Content-Type: application/json" \
  -d '{"token": "NEW_TOKEN"}'
```
This updates the config and restarts the MCP connection.

---

## Senpi Domain Quick Reference

### What is Senpi

Senpi is a trading platform built on **Hyperliquid** — a high-performance Layer 1 perpetual futures DEX. Users can:

- **Copy trade** (mirror) profitable traders automatically in real-time
- **Run custom strategies** with manual control over positions, leverage, and direction
- **Discover traders** via leaderboards, historical analytics, and behavioral classification

### Wallets & Fund Flow

Every user has **one main wallet** from which stratigies are funded. Each strategy gets its own isolated **sub-wallet**.

```
Deposit → Main Wallet → Strategy Sub-Wallet → Trading → Close Strategy → Main Wallet → Withdraw
```

- Creating a strategy moves funds from main wallet → sub-wallet
- Closing a strategy returns remaining funds → main wallet
- Strategies never trade from the main wallet directly
- You can run multiple strategies simultaneously (even opposing positions across strategies)

### Strategies

A **strategy** is Senpi's core unit of trading — a managed sub-wallet with a budget and configuration.

**Mirror (Copy Trading):** Automatically replicates another trader's positions. Follows an "OG" trader; positions open/close/resize automatically.

**Custom:** User manually manages their own positions with full control. No OG trader.

**Lifecycle:** Create → Active → (optional: top-up, close positions) → Close strategy (irreversible, returns funds to main wallet)

### Positions

Positions live inside strategies. Each is an open perpetual futures trade on Hyperliquid.

- **TP/SL** triggers on gross % of funded amount (before fees), closes the entire position (no partial close)
- Can hold multiple positions in different assets within one strategy
- Cannot hold the same asset at different direction/leverage within a single strategy
- CAN have opposing positions on the same asset across different strategies

### Common Operations

**Research a trader to copy:**
Discovery (find candidates by track record) → check their current positions → Leaderboard (confirm current momentum) → check your available capital → preview strategy creation (dry run) → execute

**Check what's hot right now:**
Leaderboard top performers → drill into a specific trader's momentum → validate with Discovery history → act if warranted

**Monitor portfolio:**
Portfolio overview → list active strategies → check individual strategy performance → review recent audit trail

**Adjust a strategy:**
Get current strategy state → review decision history → preview update (dry run) → execute with reason

### Discovery vs Leaderboard

| Intent | Use | Why |
|---|---|---|
| "Find good traders to copy" | **Discovery** | Historical track record, consistency labels |
| "Who's hot right now?" | **Leaderboard** | 4-hour rolling window momentum |
| "Is this trader reliable?" | **Discovery** | Long-term history + behavioral labels |
| "Is this trader on a streak?" | **Leaderboard** | Current momentum + tier events |
| "What should I copy?" | **Both** | Discovery to select, Leaderboard to time |

**Rule:** "Is this trader *good*?" → Discovery. "Is this trader *hot right now*?" → Leaderboard.

### Key Concepts

- **OG** — Original trader being copied in a mirror strategy
- **Mirror multiplier** — 0.1–10x scaling of position sizes relative to OG (default 1x)
- **Slippage (Senpi)** — Distance from OG's entry price to your fill price when copying (NOT order book slippage). Default tolerance: 1%, range: 0.1–5%
- **Dry run** — Preview mode. All mutation actions support it. Always preview first.
- **Reason** — Free-text field logged with every mutation for audit trail. Always include one.
- **Fee drag** — ~0.19% round-trip (Hyperliquid + builder fee). At typical leverage, ~2.3% of funded amount. TP at 10% gross ≈ 7.7% net.
- **Sub-wallet** — Each strategy gets its own isolated wallet (separate from main wallet)
- **Cross-margin** — Default mode. All positions in a strategy share margin.

### Trader Labels (from Discovery)

- **Activity:** DEGEN (≥75 TAS) / ACTIVE (≥60) / TACTICAL (35–60) / PATIENT (<35)
- **Consistency:** ELITE / RELIABLE / STREAKY / CHOPPY
- **Risk:** CONSERVATIVE / BALANCED / AGGRESSIVE / SNIPER

### Momentum Tiers (from Leaderboard)

- **Tier 1** — Lowest threshold, most frequent (early signal)
- **Tier 2** — Medium threshold (confirmed momentum)
- **Tier 3** — Highest threshold, rare (strong signal)

### Gotchas

- **TP/SL triggers on gross %** — net will be lower by ~2.3% fee drag
- **No partial close** — TP/SL closes the entire position
- **Strategy close is irreversible** — use "close positions" to keep strategy alive
- **Discovery data lags 24–48h** — not real-time for historical rankings
- **Leaderboard window = 4 hours rolling** — rankings change fast
- **Filter arrays are AND conditions** — CONSERVATIVE + DEGEN may return zero results
