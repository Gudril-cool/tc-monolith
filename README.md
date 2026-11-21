[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Gudril-cool/tc-monolith/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Gudril-cool/tc-monolith/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

# Monolith — tiny-chess-01 entry

A complete chess engine in a single `main.py` of 4,071 bytes. The engine
source is stored lzma-compressed (base85) inside `main.py` and unpacks and
execs itself at startup — a technique the competition rules explicitly
allow; everything else about the engine is ordinary Python 3 stdlib code.

## Design

- Mailbox-64 board, copy-make move application with full legality
  (castling, en passant, promotions, check detection).
- Iterative-deepening negamax alpha-beta under a fixed, deterministic
  budget of 20,000 nodes per move — no wall clock, no randomness in
  decisions, so trials replay byte-for-byte.
- Zobrist transposition table (seeded PRNG keys) with exact / lower /
  upper bound flags and TT-move ordering; MVV-ordered captures elsewhere.
- Quiescence search over captures, promotions and check evasions.
- Repetition detection along the search path.
- Evaluation: material + generated piece-square tables (rank/file
  components per piece) + pawn structure — doubled-pawn penalties,
  isolated-pawn penalties and passed-pawn bonuses by rank.

## Play

The engine talks to the environment over `MAKEFUN_ENV_URL`
(`POST /game`, `POST /move`) exactly as the starter does.

MIT licensed — see `LICENSE`.
