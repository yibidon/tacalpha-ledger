# tacalpha-ledger

Public, append-only timestamp authority for the **Tactical Alpha Lab** signal
ledger (pre-launch research record).

The current research stream is a **liquid-ETF momentum rotation**: a transparent,
reproducible systematic rule over a curated universe of highly liquid,
exchange-traded funds. Signals reference liquid ETFs only — a publisher cannot
move such markets — by deliberate design.

## Files

- **`hashes.txt`** — append-only, one line per signal: `<seq> <committed-at UTC> <sha256>`.
- **`proofs/NNNNNN.ots`** — OpenTimestamps proof for each entry hash, anchored to
  the Bitcoin blockchain.
- **`ledger.json`** — the human-readable published record: each entry's public
  signal (symbols, ranks, execution window, exit rules, thesis), its `prev_hash`
  and `hash`, and a grade summary.

## How to verify an entry

Each entry hash is `sha256` over the canonical JSON of `{"signal": …, "prev_hash": …}`
— canonical = UTF-8, sorted keys, no whitespace — computed **before** the signal's
execution window opens. For any entry in `ledger.json`:

1. Take its `signal` and `prev_hash`.
2. Recompute `sha256` of the canonical bytes and confirm it equals `hash`.
3. Find `hash` in `hashes.txt` and confirm from this file's git history that it
   was committed before the signal's `execution_window.start`.
4. Run `ots verify proofs/<seq>.ots` (the proof is over the hash) to confirm the
   Bitcoin block that anchors it.

Each entry chains the previous entry's hash (`prev_hash`), so the record is
tamper-evident as a whole. A signal cannot be backdated, edited, or removed
without breaking the match.

## Pre-launch note (transparency)

This ledger is pre-launch. During early bootstrap — before it was announced or
read — the initial experimental entries used a single-name research stream; we
reconsidered and switched the public stream to liquid ETFs (rationale published
in the Tactical Alpha Lab notes). We record that plainly rather than paper over
it. **From public launch forward, the record is append-only and immutable.**

## Not advice

Nothing in this repository is investment, legal, or tax advice, or an offer or
solicitation of any security. Published signals are specific research calls
produced by systematic models — impersonal and general, not individualized
advice or recommendations to any person. Any grades shown are hypothetical.
