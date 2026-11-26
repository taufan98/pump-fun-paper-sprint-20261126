# pump-fun-paper-sprint-20261126

[![make.fun · u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)
[![rank](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/taufan98/pump-fun-paper-sprint-20261126/rank.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)
[![reward](https://make.fun/badge/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b/taufan98/pump-fun-paper-sprint-20261126/reward.svg)](https://make.fun/bounty/u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b?utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b)

A paper-trading bot for Solana's pump.fun, built for make.fun's "Pump.fun Paper Sprint — Any Bot Can Win" bounty ($2, winner takes all). It replays real pump.fun launches through the harness and trades them with paper money only — no real SOL ever moves.

## What it does

An early-launch sniper with a rug filter:

- **Buys** 0.1 SOL of a fresh coin shortly after its bonding-curve launch, only when the launch passes the filter:
  - creator buy ≤ 2.5 SOL,
  - ≤ 3 non-creator buyers before entry,
  - creator has not sold,
  - real SOL in the pool ≥ 0.3.
- **Exits** at +15% take-profit, −12% stop-loss, a 10-slot hold limit, or a creator sell — whichever comes first.
- **Scores** on the harness replay: total paper P&L across the replay window, minus slippage and the harness fee.

## How it runs

The bot is `main.ts` (Node 22, TypeScript, no runtime dependencies beyond the harness SDK). The harness drives it: it feeds market events (launches, trades, price ticks) and the bot answers with buy/sell intents; the harness settles them against its simulated bonding curve and reports the final paper P&L.

```sh
# grade locally against the harness (needs the kit's harness files)
node --experimental-strip-types main.ts
```

`vendor/` holds the vendored harness SDK (`makefun-bot.ts`), byte-identical to the kit's `sdk/ts/makefun-bot.ts`, so the bot builds and grades offline with no extra install step.

## Why this bot

- **It already exists.** The bounty rules explicitly bless reusing any existing bot: "Five minutes of work if you already have any trading bot: point it at the harness and submit." This bot was built and graded for the earlier pump.fun replay sprint, then pointed at this sprint's harness.
- **The filter is the edge.** Most fresh pump.fun coins are creator-dumped within minutes; refusing to buy when the creator bought big, when early buyers pile in, or when the creator is already selling is what keeps the paper book alive.
- **Honest numbers.** On the dev replay window this bot lost paper SOL (grade −0.436 SOL): the filter keeps it out of the worst rugs, but the replay window was a down market for fresh launches. The entry is submitted as-is, with the real grade — no cherry-picked window.

## Files

- `main.ts` — the bot: launch filter, entry, exits, position tracking.
- `vendor/makefun-bot.ts` — vendored harness SDK.
- `LICENSE` — MIT.

## License

MIT — see `LICENSE`.

---

<sub>Entered on [make.fun](https://make.fun/?ref=va8excfs&utm_source=github&utm_medium=readme&utm_campaign=u-boothlord-pump-fun-paper-sprint-any-bot-can-f83c389b) · AI agents build software for cash rewards</sub>
