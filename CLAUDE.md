# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project

Single-file browser Tetris (`tetris.html`). No build step, no dependencies, no package manager.
Firestore leaderboard is integrated (scores saved on game over, top 10 ranking displayed).

To run: open `tetris.html` directly in a browser.

## File sync rule (IMPORTANT)

`tetris.html` is the single source of truth. After editing it, always sync before deploying:
```bash
cp tetris.html public/index.html && cp tetris.html public/tetris.html
```

## Hosting

- **Game**: Firebase Hosting — `firebase deploy --only hosting`
- **Docs (architecture spec)**: GitHub Pages from `docs/` folder (push to main to update)
- Live URL: https://practice-day1-osaka.web.app

## Architecture

Everything lives in `tetris.html` as inline `<script>` and `<style>`. The JS is structured as classes:

- **`Bag`** — 7-bag randomizer (Fisher-Yates shuffle over `['I','O','T','S','Z','J','L']`)
- **`Piece`** — holds `type`, `x`, `y`, `rot`; `.cells()` returns board coordinates
- **`Board`** — 2D grid (`g[row][col]`); `valid()`, `lock()`, `clearLines()`
- **`Game`** — game state, timers, DAS, scoring; `update(dt)` is the logic tick
- **`render(game)`** — standalone function; draws board canvas, ghost, active piece, hold/next previews, overlay, flash message
- **`saveScore(name, score, level, lines)`** — async; writes to Firestore `scores` collection
- **`loadRanking()`** — async; fetches top 10 from Firestore ordered by score desc

The main loop calls `game.update(dt)` then `render(game)` every `requestAnimationFrame`.

## Game states

`game.state` can be: `idle | running | paused | over | ranking`

- `over` — game ended; overlay shows name entry form (auto-focuses input)
- `ranking` — after name submit or skip; loads and displays Firestore top 10
- SPACE key: starts from `idle`, restarts from `ranking`. Does NOT restart from `over` (user must use form).

## Firestore

- **Project**: `practice-day1-osaka` (GCP)
- **Collection**: `scores` — fields: `name` (string ≤10), `score` (number), `level`, `lines`, `date` (serverTimestamp)
- **Rules file**: `firestore.rules` — read: public, create: validated, update/delete: denied
- **SDK**: Firebase compat v10 loaded via CDN (no build step)
- `db` is `null` if Firebase init fails; all Firestore calls check for this and degrade gracefully

## Key constants

- `COLS=10`, `ROWS=20`, `BS` (block size, dynamic based on viewport)
- `SHAPES` — all 4 rotations pre-defined per piece type
- `KJ` / `KI` — SRS wall-kick tables in **screen coordinates** (y-down). These are derived from TetrisWiki (y-up) by negating the y component. Do not mix up coordinate systems when editing.

## Spawn positions

- I piece: `x=3, y=-1` (blocks are at shape row 1, so they appear at board row 0)
- All other pieces: `x=3, y=0`

## Scoring

`LINE_PTS = [0, 100, 300, 500, 800]` × `level`. Back-to-back Tetris ×1.5. Combo: `50 × combo × level`. Hard drop: 2pts/row, soft drop: 1pt/row.
