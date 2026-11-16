# Tiny Chess Engine

A compact chess engine in ≤4,096 bytes of Python (stdlib only).

## Design

- **Board:** 0x88 representation, pieces P=1 N=2 B=3 R=4 Q=5 K=6.
- **Search:** Negamax alpha-beta with iterative deepening (fixed 120,000-node
  budget per move), transposition table, killer moves, MVV-LVA capture
  ordering, null-move pruning (R=2) and late-move reductions.
- **Quiescence:** Captures and promotions only; all evasions when in check;
  delta pruning.
- **Evaluation:** Material + formula piece-square tables + bishop pair;
  king table tapers from middlegame to endgame by non-pawn material;
  pawn structure (doubled / isolated / passed-pawn bonus) and rook
  open / semi-open file bonus. King squares are tracked incrementally
  for fast check detection.
- **Repetition:** Game-history repetition returns draw score in search.

The readable source is `engine.py`; `main.py` is packed from it with the
kit's `makefun compress` (LZMA stub, `makefun-compress/v1`) to fit the size
limit. Deterministic: fixed node budget, no clock, no randomness.

## Results (v2)

Local practice trials (6 games vs the kit gauntlet, 2026-11-16):

- trial-0: 4W 0D 2L = 4.0/6, practice Elo **1451** (wall 1182 s,
  slowest single move 6.1 s, limit 10 s).
- trial-1: 3W 1D 2L = 3.5/6, practice Elo **1364** (wall 1242 s,
  slowest single move 5.3 s).

v1 scored 1099 on the server (below the 1154 practice floor); v2 fixes
four search/eval bugs (pawn-attack direction, king taper, MVV-LVA values,
en-passant unmake) and adds null-move pruning, LMR, pawn-structure and
rook-file eval on a 120k-node budget.

## Protocol

Talks to the make.fun env server via `MAKEFUN_ENV_URL` (`/game`, `/move`).
