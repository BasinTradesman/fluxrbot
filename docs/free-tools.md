# Free tools

Seven browser calculators on [fluxrbot.com/tools](https://fluxrbot.com/tools/?utm_source=github&utm_medium=docs).
No account, no card, nothing to install. They are the same arithmetic the
engine runs before a paper trade, exposed one step at a time so you can check
its work by hand.

| Tool | What it answers |
|---|---|
| [Kelly calculator](https://fluxrbot.com/tools/kelly-calculator/?utm_source=github&utm_medium=docs) | Position size for a Polymarket or Kalshi contract from your probability estimate and the market price. Full, half and quarter Kelly. |
| [Odds converter](https://fluxrbot.com/tools/odds-converter/?utm_source=github&utm_medium=docs) | Implied probability, American, decimal and fractional odds, and contract price in cents, converted both ways. |
| [Payout calculator](https://fluxrbot.com/tools/payout-calculator/?utm_source=github&utm_medium=docs) | Payout, return and break-even probability for a contract, with Kalshi's taker and maker fee formula applied. |
| [Arbitrage checker](https://fluxrbot.com/tools/arbitrage-checker/?utm_source=github&utm_medium=docs) | Whether a price gap between Polymarket and Kalshi survives fees: locked spread, return, and the smallest gap worth acting on. |
| [Live radar](https://fluxrbot.com/tools/live-radar/?utm_source=github&utm_medium=docs) | Biggest 24-hour movers, highest-volume contracts and near-resolved markets across both venues. Rebuilt hourly. |
| [Conditional chain probability](https://fluxrbot.com/tools/conditional-chain-probability/?utm_source=github&utm_medium=docs) | Joint probability of a multi-step event and the fair contract price in cents, from the probability of each step. |
| [Crosstab to topline](https://fluxrbot.com/tools/crosstab-to-topline-calculator/?utm_source=github&utm_medium=docs) | Turnout-weighted topline margin and an implied win-probability range from subgroup margins and turnout shares. |

## How they relate to the app

The [payout calculator](https://fluxrbot.com/tools/payout-calculator/?utm_source=github&utm_medium=docs)
uses the same 2026 fee schedules as the engine (Kalshi 7% × p × (1−p) taker,
makers free). The [arbitrage checker](https://fluxrbot.com/tools/arbitrage-checker/?utm_source=github&utm_medium=docs)
is the single-pair version of the check the engine runs on every basket
before it records `arb_edge_below` or `arb_too_good` in the
[refusal journal](refusal-reasons.md). The
[Kelly calculator](https://fluxrbot.com/tools/kelly-calculator/?utm_source=github&utm_medium=docs)
is the sizing rule described in [risk.md](risk.md), which the engine only
switches on after calibration has enough settled outcomes behind it.

Found a wrong number? Open a thread in
[Discussions](https://github.com/BasinTradesman/fluxrbot/discussions) or write
to hello@fluxrbot.com.

---

[← All documentation](https://fluxrbot.com/docs/?utm_source=github&utm_medium=docs) · [Tools on the site](https://fluxrbot.com/tools/?utm_source=github&utm_medium=docs) · [fluxrbot.com](https://fluxrbot.com/?utm_source=github&utm_medium=docs)
