# FluxrBot

**A prediction-market research bot that shows its work — including every trade it refused to make.**

### → [**fluxrbot.com**](https://fluxrbot.com)

[How it works](https://fluxrbot.com/#how) ·
[Strategies](https://fluxrbot.com/strategies/) ·
[What the engine refuses](https://fluxrbot.com/risk/) ·
[Getting started](https://fluxrbot.com/docs/) ·
[Free tools](https://fluxrbot.com/tools/) ·
[Live radar](https://fluxrbot.com/tools/live-radar/) ·
[Glossary](https://fluxrbot.com/glossary/) ·
[Blog](https://fluxrbot.com/blog/) ·
[Changelog](https://fluxrbot.com/changelog/)

*Already have a product key?* → [activate and download](https://app.fluxrbot.info)

---

FluxrBot reads breaking news, asks a language model how it moves a specific
prediction-market contract, and compares that answer against the price the
order book is already showing. When the two disagree by enough to survive the
spread and the fees, that is a signal.

Most of the time they do not disagree, and the bot says so. Out of roughly
13,000 decisions recorded so far, **19 became trades.** The rest are written
down with the reason they were rejected. That ratio is the product, not an
embarrassment: a bot that finds an opportunity every hour is not finding
opportunities, it is finding noise.

This repository holds the public documentation, the changelog and the release
notes. The trading engine itself is not open source.

---

## What it actually does

**Reads.** 37 news feeds plus 15 Bluesky accounts on a live streaming
connection. Reuters and AP closed their public RSS; on Bluesky they publish
first-hand, and a post reaches our database in **16–22 seconds** against a
12-minute median for polled feeds.

**Matches.** Every event is matched against ~1,300 live contracts on Kalshi and
Polymarket by entity and wording. The entity dictionary extends itself: a
nightly job asks the model to name the entities in contracts our manual
dictionary missed.

**Judges.** Contracts that survive matching go to the model with the headline,
the article body, and the current book price. The model answers three things:
does this news bear on the outcome, which way, and how sure are you.

**Argues with itself.** Every trade candidate gets a second question: *what is
the strongest objection to this bet?* Not "score it again" — rephrasing the
same reasoning returns the same answer. A strong objection cancels the trade.
A weak one is recorded and shown anyway. An argument you only see when it wins
cannot be checked, and a journal you cannot check is not a journal.

**Checks twice.** A second, independent estimate sees only the news, the
contract and the market price — not our probability and not our reasoning. If
the two disagree on direction, there is no signal. If they agree, we take the
**more cautious** of the two, not the average: both came from one model on one
set of facts, and averaging would narrow the spread more than reality allows.

**Prices against the real book.** Entry is computed by walking the actual order
book, not from the last traded price. When the spread eats the edge, the engine
places a limit order instead of refusing — measured fill rate 71%, average
spread crossed 4.1¢ against 15.0¢ for market orders.

**Refuses.** Eight risk gates run in order of weight, so the journal records
the main reason rather than the first one hit: account halted, position cap,
cash, total exposure, daily turnover, one position per mutually-exclusive
group, category concentration, and no more than two positions sharing an
entity. Two circuit breakers: a daily loss stop that clears itself at midnight,
and a drawdown halt that only a human can clear — because drawdown means
*something is wrong with the strategy*, and time does not answer that.

**Reports.** Every refusal is logged next to every trade, with a reason code
translated into four languages. Losing trades that settle get an automated
post-mortem naming the cause from a closed list, so repeated mistakes group
instead of scattering.

---

## Strategies

Four, each on its **own** separate paper account. Mixing two strategies into
one account means never being able to tell them apart afterwards.

| Strategy | What it looks for |
|---|---|
| `news-llm` | News moves the probability; the market has not moved yet |
| `group-arb` | Every outcome of one event trades for less than the dollar it must pay |
| `cross-venue-arb` | Same question, two venues, two prices |
| `weather` | A three-model forecast ensemble against the price of a temperature contract |

Each strategy's page on the site states its assumption and, more importantly,
**how it loses.** `group-arb` in particular required a lesson: "mutually
exclusive" does not mean "exhaustive". Its first run found an 84¢ "arbitrage"
across eight outcomes of *which state becomes the 51st* — no arbitrage exists
there, because no new state may appear at all.

---

## What it does not do

Stated plainly, because finding out later is worse.

- **No live trading yet.** Everything runs on paper. This is a deliberate block
  until the measurement base fills: until we can show the model beats the
  *book price* — not a coin flip, the book — connecting real money would be
  guessing with someone's savings.
- **No income promises.** Not on the site, not here, not ever.
- **No wallet connection.** Kalshi automates through an API key that never
  leaves your machine. Polymarket requires a wallet private key to mint API
  credentials, so Polymarket stays signal-only. That makes "no wallet needed"
  true rather than marketing.
- **No auto-install.** The app tells you an update exists and downloads the
  installer; you run it. Software that watches money should not replace itself
  while you are not looking.
- **The installer is not certificate-signed.** Windows SmartScreen will warn
  you. Choose *More info → Run anyway*.
- **The Telegram bot speaks English only.** The app speaks four languages.

---

## How the numbers here are produced

Every figure on this page and on the site is measured, not estimated. Where a
number is not yet measurable, it is absent rather than approximated.

The one that matters most is still open: **does the model beat the book?** The
market price *is* the market's probability, so beating a coin flip proves
nothing. As of this writing the honest answer is *not enough settled outcomes
to say* — 2.3 usable ones accumulate per day, and the threshold is 200.

Three measurement errors were found and fixed while trying to answer it, each
of which made the engine look worse than it was: comparing against 50% when
prediction-market prices systematically drift down, letting settlement prices
count as "market movement", and treating many verdicts on one contract as
independent observations. The corrected read is that there is **no measurable
signal and no measurable anti-signal** — the data cannot answer yet, which is a
different and more honest statement than the one we started with.

---

## Getting it

Start at **[fluxrbot.com](https://fluxrbot.com)** — pricing, what it does, and
how to request access are all there.

If you already hold a product key, the installer is at
[app.fluxrbot.info](https://app.fluxrbot.info): enter the key and the download
link appears. The installer is not published here — access to it goes through
the key.

Release notes for every version: [CHANGELOG.md](CHANGELOG.md) here, or
[fluxrbot.com/changelog](https://fluxrbot.com/changelog/) on the site.

---

## Documentation

Full documentation lives on the site — [**fluxrbot.com/docs**](https://fluxrbot.com/docs/).
These pages mirror the parts most worth reading before you decide anything:

- [What the engine refuses on its own](docs/risk.md) — the eight gates and two
  breakers · [on the site](https://fluxrbot.com/risk/)
- [Refusal reasons](docs/refusal-reasons.md) — every code the journal can show you
- [Data sources](docs/sources.md) — what it reads and how fast

---

*FluxrBot is a research tool for prediction markets. Nothing here is financial
advice, and no part of it promises a return.*
