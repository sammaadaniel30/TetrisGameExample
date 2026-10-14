# Requirements

This document describes what the first version must do. Requirement IDs make it easier to discuss and test each behaviour. The wording is intentionally formal but readable.

## 1. Project goal

Create a small Tetris-style desktop game using Python and Pygame. It must be enjoyable to play, easy to understand, and packageable as a standalone Windows executable.

## 2. Functional requirements

| ID | Requirement |
| --- | --- |
| FR-01 | When launched, the game shall open a desktop game window and begin a new round. |
| FR-02 | The playfield shall contain 10 columns and 20 visible rows. |
| FR-03 | The game shall provide the seven standard tetromino shapes: I, O, T, S, Z, J and L. Each piece shall occupy four cells. |
| FR-04 | The player shall be able to move the active piece left and right when the movement does not overlap occupied cells or leave the playfield. |
| FR-05 | The player shall be able to rotate the active piece clockwise when the resulting position is valid. An invalid rotation shall leave the piece unchanged. |
| FR-06 | The Down key shall move the active piece down by one cell when that move is valid. |
| FR-07 | The Space key shall perform a hard drop: move the active piece to the lowest valid position and lock it there immediately. |
| FR-08 | A piece shall fall automatically at a fixed time interval while the game is playing. The interval shall not become faster as the score increases. |
| FR-09 | When a piece can no longer move downward, the game shall lock it into the board. |
| FR-10 | After a piece locks, every completely filled row shall be removed, the rows above shall move downward, and the board shall remain consistent. |
| FR-11 | The game shall award points based on the number of rows cleared in one lock and shall display the current score. |
| FR-12 | The interface shall show a preview of the next piece. |
| FR-13 | Pressing P during play shall pause the game. Pressing P again shall resume it. While paused, the board shall not advance and gameplay controls shall not move or rotate pieces. |
| FR-14 | The game shall enter the game-over state when a newly created piece cannot be placed at its starting position. |
| FR-15 | The game-over screen shall show that the round has ended and allow the player to start another round with Enter. A new round shall reset the board and score. |
| FR-16 | Pressing Escape shall close the game cleanly. |
| FR-17 | The game shall show the current score and the paused or game-over state clearly enough for a player to understand what is happening. |

## 3. Non-functional requirements

| ID | Requirement |
| --- | --- |
| NFR-01 | The game shall run as a desktop application on Windows 10 and Windows 11. |
| NFR-02 | The final release shall include a single file named TetrisGameExample.exe. |
| NFR-03 | A player shall not need to install Python or Pygame to run the packaged executable. |
| NFR-04 | The first version shall not require an account, a server, an internet connection or network access. |
| NFR-05 | The architecture shall separate game rules from drawing and keyboard handling so that core rules can be tested without opening a game window. |
| NFR-06 | The implementation shall use clear names, small responsibilities and comments where a decision is not obvious. It shall avoid unnecessary design patterns and extra layers. |
| NFR-07 | Important game rules shall have automated tests. |
| NFR-08 | The game shall respond to input during normal play and render at a stable target frame rate without tying falling speed to frame rate. |
| NFR-09 | The documentation shall explain the game to both players and developers in plain English. |

## 4. Out of scope for the first release

The first release shall not include online multiplayer, network leaderboards, saved high scores, user accounts, sound or music, extra game modes, user-configurable controls, a hold-piece feature, a ghost piece, achievements or a level-based speed increase.

## 5. Acceptance criteria

The project is accepted when all of the following are true:

1. The game can be started and closed without an unexpected crash.
2. All seven pieces can appear, move, rotate where the resulting position is valid, fall and lock.
3. Pieces never pass through the board boundaries or occupied cells.
4. Completed rows disappear correctly, including when several rows are cleared at once.
5. The score follows the published scoring table in the game-rules document.
6. The next-piece preview matches the next piece that appears.
7. Pause freezes game progression and resume continues the same round.
8. Hard drop locks the piece at its lowest valid position.
9. A blocked spawn produces game over; Enter starts a clean new round.
10. Automated rule tests pass.
11. The packaged executable runs on a Windows machine without a separate Python installation.
12. The executable launches as a game window rather than opening a command prompt.

## 6. Requirement change rule

If implementation reveals that a requirement needs to change, update this document and the relevant game-rules or testing document first. Do not silently add features or change scoring, controls or difficulty.
