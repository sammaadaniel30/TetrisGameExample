# Instructions for Claude: TetrisGameExample

You are implementing a small Tetris-style Windows desktop game. Treat the existing documentation as the agreed product specification.

## Read first

Read these documents before writing implementation code:

1. `README.md`
2. `docs/requirements.md`
3. `docs/architecture.md`
4. `docs/game-rules.md`
5. `docs/diagrams.md`
6. `docs/testing.md`
7. `docs/implementation-plan.md`
8. `docs/build-and-release.md`

## Product decisions that must not drift

- Language: Python.
- Desktop/graphics library: Pygame.
- Packaging: PyInstaller.
- Target: Windows 10 and Windows 11.
- Final executable name: `TetrisGameExample.exe`.
- Playfield: 10 columns by 20 visible rows.
- Pieces: I, O, T, S, Z, J, L.
- Controls: Left/Right move, Down soft drop, Up clockwise rotation, Space hard drop, P pause/resume, Enter starts a new round after game over, Escape quits.
- Difficulty: fixed gravity interval; do not add levels or speed increases.
- Scoring: use the exact table in `docs/game-rules.md`.
- Features: next-piece preview, pause/resume, hard drop, standard scoring and restart after game over.
- Persistence: no saved high scores.
- Architecture: simple and modular. Keep game rules independent of Pygame drawing and keyboard events.

## Scope limits

Do not add sound, music, a settings menu, configurable controls, a hold-piece feature, a ghost piece, achievements, online leaderboards, user accounts, multiplayer, networking, extra modes, or levels unless the owner explicitly changes the specification.

Do not build a large framework or introduce design patterns without a clear need. Prefer clear names, small responsibilities, simple data structures and focused tests.

## Source-code constraints

- Follow the file responsibilities in `docs/architecture.md`.
- Board and Piece logic must not import Pygame.
- Renderer must display the game state without changing it.
- Gravity must be based on elapsed time, not frame count.
- Collision validation must be consistent for movement, rotation, gravity and hard drop.
- Keep board dimensions, colours, cell size, gravity interval and target frame rate in one settings module.
- Do not add internet access or external assets.
- Use Python's `unittest` for automated rule tests.
- Keep `README.md` and other documents; do not replace them with generated boilerplate.
- Do not place generated executable or build output in source modules.

## Work order

Follow the phases in `docs/implementation-plan.md`. Implement and test the game logic before building the interface; then run the complete tests and package on Windows.

## Completion report

At the end, report which requirements are implemented, the exact tests run and their results, whether the Windows executable was genuinely built and tested, and any remaining limitations. Never report a successful build or test that you did not run.
