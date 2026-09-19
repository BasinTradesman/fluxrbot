<p align="center">
  <a href="https://fluxrbot.com/?utm_source=github&utm_medium=banner"><img src="https://fluxrbot.com/img/x-banner-fluxrbot-2026.png" alt="FluxrBot: desktop trading bot for Polymarket and Kalshi" width="100%"></a>
</p>

<h1 align="center">FluxrBot</h1>

<p align="center"><b>A Windows desktop bot for Polymarket and Kalshi that reads the news, applies the rules you set, and shows every decision it makes, including the trades it refused.</b></p>

<p align="center">
  <a href="https://github.com/BasinTradesman/fluxrbot/releases/latest"><img src="https://img.shields.io/github/v/release/BasinTradesman/fluxrbot?style=flat&color=FFB224&labelColor=0F0D12&label=release" alt="Latest release"></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/docs%20license-CC%20BY%204.0-FFB224?style=flat&labelColor=0F0D12" alt="Documentation licensed CC BY 4.0"></a>
  <a href="https://fluxrbot.com/?utm_source=github&utm_medium=badge"><img src="https://img.shields.io/website?url=https%3A%2F%2Ffluxrbot.com&style=flat&label=fluxrbot.com&up_color=FFB224&labelColor=0F0D12" alt="fluxrbot.com"></a>
  <a href="https://x.com/fluxrbot"><img src="https://img.shields.io/badge/follow-%40fluxrbot-0F0D12?style=flat&logo=x&logoColor=white" alt="Follow @fluxrbot on X"></a>
</p>

<p align="center">
  <a href="https://fluxrbot.com/?utm_source=github&utm_medium=readme">Website</a> ·
  <a href="https://fluxrbot.com/docs/?utm_source=github&utm_medium=readme">Getting started</a> ·
  <a href="CHANGELOG.md">Changelog</a> ·
  <a href="https://github.com/BasinTradesman/fluxrbot/releases">Releases</a> ·
  <a href="https://fluxrbot.com/tools/?utm_source=github&utm_medium=readme">Free tools</a> ·
  <a href="https://github.com/BasinTradesman/fluxrbot/discussions">Discussions</a>
</p>

This repository holds the public documentation for FluxrBot: how the engine decides, what it refuses and why, where its data comes from, the changelog for every version, and the measured numbers behind the site. The application's source code is not here and will not be.

