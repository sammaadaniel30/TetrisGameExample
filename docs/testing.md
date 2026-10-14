# Testing plan

The goal is to catch gameplay mistakes before packaging. Tests should be repeatable and should not need a visible game window for core rules.

## 1. Approach

Use Python's built-in `unittest` framework for board and game logic. Keep tests for collision, rotation, row clearing, scoring, pause and game-over decisions independent of rendering whenever practical.

A smaller set of manual checks validates keyboard input, the appearance of the window and the final Windows executable.

## 2. Automated test cases

The IDs below are test cases, not new product requirements.

| Test ID | What to check | Expected result |
| --- | --- | --- |
| T-01 | Create a new board. | It has 20 rows and 10 columns, all initially empty. |
| T-02 | Check each of the seven pieces. | Each piece occupies exactly four cells. |
| T-03 | Move a piece left or right in open space. | Its column changes by exactly one. |
| T-04 | Move a piece beyond the left or right boundary. | The invalid move is rejected; no cell leaves the board. |
| T-05 | Move or rotate a piece into an occupied cell. | The invalid placement is rejected and the piece stays valid. |
| T-06 | Rotate a piece in open space. | Its orientation changes clockwise; its occupied cells remain four squares. |
| T-07 | Attempt a rotation that would leave the board. | The rotation is rejected without corrupting the piece. |
| T-08 | Lock a piece. | Its cells become occupied on the board. |
| T-09 | Fill one row and clear it. | The row disappears and rows above move down correctly. |
| T-10 | Clear two or more adjacent complete rows. | Every complete row is removed in that operation. |
| T-11 | Clear no complete rows. | The board remains the same except for the newly locked piece. |
| T-12 | Check scoring for one, two, three and four cleared rows. | The score increases by 100, 300, 500 and 800 respectively. |
| T-13 | Hard drop a piece. | The piece locks at the lowest valid position and cannot overlap existing cells. |
| T-14 | Pause the game and advance elapsed time. | Board, active piece and gravity progression stay unchanged. |
| T-15 | Resume after pause. | The same round continues normally. |
| T-16 | Force a blocked spawn position. | The game enters GameOver instead of placing overlapping pieces. |
| T-17 | Start a new round after game over. | Board and score reset, and a new active and next piece are available. |
| T-18 | Advance the game with different elapsed-time intervals. | Gravity is based on elapsed time, not rendering frame count. |
| T-19 | Reset a round. | Score starts at zero and speed returns to the fixed initial interval. |

The implementation should use clear, small test helpers to prepare specific board states. Avoid relying on random outcomes to prove a rule.

## 3. Manual gameplay checks

1. Launch the game from Python. The board, score and next-piece preview are visible.
2. Use Left and Right repeatedly. The piece stays within the walls.
3. Use Up to rotate several shapes. Invalid rotations do not break the board.
4. Hold Down to make repeated one-row downward moves by pressing it repeatedly.
5. Press Space and verify the piece locks immediately.
6. Fill one complete row and then more than one row. Check that rows disappear and the score matches the rules.
7. Press P. The game freezes visibly; press P again and verify it resumes.
8. Allow pieces to stack until the next piece cannot spawn. The game-over message appears.
9. Press Enter. A fresh board and zero score appear.
10. Press Escape and separately test the window close button. Both exit cleanly.
11. Play for several minutes. Check that the fall speed remains constant and controls continue to respond.

## 4. Packaged-build checks

After building `dist/TetrisGameExample.exe` on Windows:

- Double-click the executable from File Explorer; no command prompt should appear.
- Run it from a directory outside the repository.
- Confirm all controls, drawing, line clearing and pause work just as they do from Python.
- Confirm no missing asset or module error appears.
- Confirm Escape and the window close button work.
- Test on a Windows 10 or Windows 11 machine that does not have Python installed, where available.
- Rebuild from a clean checkout or clean build directory and repeat the checks.

## 5. Test completion

A release is not ready if an automated test fails, a manual gameplay check fails, or the packaged executable has not been launched and tested on Windows. A successful packaging command alone does not prove that the game works.
