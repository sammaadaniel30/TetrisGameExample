# Game rules

This document defines the rules the player experiences. It is also the source of truth for implementing and testing gameplay.

## 1. Playfield and pieces

- The visible playfield has **10 columns and 20 rows**.
- Each cell is either empty or occupied by one coloured square.
- The seven pieces are I, O, T, S, Z, J and L. Each contains exactly four squares.
- A newly spawned piece appears near the top of the playfield.
- The next-piece preview shows the shape that will spawn after the active piece locks.
- Piece selection is random. The first version does not need a more advanced piece-bag algorithm.

## 2. Movement and rotation

- **Left / Right:** attempt to move the active piece by one column.
- **Down:** attempt to move the active piece down by one row.
- **Up:** attempt a 90-degree clockwise rotation.
- **Space:** move the active piece down until the next downward move would be invalid, then lock it immediately.
- Movement or rotation must not allow a piece to overlap an occupied cell or leave the playfield.
- If a rotation would be invalid, the piece stays in its current orientation and position.
- To keep the first version simple, advanced wall-kick rules are not required.
- A piece locks when it cannot move down further during gravity or after a hard drop.

## 3. Gravity and fixed difficulty

- Automatic gravity attempts to move the active piece down at a constant interval.
- The initial target interval is **0.6 seconds per row**. Keep this value in one settings location so it can be adjusted easily during testing.
- The gravity timer must use elapsed time, not the number of rendered frames.
- The falling interval shall stay the same regardless of score or rows cleared.
- The Down key is a manual one-row move and does not change the automatic gravity interval.

## 4. Line clearing

After a piece locks:

1. Find every row whose 10 cells are occupied.
2. Remove all complete rows found in that lock operation.
3. Move all rows above each removed row downward, keeping their order.
4. Add points for the number of rows cleared at once.
5. Create the next active piece and update the next-piece preview.

If no row is complete, no line-clear points are added. The line-clearing operation must handle adjacent complete rows without skipping any.

## 5. Scoring

The score starts at zero and is reset at the beginning of each new round.

| Rows cleared by one piece | Points awarded |
| --- | ---: |
| 0 | 0 |
| 1 | 100 |
| 2 | 300 |
| 3 | 500 |
| 4 | 800 |

Points depend on the number of rows cleared by a single lock operation. The score does not increase for moving, rotating, soft dropping or hard dropping. The first version has no levels and no score multiplier.

## 6. Game states

The game uses three visible states:

- **Playing:** the piece falls, gameplay input is accepted, and the board can change.
- **Paused:** gravity and board progression stop. P resumes the round. Escape still quits.
- **Game over:** a new piece cannot fit in its starting position. The game-over message is shown, and Enter begins a new round.

The application may also be in the process of closing, but closing is not a gameplay state.

## 7. Resetting and quitting

Starting a new round resets the board, score, current piece, next-piece preview, gravity timer and state. The round starts at the fixed initial speed.

Escape quits from any state. Closing the window through its standard window close button must also quit cleanly.

## 8. User interface guidance

The window should be simple and readable:

- Draw the playfield as a grid with clear cell boundaries.
- Use a different consistent colour for each piece type.
- Show the current score and next-piece preview next to the board.
- Show a clear PAUSED message when paused.
- Show GAME OVER and the Enter-to-play-again instruction after the round ends.
- Use a readable font size and strong contrast.
- Do not require images, downloaded fonts, sounds or other external assets for the first version.

Exact colours, font choice and window dimensions may be chosen by the implementer as long as the interface remains easy to read and the board fits within the window.
