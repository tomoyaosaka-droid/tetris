# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser Tetris (`tetris.html`). No build step, no dependencies, no package manager.

To run: open `tetris.html` directly in a browser.

## Architecture

Everything lives in `tetris.html` as inline `<script>` and `<style>`. The JS is structured as classes:

- **`Bag`** — 7-bag randomizer (Fisher-Yates shuffle over `['I','O','T','S','Z','J','L']`)
- **`Piece`** — holds `type`, `x`, `y`, `rot`; `.cells()` returns board coordinates
- **`Board`** — 2D grid (`g[row][col]`); `valid()`, `lock()`, `clearLines()`
- **`Game`** — game state, timers, DAS, scoring; `update(dt)` is the logic tick
- **`render(game)`** — standalone function; draws board canvas, ghost, active piece, hold/next previews, overlay, flash message

The main loop calls `game.update(dt)` then `render(game)` every `requestAnimationFrame`.

## Key constants

- `COLS=10`, `ROWS=20`, `BS` (block size, dynamic based on viewport)
- `SHAPES` — all 4 rotations pre-defined per piece type
- `KJ` / `KI` — SRS wall-kick tables in **screen coordinates** (y-down). These are derived from TetrisWiki (y-up) by negating the y component. Do not mix up coordinate systems when editing.

## Spawn positions

- I piece: `x=3, y=-1` (blocks are at shape row 1, so they appear at board row 0)
- All other pieces: `x=3, y=0`

## Scoring

`LINE_PTS = [0, 100, 300, 500, 800]` × `level`. Back-to-back Tetris ×1.5. Combo: `50 × combo × level`. Hard drop: 2pts/row, soft drop: 1pt/row.
