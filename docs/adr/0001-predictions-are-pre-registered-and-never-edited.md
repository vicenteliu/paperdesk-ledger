# Predictions are pre-registered, pushed unattended, and never edited

The only thing that makes a public prediction record worth reading is that each
entry demonstrably existed before its outcome did. Everything here follows from
protecting that one property.

## Decisions

**One file per prediction, never modified.** `git log --diff-filter=A -- <file>`
answers "when did this appear, and has it changed since" in one command. An
append-only JSONL would technically carry the same history, but a reader has to
reconstruct which commit added which line and confirm earlier lines were untouched.
The verification should be trivial, because a verification nobody performs is
decoration.

**Pushed automatically, the same night, with no review step.** A prediction that a
human can read and then decide whether to publish is not pre-registered — the
selection happens before anyone sees the ledger, which is precisely the bias the
ledger claims to remove. The cost is real: entries that turn out embarrassing
cannot be pulled.

**Failed nights are recorded as entries.** If a slot was scheduled and produced no
prediction, a `no-prediction` record goes in with the failing step. Otherwise the
ledger silently contains only the nights the pipeline worked, which overstates both
coverage and reliability. In August 2026 the pipeline failed 5 of 12 scheduled
runs; that number belongs in public alongside the calls.

**Scoring rules fixed before any outcome was computed** (METHODOLOGY.md), and
changes apply only to calls registered afterwards.

**`run_date` and `data_through` are separate fields.** When the prediction was made
and what data it was based on are different facts. Merging them is what produced
the archive's defect: the pipeline stamped `date.today()` and runs on Sundays, so
6 of 12 archived calls claim a date on which no market close exists.

**The archive is published as history, not as a track record.** Of 12 archived
calls, 7 are `Hold` and only 1 of the 5 directional calls can demonstrate
pre-registration. Presenting that as a scorecard with caveats underneath would be
a claim of evidence that the data does not support.

## Consequences

- Bad calls are permanent and public.
- The record starts near-empty, and the first meaningful sample is months away.
- A pipeline bug can publish a malformed entry unattended. That entry stays, with
  a correction appended as a **new** file rather than an edit to the old one.

## What would falsify the premise

If entries ever start being written after their `data_through` — the same defect
the archive has — the ledger stops meaning anything, and no amount of caveat text
repairs it. The automated same-night push exists to make that failure loud rather
than gradual.
