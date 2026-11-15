# tiny-chess-01 — make.fun entry

$500, 20 paid places. Deadline **2026-11-25 00:00 WIB**.

## The entry

- `main.py` — **4,052 bytes** (limit 4,096). Python 3.13 stdlib only,
  self-contained, deterministic (fixed 130,000-node budget, no clock,
  no randomness). No other repo file is read.
- `main.py` is compressed: lzma (FORMAT_ALONE) + base85 payload
  with unpacker (rules allow "any compressed payload with its
  unpacker"). Readable source: `bench/v22.py` (not in the repo).
- `LICENSE` — MIT. Entry = public GitHub repo (created after the
  competition opened) + AI session transcript.

## How it works

0x88 Sunfish-style engine (120-square board, full 64-square
piece-square tables for every piece). Negamax alpha-beta, iterative
deepening on a shared 130,000-node budget, transposition table,
null-move pruning, late-move reductions, quiescence search, MVV-LVA
+ TT-move + killer + history ordering, check extension, razoring,
delta pruning, repetition detection, quiet promotions in
quiescence. Full legal move generation. Talks to the env at
`MAKEFUN_ENV_URL` (`POST /game`, `POST /move`), syncs by `start_fen`
+ replaying `moves`, plays only from the env's `legal_moves`.

## Strength — honest assessment

v22 is a large step up: **practice Elo 1748 (trial-0) and 1464
(trial-1), mean ~1606** — two 8-game runs on the v5 kit. All wins
by checkmate; no time losses (max ~5 s/move). v21 was 1173.

- **Practice runs (v22): trial-0 Elo 1748, 6/1/1**; **trial-1 Elo
  1464, 4/1/3**. Mean ~1606 — above the ~1325 house baseline.
- Rounds: v1 **562** → v2 **705** → v3 **871** → v21 **1173** →
  v22 **~1606** (trial-0 1748, trial-1 1464).
- v22 changes: budget 100k → 130k, check extension, razoring,
  delta pruning, repetition detection, history, quiet promotions
  in quiescence. A root-sort tie-break bug (`reverse=True`) had
  collapsed it to 0/12; fixed. Depth now 8–11 in tactics.

## Verification done

- Perft vs python-chess, 6 positions to depth 3–4 — **all counts
  exact**. Eval/Zobrist verified identical to v21 after random
  30-move sequences. Root-sort bug found by ablation and fixed
  (v22 ties v21 6/12 at equal 20k nodes).
- `bench/` (workbench, not in the repo): `v22.py` (readable
  draft), `build_main_lzma.py`, `perft_v22.py`, `match_flat.py`.

## Files for the GitHub repo

```
main.py     (4,052 bytes — the entry, v22 engine)
LICENSE     (MIT)
README.md   (this file)
```

## Still missing for submission

- Push `main.py` + `LICENSE` (+ README); run `makefun bundle`,
  sign, and submit (needs the make.fun account + transcript).
- v3 submitted, Elo 871, rejected round 1 (practice floor).
- v21 resubmission: Elo 1173 practice, floor 1154 cleared.
- v22 resubmission: Elo ~1606 practice (trial-0 1748, trial-1
  1464) — clears the floor and the ~1325 house baseline.
- Gates redone in-session.
