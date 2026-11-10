# GoldAPI MCP — Precious Metals Quotes for AI Assistants

GoldAPI MCP is an MCP server that lets AI assistants (Claude, Cursor, WorkBuddy, Cline, etc.)
query real-time and historical precious metals quotes — gold, silver, platinum and palladium,
spot and futures across Shanghai Gold Exchange, NYMEX/COMEX and London markets.

- **Endpoint (Streamable HTTP):** `https://www.gold-api.cn/mcp`
- **Auth:** `Authorization: Bearer YOUR_APPKEY` (get a free key with 5,000 free calls at https://www.gold-api.cn/)
- **Docs:** https://www.gold-api.cn/docs.php (see the "MCP 接入" section)
- **Service status:** https://www.gold-api.cn/status.php

## Tools

| Tool | Description | Billing |
|---|---|---|
| `list_varieties` | List all listed varieties with `goldId`, name, market, category | Free |
| `get_realtime` | Real-time quotes (last price, change, bid/ask, high/low). Supports comma-separated `goldid`, up to 50 varieties per call | 1 call |
| `get_history` | Hourly OHLCV history, ascending, from 2026-05-27, max 92 days per request | 1 call |
| `get_kline` | Daily OHLC candles for the last N days (default 30, max 60) | Free |

Billing uses the same three-channel model as the REST API (per-variety / membership / free grant).
Every paid tool result includes a `billing` object with the channel and remaining quota,
so the assistant can report usage to the user.

## Install

### Claude Code

```bash
claude mcp add --transport http goldapi https://www.gold-api.cn/mcp \
  --header "Authorization: Bearer YOUR_APPKEY"
```

### Claude Desktop / Cursor / WorkBuddy (mcpServers JSON)

```json
{
  "mcpServers": {
    "goldapi": {
      "url": "https://www.gold-api.cn/mcp",
      "headers": { "Authorization": "Bearer YOUR_APPKEY" }
    }
  }
}
```

### Example prompts

- "用 GoldAPI 查一下黄金现货和白银 T+D 的最新价"
- "Show me the last 30 days of daily candles for gold spot (goldId 1061)."

## Notes

- Protocol versions supported: `2025-06-18`, `2025-03-26`, `2024-11-05` (stateless Streamable HTTP).
- Paid tools require a valid appkey; `get_realtime` supports querying up to 50 varieties in one call (counted as one call).
- Free-tier accounts get 5,000 free calls at registration: https://www.gold-api.cn/pricing.php
