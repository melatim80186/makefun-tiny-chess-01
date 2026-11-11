# tiny-chess-01 — make.fun entry

$500, 20 paid places. Deadline **2026-11-25 00:00 WIB**.

## The entry

- `main.py` — **3,992 bytes** (limit 4,096). Python 3.13 stdlib only,
  self-contained, deterministic (fixed 100,000-node budget, no clock,
  no randomness). No other repo file is read.
- `main.py` is a compressed self-extracting engine: a zlib + base85
  payload with its unpacker (the rules explicitly allow "any
  compressed payload with its unpacker included"). Readable source:
  `bench/bb2.py` in the workbench (not part of the repo).
- `LICENSE` — MIT. Entry = public GitHub repo (created after the
  competition opened) + AI session transcript.

## How it works

Bitboard engine (Python ints, squares 0–63, a1 = 0). Negamax
alpha-beta, iterative deepening (depth 1–8) on a shared 100,000-node
budget, transposition table, null-move pruning (R = 2), late-move
reductions, quiescence search, MVV-LVA + TT-move ordering, formula
piece-square tables. Full legal move generation. Talks to the env at
`MAKEFUN_ENV_URL` (`POST /game`, `POST /move`), syncs by `start_fen`
+ replaying `moves`, plays only from the env's `legal_moves`.

## Strength — honest assessment

Much stronger than round 2, but still below the house baseline
(sunfish's 4 KB engine, ~1300 Elo) and below the 1000 target, so a
prize is unlikely.

- **Gauntlet `bench/runs/bb2-t0/` (trial 0, 16 games):
  Elo 871, 2 wins / 0 draws / 14 losses**
  (`{"elo":871,"working":1,"wins":2,"draws":0,"losses":14,
  "spare_plies":263}`). Trial took 43 min (limit 60).
- Direct match vs round 2 at 20k nodes: **10.5/12 (+338 Elo)**.
- Think time ~4.3 s/move; **0 moves over the 10 s limit**. The node
  budget binds: the engine uses all 100,000 nodes every move and
  never reaches depth 8.
- Rounds: v1 **562** (0/1/15) → v2 **705** (1/0/15) → v3 **871**
  (2/0/14). Reference v6 (23 KB, too big) is 1173.
- Why still weak: ~depth 5–6 in the middlegame at 100k nodes, vs
  up to 2,000,000 nodes/move for the strongest house engines.
  Raising the budget is the clearest next step, but the 60-minute
  trial limit caps it (a 40% raise would use ~52 min).

## Verification done

- Perft vs python-chess, 6 standard positions to depth 3–4 —
  **all counts exact**. Golfed engine plays the same moves as the
  readable draft on 6 test positions (6/6 at 20k nodes).
- `bench/` (workbench, not part of the submission): `bb1.py`
  (readable draft), `bb2.py` (golfed engine — the payload inside
  `main.py`), `build_main.py` (reproduces `main.py`), `ROUND3.md`.

## Files for the GitHub repo

```
main.py     (3,992 bytes — the entry)
LICENSE     (MIT)
README.md   (this file)
```

## Still missing for submission

- Create the public GitHub repo under `melatim80186`; push
  `main.py` + `LICENSE` (+ README).
- Create `.makefun/owner.json` with the payout wallet, run
  `makefun bundle`, sign, and submit. Needs the make.fun account
  and the AI session transcript.
- Do NOT submit yet — the parent agent decides submit vs drop.
