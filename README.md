# Tiny Chess Engine

A compact chess engine in ≤4,096 bytes of Python (stdlib only).

## Design

- **Board:** 0x88 representation, pieces P=1 N=2 B=3 R=4 Q=5 K=6.
- **Search:** Negamax alpha-beta with iterative deepening (fixed 85,000-node
  budget per move), transposition table, killer moves, MVV-LVA capture
  ordering.
- **Quiescence:** Captures and promotions only; all evasions when in check;
  delta pruning.
- **Evaluation:** Material + formula piece-square tables + bishop pair;
  king table tapers from middlegame to endgame by non-pawn material.
- **Repetition:** Game-history repetition returns draw score in search.

The readable source is `engine.py`; `main.py` is packed from it with the
kit's `makefun compress` (LZMA stub, `makefun-compress/v1`) to fit the size
limit. Deterministic: fixed node budget, no clock, no randomness.

## Protocol

Talks to the make.fun env server via `MAKEFUN_ENV_URL` (`/game`, `/move`).
