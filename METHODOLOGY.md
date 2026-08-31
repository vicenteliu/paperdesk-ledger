# Scoring methodology

**Fixed 2026-08-30, before any outcome was computed.** That ordering is the whole
point: choosing a horizon or a hit criterion after seeing the prices is how a
track record gets laundered. This file is in git history so the order is checkable.

## What gets scored

Only directional calls: `Buy` / `Overweight` / `Underweight` / `Sell`.

`Hold` is **not** scored. It is not a falsifiable prediction, and assigning it a
score would be manufacturing data. Instead the **no-opinion rate** — the share of
scheduled calls that came back `Hold` — is reported as its own number. How often
a system declines to commit is a result about that system.

Entries with `status: no-prediction` (the pipeline failed that night) are excluded
from scoring and counted separately, as **coverage**.

## Horizon

**One month** (21 trading days) from `data_through`.

One week was rejected: a week of price movement is mostly noise, and a threshold
that fires on noise is not a threshold. "To date" was rejected because it gives
earlier calls a longer runway than later ones — the horizon has to be the same
for every entry or the comparison is meaningless.

## Hit criterion

**Beat SPY over the horizon**, not direction alone.

- `Buy` / `Overweight` — hit if the ticker's total return over the horizon
  exceeds SPY's over the same window.
- `Underweight` / `Sell` — hit if it trails SPY.
- For SPY itself, the benchmark is 0% absolute return.

Direction-only was rejected because `Overweight` and `Underweight` are relative
claims. In a month when the whole market rises, "the price went up" is free.

**This is the harder standard and it will make the record look worse than a
direction-only count would.** That was understood when it was chosen.

## Ties and edge cases

- Within ±0.25% of the benchmark: recorded as `flat`, counted in neither column,
  reported separately. Not silently rounded into a win.
- If the horizon has not elapsed, the entry is `pending`. Pending entries are
  never scored early, and never quietly dropped.
- Delisting, halts, or missing data over the window: `unscorable`, with the reason.

## What is not adjusted for

No risk adjustment, no position sizing, no transaction costs, no dividends beyond
what the price series already reflects. This measures one thing: did the call beat
the benchmark over a fixed window. Anything more elaborate would need choices that
are easy to make favourably after the fact.

## Changing this file

Any change applies **only to calls registered after the change**, and must say so
in its commit message. Re-scoring past entries under new rules is exactly the
failure mode this file exists to prevent.
