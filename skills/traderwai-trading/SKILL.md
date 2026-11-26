---
name: traderwai-trading
description: Use TraderWAI to read live market charts and OHLC data, backtest a trading rule on 1-minute history, and place or propose orders with stop loss and take profit on the user's own broker account. Use when the user asks about markets (forex, gold, indices, oil, futures, crypto), wants a chart or levels, a backtest, or to trade.
---

# TraderWAI

TraderWAI is a hosted MCP server (`https://mcp.traderwai.com`). The first time a tool is used, your agent (Cursor, Grok Bot, Grok Build…) opens a sign-in page: the user signs in with Google or Microsoft. Charts, market data and analysis are free and unlimited; acting on an account has plan limits.

## Analyse
- `list_symbols` for the exact symbol ids (e.g. `XAU_USD`, `EUR_USD`, `NAS100_USD`, `BTC_USD`).
- `get_chart` to see the market (higher timeframe first, then the entry timeframe); `get_ohlc` for exact prices and candle times; `get_quote` for the latest price.
- `get_market_structure` for swings, the last BOS / CHoCH, trend by structure and live zones (order blocks, FVGs, Fibonacci 0.5-0.618, equal highs/lows) at a given time, exactly as the rule engine sees them.
- `draw_chart` to show the user the analysis on a real chart (levels, zones, trendlines, the trade idea). Never generate chart images yourself.

## Backtest
- When the user asks how a rule would have worked, use `backtest_strategy` (do not compute backtests yourself from candles). Explain the result plainly: a handful of trades proves little, and past results do not guarantee future results. If the rule uses structure, check it with `get_market_structure` first.

## Trade
- `get_broker_accounts` and `get_account_info` first. If no broker is connected, tell the user to connect one at https://traderwai.com/en/app/settings?tab=brokers.
- Never place, modify or close an order without the user's explicit confirmation of symbol, side, size, stop loss and take profit. Suggest a demo account first.
- Prefer orders sized by risk (e.g. "0.5% risk, stop below the last swing low, target 2R"); always include a stop loss.
- If you are not allowed to place trades automatically, use `suggest_order`: the user authorizes it in TraderWAI.
- Conditional orders (`create_conditional_order`) watch a rule every minute even when the chat is closed; they use the same rules as `backtest_strategy`, so backtest a rule before turning it into a conditional order. Exit rules (break-even, close on a CHoCH) work on open positions.
- `get_risk_limits` shows the account's risk limits; TraderWAI enforces them, not the AI.
- When a tool returns `PLAN_LIMIT`, do not retry: explain the limit and, if the user wants more, `get_upgrade_url` gives the link.

Trading involves risk. TraderWAI is a tool, not investment advice.
