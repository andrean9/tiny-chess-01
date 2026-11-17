# Tiny Chess Engine

A deterministic chess engine in 3,627 bytes of Python 3.13, using only the standard library.

- 0x88 board, iterative-deepening alpha-beta search, quiescence, transposition table, null-move pruning and late-move reductions.
- Fixed 60,000-node limit per move. No clock-based decisions, randomness or model calls.
- Precomputed tapered piece-square evaluation, pawn structure and bishop pair.
- `main.py` is self-contained. Its readable source travels with the build transcripts, outside the committed repository size cap.

## Validation

Practice trial-1: Elo 1654, five wins, no draws, one checkmate loss, zero timeouts.

Final Python 3.13 smoke checks pass: initial-position perft 20/400/8902, Kiwipete 48/2039/97862, special moves, incremental hash restoration, and repeated deterministic searches at the full node budget. Nine timed searches across opening, middlegame and endgame positions stayed below the 10-second move limit on the local machine. Server grading remains authoritative.

## Run

Set `MAKEFUN_ENV_URL` to the competition environment and run:

```sh
python3.13 -B -P -s main.py
```

The engine requests games at `/game` and sends UCI moves to `/move`.
