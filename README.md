# paperdesk-ledger

A public, append-only record of what an open-source multi-agent LLM trading
framework said about specific tickers — written down **before** the outcome was
known, and never edited afterwards.

## What this is not

**It is not a track record yet.** Everything under `archive/` predates this
ledger and mostly cannot prove it was written before the outcome. Of 12
archived calls, 7 are `Hold` (not a falsifiable
prediction) and only **1 of the 5 directional calls**
can demonstrate pre-registration. One data point is an anecdote.

Publishing that as a scorecard, with the caveats in a footnote, would produce
something that looks like evidence and isn't. So the archive is published as
**history**, and scoring starts from the first entry in `predictions/`.

## How pre-registration works here

- One file per call. Never modified after it lands.
  `git log --diff-filter=A -- predictions/<id>.json` shows exactly when it was
  added and that nothing touched it since. Appending to a shared file cannot
  show that as cleanly.
- **Pushed automatically the same night, before the next session opens.** There is
  no human review step, because a prediction that can be looked at and then
  withheld is not a pre-registration.
- Scheduled slots that produced no prediction are recorded too, with the reason
  and the step that failed. A ledger that only contains the nights the pipeline
  worked is selective disclosure.
- Scoring rules were fixed in [METHODOLOGY.md](./METHODOLOGY.md) **before** any
  outcome was computed, and are themselves in the git history for that reason.

Rationale and the trade-offs: [docs/adr/0001](./docs/adr/0001-predictions-are-pre-registered-and-never-edited.md).

## Two known defects in the archive

**Six of 12 archived calls carry a `run_date` on which no market close
exists.** The pipeline stamped `date.today()`, and it runs on Sundays. The design
always intended Monday slots to use Friday's close — the label was wrong, not the
analysis. Those rows are marked ⚠ and left as recorded; back-filling a "corrected"
date into a ledger whose entire premise is that entries are never edited would
defeat the premise, and the correct date would be a guess anyway.

**`data_through` is derived, not reported.** The framework does not emit the last
bar it actually consumed, so `data_through` is the last real close at or before
`run_date`, looked up separately and flagged `data_through_derived: true`. It is
the best available information, not proof of which bar was used.

## Archive (12 calls · Aug 2026)

| id | ticker | rating | run date | data through | recorded | pre-registered |
|---|---|---|---|---|---|---|
| `2026-08-01-AAPL` | AAPL | Hold | 2026-08-01 ⚠ | 2026-07-31 | 2026-08-01 | yes |
| `2026-08-01-NVDA` | NVDA | Buy | 2026-08-01 ⚠ | 2026-07-31 | 2026-08-01 | yes |
| `2026-08-04-AAPL` | AAPL | Hold | 2026-08-04 | 2026-08-04 | 2026-08-05 | **no** |
| `2026-08-06-MSFT` | MSFT | Hold | 2026-08-06 | 2026-08-06 | 2026-08-06 | yes |
| `2026-08-09-TSLA` | TSLA | Underweight | 2026-08-09 ⚠ | 2026-08-07 | 2026-08-16 | **no** |
| `2026-08-11-SPY` | SPY | Overweight | 2026-08-11 | 2026-08-11 | 2026-08-16 | **no** |
| `2026-08-13-SMCI` | SMCI | Hold | 2026-08-13 | 2026-08-13 | 2026-08-14 | **no** |
| `2026-08-16-QQQ` | QQQ | Hold | 2026-08-16 ⚠ | 2026-08-14 | 2026-08-18 | **no** |
| `2026-08-18-NVDA` | NVDA | Overweight | 2026-08-18 | 2026-08-18 | 2026-08-19 | **no** |
| `2026-08-23-AAPL` | AAPL | Hold | 2026-08-23 ⚠ | 2026-08-21 | 2026-08-23 | yes |
| `2026-08-25-TSLA` | TSLA | Underweight | 2026-08-25 | 2026-08-25 | 2026-08-30 | **no** |
| `2026-08-30-SPY` | SPY | Hold | 2026-08-30 ⚠ | 2026-08-28 | — | unknown |

⚠ = `run_date` fell on a day with no market close.
"recorded" is when the call first entered a **private** repo, so an outside reader
cannot verify it. It is shown to mark honestly which rows could not have been
written after the fact — not to be believed on trust.

**No-opinion rate: 7/12 (58%).**
How often the framework declines to take a position is itself a result, so it is
reported rather than filtered out.

## License

Apache-2.0. Nothing here is investment advice; it is a record of what a piece of
software output.
