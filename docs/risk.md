# What the engine refuses on its own

Risk rules exist to be boring. These are the ones that stop a trade before it
happens, in the order they are checked — deliberately from the weightiest to
the most specific, so the journal records the *main* reason rather than the
first one hit.

## The eight gates

| # | Gate | Rule | Why it is shaped this way |
|---|---|---|---|
| 1 | Account halted | absolute block | a stopped account is stopped; nothing else needs checking |
| 2 | Position cap | 10% of the account | a larger trade is **trimmed**, not rejected — the idea was fine, the size was not |
| 3 | Cash | open + new ≤ balance | without it a paper account quietly goes negative |
| 4 | Total exposure | ≤ 50% of the **current** balance | an account that has lost half should risk half as much |
| 5 | Daily turnover | $30, resets at midnight | a limit has to reset in a way a human can predict |
| 6 | Group | one position per mutually-exclusive group | three contracts about one question are one bet |
| 7 | Category | ≤ 30% of the account | five macro positions are a single wager on macro |
| 8 | Subject | ≤ 2 positions sharing an entity | a checkable stand-in for correlation |

A hedge basket passes the gates as **one** trade and is never trimmed:
trimming one leg turns a hedge into a directional bet.

## The two circuit breakers

They are different on purpose.

**Daily loss stop** — 20% of the starting balance in realised losses. Clears
itself at midnight, because it is a statement about a *day*, and days end.

**Drawdown halt** — 30% below the account's peak. Clears only by hand, because
it is a statement that *something is wrong with the strategy*, and time does
not answer that.

Drawdown does not fire until at least twenty trades have closed. Three full
losses in a row at a 10% position size produce exactly the same 30% and are
ordinary bad luck. Reacting to that is reacting to noise.

## Stopping blocks opening, not closing

A halted account still settles and closes everything already open. Otherwise
the circuit breaker would become a way to freeze a loss instead of realising
it.

A halted strategy is not even asked for candidates — its verdicts cost money,
and we could not act on them anyway.

## What is deliberately absent

**A correlation model.** Analysis of $40M of realised prediction-market
arbitrage found that 62% of the inter-market dependencies a language model
identifies do not hold up in execution. Rather than model correlation badly, we
measure the part that can be checked by eye: a shared group key and a shared
entity in the question.

**Kelly sizing, for now.** Fractional Kelly is implemented and switched off. It
turns on only once calibration has been *applied* — and calibration applies
only after 200 settled outcomes. Kelly on an uncalibrated probability
multiplies the error, because it grows more aggressive precisely where the
model is more confident. The order is not optional.

---

[← All documentation](https://fluxrbot.com/docs/) · [This page on the site](https://fluxrbot.com/risk/) · [fluxrbot.com](https://fluxrbot.com)
