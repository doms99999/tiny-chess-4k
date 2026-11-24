# Tiny Chess — a chess engine in 2.7 KB of Python

[![make.fun bounty](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/doms99999/tiny-chess-4k/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/doms99999/tiny-chess-4k/reward.svg)](https://make.fun/bounty/tiny-chess-01)

An original chess engine written for the make.fun **tiny-chess-01** bounty
("Build the strongest chess engine in 4 KB of Python"). `main.py` is the whole
engine — 2,735 bytes as committed, an lzma-packed payload with its unpacker,
packed with the competition kit's own `makefun compress`, as the rules allow.

Inside the payload (~5.8 KB of golfed Python):

- 0x88 board, fully legal move generation: castling with attacked-square
  checks, en passant (pins included), all four promotions. Verified against
  the standard perft suite (start position, Kiwipete, the en-passant-pin
  position, the promotion positions) to depth 4.
- Negamax alpha-beta with iterative deepening under a fixed budget of 60,000
  nodes — no clock, no randomness, so every replay reproduces move for move.
- Quiescence over captures and promotions, extending every line that leaves a
  king in check.
- MVV-LVA capture ordering and promotion ordering.
- Evaluation: material, seven piece-square tables (pawn, knight, bishop,
  rook, queen, and separate middlegame/endgame king tables blended by a
  material phase), and the bishop pair.

It plays through the competition env at `MAKEFUN_ENV_URL` (`POST /game`,
`POST /move`), choosing each move from the position the env serves and
falling back to the env's first legal move if its own choice were ever
refused.
