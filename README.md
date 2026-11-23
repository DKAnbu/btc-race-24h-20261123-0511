# BTC Race Bot — Dip-Bounce Mean-Reversion, 60-Bar Mean, +220/−80 bps Exits

[![make.fun](https://make.fun/badge/btc-race-24h-20261123-0511.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511)
[![rank](https://make.fun/badge/btc-race-24h-20261123-0511/DKAnbu/btc-race-24h-20261123-0511/rank.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511)
[![reward](https://make.fun/badge/btc-race-24h-20261123-0511/DKAnbu/btc-race-24h-20261123-0511/reward.svg)](https://make.fun/bounty/btc-race-24h-20261123-0511)

Entry for [Race a BTC trading bot live for 24 hours](https://make.fun/bounty/btc-race-24h-20261123-0511)
(`btc-race-24h-20261123-0511`). A long-only cbBTC/USDC bot that speaks
`agent-fun/bars-replay/v1` over NDJSON stdin/stdout: it reads the `hello`,
answers every `batch` with exactly one `reply` carrying its `orders`, and
trades one bar at a time.

## Strategy

Dip-bounce mean-reversion, full-size entries, one position at a time:

- **Entry:** buy when the latest close sits at least **60 bps below its
  60-bar mean** (or at least 80 bps below its 60-bar high) **and** the
  close has turned up for two bars in a row — the two-bar rise confirms
  the dip is bouncing instead of still falling.
- **Exit:** sell at **+220 bps** over the entry fill, on a **−80 bps**
  stop, or on a 100 bps pullback from the peak since entry once at least
  +20 bps in profit (trailing exit).
- **Guards:** no trading during warm-up bars; no new entries in the last
  180 bars of the window.

## Local dev-window result

Graded locally with the kit's `grade-local` on the published 1,440-bar dev
window (24h ending at the competition opening):

- **Net PnL: +$24.83 USDC** (3 filled round trips)
- **Max drawdown: 1.77%** (limit 25%)
- Return: +2.48%

The same parameters earn about +$20.01 on the previous competition's
window (18 Nov 2026, a falling day, BTC −280 bps), so the result is not a
one-window spike: neighbouring parameter values give the same trades.

## Run

```sh
python3 main.py   # reads hello/batch lines on stdin, writes replies on stdout
```

Python 3.13, standard library only, no network. Fully deterministic: no
wall clock, no environment, no unseeded randomness.

## License

MIT — see [LICENSE](LICENSE). `SPDX-License-Identifier: MIT`
