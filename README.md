# tiny-chess-01 — make.fun entry

$500, 20 paid places. Deadline **2026-11-25 00:00 WIB**.

## The entry

- `main.py` — **4,094 bytes** (limit 4,096). Python 3.13 stdlib only,
  self-contained, deterministic (fixed 100,000-node budget, no clock,
  no randomness). No other repo file is read.
- `main.py` is a compressed self-extracting engine: a zlib + base85
  payload with its unpacker (the rules explicitly allow "any
  compressed payload with its unpacker included"). Readable source:
  `bench/v21.py` in the workbench (not part of the repo).
- `LICENSE` — MIT. Entry = public GitHub repo (created after the
  competition opened) + AI session transcript.

## How it works

0x88 Sunfish-style engine (120-square board, full 64-square
piece-square tables for every piece). Negamax alpha-beta, iterative
deepening on a shared 100,000-node budget, transposition table,
null-move pruning, late-move reductions, quiescence search, MVV-LVA
+ TT-move ordering. Full legal move generation. Talks to the env at
`MAKEFUN_ENV_URL` (`POST /game`, `POST /move`), syncs by `start_fen`
+ replaying `moves`, plays only from the env's `legal_moves`.

## Strength — honest assessment

v21 clears the practice floor: **practice Elo 1173** (floor 1154),
from an 8-game practice run — **2 wins, 1 draw, 5 losses** in
1,277.6 s total, with **no time losses**. The old bitboard line
topped out at 871 (v3); the 0x88 rewrite with full piece-square
tables is what got it over the floor.

- **Practice run (v21, 8 games): Elo 1173, 2 wins / 1 draw /
  5 losses** (1,277.6 s, 0 moves over the 10 s limit).
- Rounds: v1 **562** (0/1/15) → v2 **705** (1/0/15) → v3 **871**
  (2/0/14) → v21 **1173** (2/1/5, practice).
- Why still modest: ~depth 5–6 in the middlegame at 100k nodes, vs
  up to 2,000,000 nodes/move for the strongest house engines.
  Raising the budget is the clearest next step, but the 60-minute
  trial limit caps it (a 40% raise would use ~52 min).

## Verification done

- Perft vs python-chess, 6 standard positions to depth 3–4 —
  **all counts exact**. Golfed engine plays the same moves as the
  readable draft on 6 test positions (6/6 at 20k nodes).
- `bench/` (workbench, not part of the submission): `v21.py`
  (readable draft of the v21 engine — the payload inside
  `main.py`), `build_main.py` (reproduces `main.py`), `ROUND3.md`.

## Files for the GitHub repo

```
main.py     (4,094 bytes — the entry, v21 engine)
LICENSE     (MIT)
README.md   (this file)
```

## Still missing for submission

- Create the public GitHub repo under `melatim80186`; push
  `main.py` + `LICENSE` (+ README).
- Create `.makefun/owner.json` with the payout wallet, run
  `makefun bundle`, sign, and submit. Needs the make.fun account
  and the AI session transcript.
- v3 submitted, Elo 871, rejected round 1 (practice floor).
- v21 resubmission: Elo 1173 practice, floor 1154 cleared.
- Gates redone in-session.
