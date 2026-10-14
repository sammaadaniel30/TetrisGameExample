# Implementation plan for Claude

Use this document as the ordered implementation plan. Read `CLAUDE.md` and the linked requirements before writing source files. The goal is a working, small game, not a large framework.

## Phase 1 — Confirm the plan

1. Read `README.md`, `docs/requirements.md`, `docs/architecture.md`, `docs/game-rules.md`, `docs/diagrams.md`, `docs/testing.md` and `docs/build-and-release.md`.
2. Keep the design consistent with these documents.
3. Do not add unrequested features. In particular, do not add levels, increasing speed, persistent scores, a hold piece, a ghost piece, sound, accounts, online services or extra game modes.
4. Create the source folders and dependency file from the proposed structure in the architecture document.

**Checkpoint:** the implementation outline and filenames match the architecture document.

## Phase 2 — Implement core rules first

1. Implement Piece: the seven shapes, orientation and occupied board coordinates.
2. Implement Board: grid creation, placement checks, piece locking and complete-row removal.
3. Implement Game: current and next piece, score, fixed gravity, hard drop, pause/resume, round reset and game-over detection.
4. Use the published scoring table and board dimensions. Keep rendering out of these components.

**Checkpoint:** the main board and game-rule tests can run without opening the Pygame window.

## Phase 3 — Test before adding presentation

1. Add tests in the `tests/` folder using `unittest`.
2. Cover boundary movement, collision, all seven shapes, row clearing, scoring, hard drop, pause, spawn failure and resetting.
3. Run the automated test suite and fix failures before proceeding.

**Checkpoint:** all automated rule tests pass.

## Phase 4 — Add the desktop game

1. Implement the application window and main loop.
2. Implement the keyboard input handler using the controls in the game-rules document.
3. Implement drawing for the board, active piece, next-piece preview, score, paused state and game-over state.
4. Use elapsed time for fixed gravity and a stable target frame rate for drawing.
5. Keep the visual design simple, readable and high-contrast. Do not require downloaded assets.
6. Make Escape and the standard window close button close cleanly.

**Checkpoint:** every manual check in the testing document can be performed from the Python application.

## Phase 5 — Package for Windows

1. Add Pygame as the runtime dependency and PyInstaller as the build dependency in a clear dependency file.
2. Follow `docs/build-and-release.md`.
3. Build a single-file, windowed executable called `TetrisGameExample.exe` on Windows.
4. Test the actual executable, not only the Python script.
5. If the packaged application fails, temporarily create a console-enabled diagnostic build, fix the issue, then produce and retest the final windowed build.

**Checkpoint:** `dist/TetrisGameExample.exe` launches and passes the packaged-build checklist.

## Phase 6 — Finish and report

1. Update documentation only if an approved implementation detail genuinely differs from the plan.
2. Do not mark a test or packaged build as successful unless it was actually run.
3. Provide a concise completion report stating:
   - which source files were created;
   - which requirements are implemented;
   - the automated tests that were run and their result;
   - whether the Windows executable was built and where it is located;
   - which manual or platform checks remain incomplete.

## Implementation rules

- Use ordinary, readable Python and small methods.
- Keep one clear source of truth for movement validation, scoring, board dimensions and gravity interval.
- Prefer the standard library for tests and basic support logic.
- Avoid unnecessary inheritance, global mutable state, complicated patterns, network services and extra dependencies.
- Do not make the renderer mutate game state.
- Do not couple board or piece logic to Pygame.
- Keep any build output out of source files and do not commit large generated binaries unless explicitly requested.
