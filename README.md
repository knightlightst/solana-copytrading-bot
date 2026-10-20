# Solana CopyTrading Bot

Copytrading bot for Solana, written in C#. Watches selected wallets and mirrors their swaps on Raydium and Pump.fun. Uses public RPC nodes — monitoring mode doesn't need any private keys.

![](https://github.com/knightlightst/solana-copytrading-bot/blob/main/Untitled.png?raw=true)

## Features

- Real-time monitoring of watched wallets (websocket subscriptions, not polling)
- Trade filters: swaps, transfers, token launches, liquidity add/remove
- Slippage control and per-trade / daily loss limits
- Optional sniping mode for new Pump.fun launches
- Auto-copy execution via Raydium (Jupiter routing is in the roadmap, see #2)
- Telegram notifications for every copied trade

## Status

v0.2.1 — monitoring and Raydium execution work. Pump.fun execution is flaky (#4). Moonshot/GMGN support planned, not started.

## Getting started (Windows)

1. Download the latest build from [Releases](https://github.com/knightlightst/solana-copytrading-bot/releases) (`copybot-v0.2.1-win-x64.rar`)
2. Extract archive
3. Copy `appsettings.example.json` to `appsettings.json` and edit it
4. Run `CopyBot.exe`

The default config runs in **read-only monitoring mode**: no keys required, the bot just watches the example wallets and logs what it would copy. Trading stays off until you set `"execution.enabled": true` yourself.

If you ever enable execution — use a fresh burner wallet with a small balance. Never paste keys from your main wallets into any bot config, including this one.

## Building from source

Requires .NET 8 SDK:

```
dotnet build -c Release
```

The binary ends up in `bin/Release/net8.0-windows/`.

## Configuration

Everything is in `appsettings.json`. Main options:

```json
{
  "rpcEndpoint": "https://api.mainnet-beta.solana.com",
  "watchedWallets": [
    "8wJ3kPzR7vQmT2nLdXcB4yHsFgU6aE9KpM5tWrYuVqN8",
    "Gk9mRt2vXw7LpQ4nHsYdF8cB3jAeU6zT5iKoP1xMvNwE"
  ],
  "filters": {
    "swaps": true,
    "transfers": false,
    "tokenLaunches": true,
    "minTradeSizeUsd": 500
  },
  "execution": {
    "enabled": false,
    "maxSlippagePct": 2.5,
    "maxSolPerTrade": 0.5,
    "dailyLossLimitSol": 2
  },
  "telegram": {
    "botToken": "",
    "chatId": ""
  }
}
```

For `watchedWallets` just grab any top performers from GMGN or Solscan — that's what the two example addresses are.

## Known issues

- Pump.fun execution sometimes misses the bonding curve state (#4)
- High CPU for ~2 minutes on first start while it backfills wallet history — normal
- Telegram alerts can arrive out of order when trades come in fast

## Changelog

**v0.2.1**
- fixed crash on empty `watchedWallets` (#3)
- slippage is now in percent, was a fraction (sorry if that ate a trade)

**v0.2.0**
- Raydium execution
- Telegram alerts

**v0.1.0**
- monitoring only

## Disclaimer

Trading bots lose money. Use a burner, start small, don't trade what you can't lose.

## Contact

Bugs and ideas — open an [Issue](https://github.com/knightlightst/solana-copytrading-bot/issues). If you want to contribute, open an issue first so we don't duplicate work.
