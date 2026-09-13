# FluxrBot

**A prediction-market research bot that shows its work — including every trade it refused to make, and the measurement that its own model does not beat the market price.**

[![settled outcomes](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Doutcomes)](https://fluxrbot.com/api/truth)
[![model vs price](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Dmodel)](https://fluxrbot.com/api/truth)
[![paper trades](https://img.shields.io/endpoint?url=https%3A%2F%2Ffluxrbot.com%2Fapi%2Fbadge%3Fkind%3Dpaper)](https://fluxrbot.com/api/truth)

### → [**fluxrbot.com**](https://fluxrbot.com/?utm_source=github&utm_medium=readme&utm_campaign=top)

[How it works](https://fluxrbot.com/?utm_source=github&utm_medium=readme#how) ·
[Strategies](https://fluxrbot.com/strategies/?utm_source=github&utm_medium=readme) ·
[What the engine refuses](https://fluxrbot.com/risk/?utm_source=github&utm_medium=readme) ·
[Getting started](https://fluxrbot.com/docs/?utm_source=github&utm_medium=readme) ·
[Free tools](https://fluxrbot.com/tools/?utm_source=github&utm_medium=readme) ·
[Blog](https://fluxrbot.com/blog/?utm_source=github&utm_medium=readme) ·
[Changelog](CHANGELOG.md) ·
[**Releases ↓**](https://github.com/BasinTradesman/fluxrbot/releases)

*Already have a product key?* → [activate and download](https://app.fluxrbot.info)

---

## Why this exists

Every prediction-market bot on GitHub and Telegram sells a win rate. In 2026 the independent numbers are brutal: ~70% of Polymarket addresses lose money, 0.04% of addresses take 70% of the profit, and the best language models score *at* the market price on settled outcomes, not above it (Prophet Arena, AIA Forecaster, PolyBench, Prediction Arena — all 2025–2026).

We measured the same thing on our own pipeline — and got the same answer. So FluxrBot is not a signal machine. It is the tool you open **before** you trade:

- **Check** a contract (paste a Kalshi or Polymarket link): live order book on $50 each side, taker vs maker fee, breakeven probability, how contracts at this price and horizon actually resolved in our 7,000+ settled outcomes, what news touched it and what the model read into it, and which resolution-rule phrases start disputes.
- **Watch** the contracts where your money is: when a story lands that the model reads as moving the outcome, you get it in Telegram — marked *with* or *against* your position.
- **Read the refusal journal**: every decision the engine made, with the reason, next to the few it turned into paper trades.

Everything is measured. Where a number is not yet measurable, it is absent rather than approximated.

---

## Measured, not claimed

<!-- measured:start -->
_Updated 2026-09-13 from the live database. Same numbers: [`/api/truth`](https://fluxrbot.com/api/truth) · [strategies page](https://fluxrbot.com/strategies/?utm_source=github&utm_medium=readme)._

**Mode:** paper only. No live orders. Trading since 2026-07-31.

| What | Count |
|---|---|
| News events read | 146,530 |
| Event → contract links | 289,302 |
| Model verdicts | 63,816 |
| Contracts with a settled outcome | 7,435 |
| Price points recorded | 637,727 |
| Paper trades / refusals | 76 / 338,891 |

**Does the model beat the price?** On **2,847 settled outcomes** the model's Brier score is **0.145** against **0.145** for the market price. Direction hit rate 58% vs 79% for the price. **It does not beat the price.** That is the honest answer, and it matches every independent 2025–2026 study we could find (Prophet Arena, AIA Forecaster, PolyBench, Prediction Arena).

**Strategies, each on its own $100 paper account:**

| Strategy | | Trades | Closed | Direction right | Result |
|---|---|---|---|---|---|
| `news-llm` | on | 21 | 18 | 50% | +$3.18 |
| `group-arb` | on | 37 | 33 | 42% | +$0.49 |
| `cross-venue-arb` | on | 0 | 0 | — | +$0 |
| `weather` | on | 12 | 12 | 42% | +$56.49 |
| `no-bias` | off | 6 | 6 | 17% | $-27.93 |

Total closed 69, net +$32.23. Weather's plus is one +$95 trade on a 12-trade sample. `no-bias` was switched off on 2026-09-13 after 1 hit in 6: the overpricing it was built on did not hold out of sample. Neither is an edge; we say so.

**Market calibration, our own data (all categories, price ~24 h before close):**

| Price band | n | Avg price | YES resolved | YES − price |
|---|---|---|---|---|
| 0-10¢ | 1133 | 0.8¢ | 0.5% | -0.3pp |
| 10-25¢ | 105 | 17.6¢ | 18.1% | +0.5pp |
| 25-40¢ | 114 | 32.7¢ | 30.7% | -2pp |
| 40-60¢ | 176 | 50.1¢ | 50% | -0.1pp |
| 60-75¢ | 113 | 66.5¢ | 63.7% | -2.8pp |
| 75-90¢ | 51 | 81.7¢ | 72.5% | -9.2pp |
| 90-100¢ | 773 | 99.6¢ | 99.6% | 0pp |

Model outages in the last 60 days: 21 day(s) (2026-08-22 → 2026-09-11, gateway balance — now alerted).
<!-- measured:end -->

---

## What it does

**Reads.** 37 news feeds plus 15 Bluesky accounts on a streaming connection (Reuters and AP publish there first-hand; a post reaches the database in 16–22 seconds against a 12-minute median for polled feeds).

**Matches.** Every event is matched against ~1,300 live contracts on Kalshi and Polymarket by entity and wording. The entity dictionary extends itself nightly.

**Judges.** Contracts that survive matching go to the model with the headline, the article body and the current book price. Three questions: does this bear on the outcome, which way, how sure.

**Argues with itself.** Every trade candidate gets the question *what is the strongest objection to this bet?* A strong objection cancels the trade; a weak one is recorded and shown anyway.

**Prices against the real book.** Entry is computed by walking the actual order book. When the spread eats the edge, the engine posts a limit order instead — measured fill rate 80%, average spread crossed 3.7¢ against 15.5¢ for market orders. Fees are the venues' real 2026 schedules: Kalshi 7% × p × (1−p) taker, Polymarket 4–7% × p × (1−p) by category, makers free.

**Refuses.** Eight risk gates in order of weight, two circuit breakers, one journal for trades and refusals alike.

**Checks, watches, alerts.** The pre-trade check and the watchlist are in the desktop app and in the Telegram bot (`/check <link>`, `/watch <link> [yes|no] [price]`, `/list`).

**Updates itself, if you let it.** From 1.0.4 the app installs signed updates over the air; a checkbox at activation (on by default) controls it, and you get an email either way.

---

## What it does not do

- **No live trading.** Everything runs on paper until the engine can show — on settled outcomes, against the *book price* — that it earns after fees. It cannot yet, and this page says so above.
- **No income promises.** Not on the site, not here, not ever.
- **No wallet connection.** Kalshi connects through an API key that never leaves your machine. Polymarket needs a wallet private key to mint trading credentials, so Polymarket stays signal-only.
- **No 5- and 15-minute crypto markets.** 55–62% of that volume is bots with sub-100 ms latency; we do not pretend to compete there.
- **The installer is not certificate-signed.** Windows SmartScreen warns on first manual install; over-the-air updates are signed with our own key and skip it.

---

## Getting it

Installers are attached to [**GitHub Releases**](https://github.com/BasinTradesman/fluxrbot/releases). The app asks for a product key on first launch; keys and the trial are at [fluxrbot.com](https://fluxrbot.com/?utm_source=github&utm_medium=readme&utm_campaign=getting).

Release notes for every version: [CHANGELOG.md](CHANGELOG.md).

---

## Documentation

- [What the engine refuses on its own](docs/risk.md) — the eight gates and two breakers
- [Refusal reasons](docs/refusal-reasons.md) — every code the journal can show you
- [Data sources](docs/sources.md) — what it reads and how fast
- [Engine truth (JSON)](docs/engine-truth.json) — the numbers above, machine-readable, regenerated every six hours

Full documentation lives on the site — [**fluxrbot.com/docs**](https://fluxrbot.com/docs/?utm_source=github&utm_medium=readme).

---

*FluxrBot is a research tool for prediction markets. Nothing here is financial advice, and no part of it promises a return.*
