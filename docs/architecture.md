# Architecture

## 1. Design goal

Use a small, object-oriented design that a beginner can follow. Each part should have one clear job. The rules of Tetris must not depend on Pygame drawing functions, so they can be tested separately from the window.

This is intentionally not a large framework. Use ordinary Python classes and functions where they make the design clearer; do not add patterns or layers without a practical reason.

## 2. Technology choices

- **Python:** application language.
- **Pygame:** desktop window, drawing, events and frame timing.
- **PyInstaller:** packages the finished application for Windows.
- **unittest:** tests core rules using Python's standard testing library.

The game should not require an online service or external game assets.

## 3. Proposed folder structure

The implementation should create the following structure. This is a plan, not existing source code.

    TetrisGameExample/
    ├── README.md
    ├── CLAUDE.md
    ├── main.py
    ├── requirements.txt
    ├── tetris_game/
    │   ├── __init__.py
    │   ├── app.py
    │   ├── game.py
    │   ├── board.py
    │   ├── piece.py
    │   ├── input_handler.py
    │   ├── renderer.py
    │   └── settings.py
    ├── tests/
    │   ├── test_board.py
    │   └── test_game.py
    └── docs/
        ├── requirements.md
        ├── architecture.md
        ├── game-rules.md
        ├── diagrams.md
        ├── testing.md
        ├── implementation-plan.md
        └── build-and-release.md

The documentation files are already part of the specification. Claude should create the Python files and dependency file only when implementing the game.

## 4. Component responsibilities

| Component | Responsibility | Must not do |
| --- | --- | --- |
| main.py | The application entry point; starts the desktop application. | Contain the rules for moving pieces or clearing rows. |
| app.py | Creates and closes the Pygame window; runs the main loop; forwards input; asks the game to update; asks the renderer to draw. | Define the scoring rules or directly manipulate board rows. |
| game.py | Coordinates a round: current and next pieces, score, game state, gravity timing, locking pieces, spawning, pause, reset and game over. | Draw graphics or read raw Pygame events. |
| board.py | Stores the grid; checks whether a set of piece cells fits; locks cells; finds and removes full rows. | Know about fonts, colours or window surfaces. |
| piece.py | Defines the seven tetromino shapes, orientation and occupied cells at a board position. | Know about the user interface or score. |
| input_handler.py | Maps keyboard events to clear actions such as move left, rotate, hard drop, pause and quit. | Implement collision detection or line clearing. |
| renderer.py | Draws the board, active piece, next preview, score and state messages. | Change game state as a side effect of drawing. |
| settings.py | Holds shared constants such as board dimensions, cell size, colours, gravity interval and target frame rate. | Contain round-by-round mutable state. |
| tests/ | Checks board and gameplay rules, including important edge cases. | Depend on a visible game window for basic rule tests. |

## 5. Main data objects

### Board

- Stores 20 rows of 10 cells.
- Represents an empty cell consistently, for example with a null value, and stores a piece colour or piece identifier in an occupied cell.
- Offers simple operations to check piece positions, lock a piece and clear complete rows.
- Owns the grid; other components must not silently change its internal rows.

### Piece

- Knows its type, rotation state and grid position.
- Can provide the four board coordinates occupied by its current orientation.
- Uses defined orientations for each shape. A rotation is applied only when the board confirms that the resulting cells are valid.

### Game

- Owns the current Board, active piece, next-piece preview value, score and game state.
- Exposes gameplay actions such as move, rotate, soft drop, hard drop, pause/resume, start a new round and update elapsed time.
- Calls Board operations and applies the scoring table from the game-rules document.

## 6. Main update cycle

The application follows a simple loop:

1. Read window and keyboard events.
2. Convert key events into game actions.
3. Measure elapsed time since the previous frame.
4. Update game logic if the game is in the Playing state.
5. Draw the current state.
6. Repeat at a target of about 60 frames per second until the player quits.

Gravity must be based on elapsed time. The game must not fall faster merely because the computer renders more frames.

## 7. Dependency direction

The app coordinates the program. The renderer and input handler interact with Pygame. The game rules depend on the board and piece models, not on the renderer. The board and piece models should not import Pygame.

This keeps core rules testable on their own and avoids circular imports.

## 8. Error handling and simplicity

- Validate piece placement through one consistent board operation rather than duplicating collision rules in several places.
- Keep scoring in one obvious function or small section of the game logic.
- Keep board changes in Board methods.
- Avoid global mutable game state.
- Do not add a database, networking, dependency injection framework, plugin system or custom event bus.
- If a non-obvious technical choice is needed, comment on the reason briefly.

## 9. Packaging boundary

PyInstaller is a release tool, not part of the game loop. The game must first run correctly from Python. Packaging settings should be maintained in a repeatable build command or a PyInstaller specification file, and the final executable must be tested on Windows.
