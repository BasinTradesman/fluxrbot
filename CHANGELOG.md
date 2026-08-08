# Changelog

Every published version of the FluxrBot app, plus the engine changes behind it.
The engine updates itself on the server; app versions are installed by you.

Also published at [fluxrbot.com/changelog](https://fluxrbot.com/changelog/).
Product and pricing: [fluxrbot.com](https://fluxrbot.com) · Have a key? [activate and download](https://app.fluxrbot.info)

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