*Already have a product key?* [Activate and download](https://app.fluxrbot.info).

## What it does

1. **Reads 52 news sources.** 37 polled feeds (government releases, SEC filings, wire services, major outlets, financial press) plus 15 streaming accounts where Reuters and AP publish first-hand. A streaming post reaches the database in 16 to 22 seconds; polled feeds take a median of 12 minutes.
2. **Matches stories to contracts.** Every event is checked against about 1,300 live Kalshi and Polymarket contracts by entity and wording. Weak matches are dropped before the model is asked anything.
3. **Executes the rules you set.** Risk appetite, position cap, daily turnover, exposure and category ceilings, drawdown halt, daily loss stop. Eight gates and two circuit breakers, checked in a fixed order. All of them are described in [docs/risk.md](docs/risk.md).
4. **Shows every decision.** Trades and refusals sit in the same journal, each with a stable reason code. On a typical day the engine refuses thousands of candidates and takes a handful. The full list of codes is in [docs/refusal-reasons.md](docs/refusal-reasons.md).
5. **Paper mode first.** Every trade is paper today. Live order routing is not switched on, and it stays off until the engine can show, on settled outcomes and against the real order book, that it earns after fees. The numbers below say whether it can.

## How a trade happens

Times are measured on the streaming path (Reuters or AP on Bluesky). Stories from polled feeds arrive minutes later and then go through the same steps.

| When | What happens |
|---|---|
| T+0s | A story is published. |
| T+16–22s | It is in the database, deduplicated against every other source reporting the same event. |
| T+20s | Matched against live contracts. Most events match nothing and stop here. |
| T+21s | The model reads the headline, the article and the current book price and answers three questions: does this bear on the outcome, which way, how sure. |
| T+22s | The model is asked for the strongest objection to the trade. A strong objection cancels it; a weak one is recorded and shown anyway. |
| T+23s | The candidate goes through the eight risk gates in order. The journal records the first gate that says no. |
| T+24s | The order book is walked to find the real entry price. If the spread is too wide for the price the model expects, a limit order is posted instead. |
| T+25s | A paper trade is opened, or a refusal is written with its reason. Either way it is in the app and, if connected, in your Telegram. |

The first streaming post the engine ever received went the whole way in four seconds. It was refused.

## Screenshots

<p align="center">
  <img src="https://fluxrbot.com/img/overview-screen.png" alt="Overview screen: paper balance, open positions, decisions today" width="32%">
  <img src="https://fluxrbot.com/img/signals-screen.png" alt="Feed screen: paper trades and refusals in one list, each with its reason" width="32%">
  <img src="https://fluxrbot.com/img/settings-screen.png" alt="Settings screen: risk appetite, venues, updates, Telegram" width="32%">
</p>

## Measured, not claimed

Every number in this section is regenerated from the live database every six hours by the same job that feeds the site. If a number is not yet measurable, it is absent rather than estimated.

[![settled outcomes](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Doutcomes)](https://fluxrbot.com/api/truth)
[![model vs price](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Dmodel)](https://fluxrbot.com/api/truth)
[![paper trades](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Dpaper)](https://fluxrbot.com/api/truth)

<!-- measured:start -->
_Updated 2026-09-19 from the live database. Same numbers: [`/api/truth`](https://fluxrbot.com/api/truth) · [strategies page](https://fluxrbot.com/strategies/?utm_source=github&utm_medium=readme)._

**Mode:** paper only. No live orders. Trading since 2026-07-31.

| What | Count |
|---|---|
| News events read | 174,022 |
| Event → contract links | 352,446 |
| Model verdicts | 92,650 |
| Contracts with a settled outcome | 8,661 |
| Price points recorded | 747,848 |
| Paper trades / refusals | 78 / 407,946 |

**Does the model beat the price?** On **3,473 settled outcomes** the model's Brier score is **0.141** against **0.141** for the market price. Direction hit rate 61% vs 79% for the price. **It does not beat the price.** That is the honest answer, and it matches every independent 2025–2026 study we could find (Prophet Arena, AIA Forecaster, PolyBench, Prediction Arena).

**Strategies, each on its own $100 paper account:**

| Strategy | | Trades | Closed | Direction right | Result |
|---|---|---|---|---|---|
| `news-llm` | on | 21 | 18 | 50% | +$3.18 |
| `group-arb` | on | 37 | 37 | 54% | $-0.34 |
| `cross-venue-arb` | on | 2 | 0 | — | +$0 |
| `weather` | on | 12 | 12 | 42% | +$56.49 |
| `no-bias` | off | 6 | 6 | 17% | $-27.93 |

Total closed 73, net +$31.4. Weather's plus is one +$95 trade on a 12-trade sample. `no-bias` was switched off on 2026-09-13 after 1 hit in 6: the overpricing it was built on did not hold out of sample. Neither is an edge; we say so.

**Market calibration, our own data (all categories, price ~24 h before close):**

| Price band | n | Avg price | YES resolved | YES − price |
|---|---|---|---|---|
| 0-10¢ | 1270 | 0.8¢ | 0.5% | -0.3pp |
| 10-25¢ | 117 | 17.7¢ | 17.1% | -0.6pp |
| 25-40¢ | 124 | 32.6¢ | 32.3% | -0.3pp |
| 40-60¢ | 230 | 49.9¢ | 50.9% | +1pp |
| 60-75¢ | 125 | 66.7¢ | 66.4% | -0.3pp |
| 75-90¢ | 62 | 82.1¢ | 74.2% | -7.9pp |
| 90-100¢ | 890 | 99.5¢ | 99.6% | +0.1pp |

Model outages in the last 60 days: 22 day(s) (2026-08-22 → 2026-09-18, gateway balance — now alerted).
<!-- measured:end -->

## What it does not do

- **No live trading yet.** See above.
- **No income promises.** Not on the site, not here.
- **No wallet connection.** Kalshi will connect through an API key that stays on your machine. Polymarket needs a wallet private key to trade, which the app will never ask for, so Polymarket is read and match only.
- **No 5- and 15-minute crypto markets.** Most of that volume is bots with sub-100 ms latency. We do not pretend to compete there.
- **No certificate-signed installer.** Windows SmartScreen warns on the first manual install (More info → Run anyway). Over-the-air updates are signed with our own key and skip the warning.

## Get started

1. **Windows 10 or 11.** Download the `.msi` from the [latest release](https://github.com/BasinTradesman/fluxrbot/releases/latest). The SHA-256 of every installer is in the release notes.
2. **Get a product key** at [fluxrbot.com](https://fluxrbot.com/?utm_source=github&utm_medium=getting). The trial is 3 days, no card, and ends by itself. After that it is $2,499 a month, cancel anytime.
3. **Activate**, pick a risk appetite, connect Telegram if you want alerts on your phone. The app starts in paper mode and stays there.
4. **Read the feed.** The [getting-started guide](https://fluxrbot.com/docs/?utm_source=github&utm_medium=readme) explains each screen and how to read a refusal.

Updates: from 1.0.4 the app installs signed updates on its own if you leave the checkbox on at activation. Turn it off and you get a banner and an Update now button instead. Either way you get an email about every version.

## Docs

| Page | What is in it |
|---|---|
| [docs/risk.md](docs/risk.md) | The eight gates and two circuit breakers, in the order they are checked, and why each is shaped the way it is |
| [docs/refusal-reasons.md](docs/refusal-reasons.md) | Every reason code the journal can show, generated from the engine's own table |
| [docs/sources.md](docs/sources.md) | The 52 sources, their weights, the streaming connection, and the feed failures we learned from |
| [docs/free-tools.md](docs/free-tools.md) | The seven calculators on the site and how they relate to the engine |
| [docs/engine-truth.json](docs/engine-truth.json) | The measured numbers above, machine-readable, same source as `/api/truth` |

The full guide, the strategies page and the risk disclosure live on the site: [fluxrbot.com/docs](https://fluxrbot.com/docs/?utm_source=github&utm_medium=readme).

## Free tools

Browser calculators at [fluxrbot.com/tools](https://fluxrbot.com/tools/?utm_source=github&utm_medium=readme). No account, nothing to install.

- [Kelly calculator](https://fluxrbot.com/tools/kelly-calculator/?utm_source=github&utm_medium=readme): position size from your probability and the price, with half and quarter Kelly
- [Odds converter](https://fluxrbot.com/tools/odds-converter/?utm_source=github&utm_medium=readme): probability, American, decimal, fractional odds and contract price in cents
- [Payout calculator](https://fluxrbot.com/tools/payout-calculator/?utm_source=github&utm_medium=readme): payout, return and break-even with Kalshi's fee formula applied
- [Arbitrage checker](https://fluxrbot.com/tools/arbitrage-checker/?utm_source=github&utm_medium=readme): whether a Polymarket vs Kalshi price gap survives fees
- [Live radar](https://fluxrbot.com/tools/live-radar/?utm_source=github&utm_medium=readme): biggest movers, highest volume and near-resolved contracts, rebuilt hourly
- [Conditional chain probability](https://fluxrbot.com/tools/conditional-chain-probability/?utm_source=github&utm_medium=readme): joint probability and fair price for a multi-step event
- [Crosstab to topline](https://fluxrbot.com/tools/crosstab-to-topline-calculator/?utm_source=github&utm_medium=readme): turnout-weighted margin and win-probability range from subgroup polls

Details and how each maps to the engine: [docs/free-tools.md](docs/free-tools.md).

## Changelog

[CHANGELOG.md](CHANGELOG.md) lists every published app version and the engine changes behind it. The same text is on the site at [fluxrbot.com/changelog](https://fluxrbot.com/changelog/?utm_source=github&utm_medium=readme), and each app version is a [GitHub release](https://github.com/BasinTradesman/fluxrbot/releases) with the installer attached.

## Star the repo

If you want the desk's changelog and new tools first, star the repo. Releases and doc changes land here before they are announced anywhere else.

## Community

- [Discussions](https://github.com/BasinTradesman/fluxrbot/discussions): questions about a refusal, a number that looks wrong, a source you think we should read
- X: [@fluxrbot](https://x.com/fluxrbot)
- TikTok: [@fluxr.news](https://www.tiktok.com/@fluxr.news)
- Email: [hello@fluxrbot.com](mailto:hello@fluxrbot.com)

## License

Documentation in this repository is licensed [CC BY 4.0](LICENSE). The application itself is proprietary.

---

*FluxrBot is software, not investment advice. Trading event contracts involves risk of loss. Results shown are from a paper account and are simulated. FluxrBot is not affiliated with Polymarket or Kalshi.*
