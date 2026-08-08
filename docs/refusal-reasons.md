# Refusal reasons

Every decision the engine makes is written down, including the ones that did
not become a trade. Each carries a stable code so the app can translate it and
so the daily report can group by it — free-form text would scatter one reason
across a hundred rows.

These are the codes the journal can show you.

## Before the model is asked

| Code | What it means |
|---|---|
| `book_at_edge` | market already priced at the edge |
| `weak_match` | link to the contract too weak |
| `not_news_driven` | this contract is settled by a forecast, not by news |

## What the model answered

| Code | What it means |
|---|---|
| `not_related` | the news does not bear on the outcome |
| `direction_unclear` | the headline does not settle a direction |
| `challenged` | the second opinion argued against this trade |
| `estimates_split` | two independent estimates disagreed on direction |

## The edge did not survive

| Code | What it means |
|---|---|
| `edge_below_threshold` | edge below the threshold |
| `edge_after_second` | the edge does not clear the bar on the more cautious estimate |
| `edge_gone_in_book` | the spread ate the edge |
| `longshot_too_cheap` | long shot: cheap contracts are systematically overpriced |
| `forecast_edge_below` | the forecast does not diverge from the market enough |
| `forecast_all_at_edge` | every bucket in the group is priced at the edge of the book |

## The order book said no

| Code | What it means |
|---|---|
| `book_empty` | order book empty — nobody to buy from |
| `book_too_thin` | not enough depth in the book |
| `book_unavailable` | venue did not answer |
| `limit_behind_queue` | limit price would sit behind the queue |
| `limit_out_of_range` | limit price outside the book |

## Arbitrage-specific

| Code | What it means |
|---|---|
| `arb_too_good` | edge too large to be arbitrage — the payout model is suspect |
| `arb_edge_below` | the basket does not clear the threshold on the real book |
| `arb_leg_unavailable` | one leg of the basket cannot be filled |
| `basket_over_cap` | one full set costs more than the position cap |
| `position_cap_atomic` | the basket is larger than the position cap and cannot be resized |

## Account risk gates

| Code | What it means |
|---|---|
| `account_halted` | the account is stopped by a risk rule |
| `no_cash` | not enough cash on the account |
| `open_exposure_limit` | too much of the account already in open positions |
| `daily_limit` | daily deployment limit reached |
| `position_open` | already holding a position on this question |
| `category_limit` | too much already riding on this category |
| `correlated_limit` | already holding positions on the same subject |
| `position_cap_tiny` | the position cap is smaller than a single contract here |

## Nothing to decide

| Code | What it means |
|---|---|
| `already_settled` | the contract has already settled — the outcome is known |

---

Generated from the engine's own reason table, so this page and the app can
never disagree about what a code means.

[← All documentation](https://fluxrbot.com/docs/) · [Strategies](https://fluxrbot.com/strategies/) · [fluxrbot.com](https://fluxrbot.com)
