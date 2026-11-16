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
  king safety switches to endgame table when material is low.
- **Repetition:** Game-history repetition returns draw score in search.

The source is LZMA-compressed and base85-encoded to fit the size limit
(the same technique the sunfish 4k baseline uses); `main.py` decompresses
and executes it at startup. Deterministic: fixed node budget, no clock,
no randomness.

## Protocol

Talks to the make.fun env server via `MAKEFUN_ENV_URL` (`/game`, `/move`).
