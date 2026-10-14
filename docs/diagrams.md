# Simple diagrams

These diagrams describe the planned design. They are deliberately small so a player, beginner or reviewer can understand them without reading the source code.

## 1. Component diagram

The application coordinates input, game rules and drawing. The game rules own the board and active piece. Drawing displays the current state but does not change it.

```mermaid
flowchart TD
    Main[main.py] --> App[Application]
    App --> Input[Keyboard input]
    App --> Game[Game coordinator]
    App --> Renderer[Renderer]
    Game --> Board[Board]
    Game --> Piece[Piece]
    Game --> Settings[Settings]
    Renderer --> Settings
```

**In plain English:** the application listens for keyboard actions and asks the Game to update. The Renderer displays the result. Board and Piece contain game data and rules; they do not draw anything.

## 2. Simplified class diagram

```mermaid
classDiagram
    class Application {
        +run()
        +close()
    }
    class Game {
        +move_left()
        +move_right()
        +rotate()
        +soft_drop()
        +hard_drop()
        +toggle_pause()
        +start_new_round()
        +update(elapsed_time)
    }
    class Board {
        +can_place(piece_cells)
        +lock_piece(piece_cells)
        +clear_full_rows()
    }
    class Piece {
        +get_cells()
        +get_rotated_cells()
    }
    class Renderer {
        +draw(game)
    }
    class InputHandler {
        +handle_event(event)
    }
    Application --> Game : updates
    Application --> Renderer : draws
    Application --> InputHandler : reads events
    Game --> Board : checks and changes
    Game --> Piece : controls
    Renderer ..> Game : reads current state
    InputHandler ..> Game : sends actions
```

The names are a guide to responsibilities, not a demand to implement every method with exactly this spelling. Keep the final design small and internally consistent.

## 3. Game-state diagram

```mermaid
stateDiagram-v2
    [*] --> Playing : launch / new round
    Playing --> Paused : press P
    Paused --> Playing : press P
    Playing --> GameOver : new piece cannot spawn
    GameOver --> Playing : press Enter / new round
    Playing --> [*] : press Escape or close window
    Paused --> [*] : press Escape or close window
    GameOver --> [*] : press Escape or close window
```

**Important rule:** the board and gravity timer do not advance while Paused. Escape and the window close button work in every state.

## 4. Hard-drop sequence

```mermaid
sequenceDiagram
    actor Player
    participant Input as Input handler
    participant Game
    participant Board
    participant Renderer

    Player->>Input: Press Space
    Input->>Game: hard_drop()
    Game->>Board: Check next lower position
    loop Until next move is blocked
        Board-->>Game: Position is valid
        Game->>Game: Move piece down one row
        Game->>Board: Check next lower position
    end
    Board-->>Game: Position is blocked
    Game->>Board: Lock piece and clear full rows
    Game->>Game: Update score and spawn next piece
    Game-->>Renderer: Updated game state
    Renderer-->>Player: Draw the result
```

## 5. Implementation flow

```mermaid
flowchart TD
    A[Read requirements] --> B[Implement board and pieces]
    B --> C[Test movement and row clearing]
    C --> D[Implement game states and scoring]
    D --> E[Add keyboard and rendering]
    E --> F[Run the complete game]
    F --> G[Run tests and manual checks]
    G --> H[Build Windows executable]
    H --> I[Test executable on Windows]
```

If the implementation plan changes materially, update these diagrams so they continue to match the code.
