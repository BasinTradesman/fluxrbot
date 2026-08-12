# Changelog

Every published version of the FluxrBot app, plus the engine changes behind it.
The engine updates itself on the server; app versions are installed by you.

Also published at [fluxrbot.com/changelog](https://fluxrbot.com/changelog/).
Product and pricing: [fluxrbot.com](https://fluxrbot.com) · Have a key? [activate and download](https://app.fluxrbot.info)

## 1.0.3 — 2026-08-12

- Fixed: external links open again — the update button, Telegram connect and every outbound link silently did nothing in 1.0.0–1.0.2 (a missing URL permission in the desktop shell)
- If opening the browser ever fails, the app now copies the link and shows it to you instead of staying silent
- New engine refusal reasons translated in all four languages: news already priced in, forecast disagreeing with the market too much, price-band statistics still thin, strategy position limits

## Engine — 2026-08-12

- New contrarian strategy: fades the crowd's systematic overpricing of YES, with the edge measured on our own settled outcomes and re-measured every run
- Arbitrage baskets are now held to resolution — the forced 24-hour exit was paying the spread twice and turning locked profit into noise
- High-confidence verdicts (0.70+) are held to resolution: measured hit rate 72–94% on settled outcomes
- News-chasing gate: if the market already moved 5¢+ toward the verdict since publication, the engine passes — late entries were buying the crowd's reversal
- Weather strategy learned its lesson: a forecast disagreeing with the market by 22¢+ is now treated as our error, not an opportunity; two open positions max
- Probability calibration crossed 300 settled outcomes and is now applied — measured shift ceiling replaces the hand-picked constant, Kelly sizing engaged

## 1.0.2 — 2026-08-08

- Fixed: every screen can now scroll — a layout bug clipped all lists to one viewport
- Paper balance now says what it is made of: total across per-strategy accounts, with the starting sum shown
- Refusal reasons in the daily summary group by cause instead of splitting one cause across price variants
- Venues screen: what is read live today, how Kalshi will connect, why Polymarket stays signal-only
- Window title no longer carries a stale version string

## 1.0.1 — 2026-08-08

- Updates screen: what you have, what is available, and the full version history with descriptions
- Strategies screen: four strategies, each scored on its own separate paper account
- The engine now argues against its own trades — the objection is shown on every signal, whether or not it stopped the trade
- Two independent probability estimates per trade; if they disagree on direction, the engine passes
- Account-level risk stops: drawdown halt, daily loss stop, exposure and concentration ceilings — visible in the app when they fire
- Risk appetite setting: choose how bold a feed you want, from confident-only to exploratory
- Trial is now 3 days from activation
- Refusal reasons translated in all four app languages

## Engine — 2026-08-05

- Streaming sources: Reuters and AP now arrive first-hand within seconds of publication
- New forecast-vs-market strategy prices daily temperature contracts from a three-model weather ensemble
- Every losing settled trade gets an automated post-mortem naming the cause
- Release calendar loaded with real CPI, jobs, GDP and PCE dates — the engine polls faster around each release

## Engine — 2026-08-04

- Event-driven pipeline: decisions now happen seconds after news arrives instead of on a 15-minute cycle
- Group and cross-venue arbitrage strategies added, with basket-level execution checks
- Limit orders placed automatically when the spread eats the edge

## 1.0.0 — 2026-07-31

- First Windows build: activation, signals feed with trades and logged refusals, journal, settings, four languages
- Telegram notifications for signals and closed positions
- Paper trading engine live on 28 news sources
