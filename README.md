# Tiny Chess 4K

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Skymavis7/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard)
[![reward](https://make.fun/badge/tiny-chess-01/Skymavis7/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01)

A chess engine in a single `main.py` of 3,923 bytes, built for [make.fun](https://make.fun)'s tiny-chess-01: the strongest chess engine in 4 KB of Python.

## How it plays

0x88 board, negamax with alpha-beta search to a fixed 80,000-node budget per move, quiescence over captures and promotions, a transposition table, MVV-LVA capture ordering with killer moves, and material + piece-square evaluation with bishop-pair and passed-pawn bonuses. Fully deterministic — no clock, no unseeded randomness — so a replayed trial plays the same moves. The node budget was raised from 60,000 to 80,000 after local timing showed headroom (average ~1.5 s/move, worst 3.6 s, against a 10 s limit).

The readable source is packed as a `zlib` + base85 payload (allowed by the rules; payload and unpacker both count toward the 4,096 bytes).

## Test results (local)

- Perft: depth-3/4 correct on start, Kiwipete, and test positions; legal-move sets agree 60/60 on random positions.
- Determinism: same move for the same FEN across fresh processes.
- Timing: ~1.45 s/move average, worst observed 3.6 s (limit 10 s).
- Games: 6/6 wins vs the kit's greedy starter, 2/2 vs random, 3.5/4 vs a 10k-node copy of itself.

## Run it

`main.py` talks to the game env at `MAKEFUN_ENV_URL` (`POST /game`, `POST /move`, `POST /resign`) and plays every game of the gauntlet. Python 3.13, standard library only.
