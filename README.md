# TetrisGameExample

A small, classic Tetris-style block puzzle for Windows. This repository is also an example project for exploring how AI assistants can help plan and develop software.

> **Project status:** This is the documentation and implementation specification. The game source code and packaged Windows executable have not been created yet.

## What the game will include

- A 10-column by 20-row playfield and the seven familiar four-square pieces.
- Move, rotate, soft-drop and hard-drop controls.
- Completed-line clearing and a visible score.
- A preview of the next piece.
- Pause and resume.
- A fixed falling speed, so the game does not become faster as the score increases.
- A game-over screen and a way to start a new round.
- A standalone Windows executable named **TetrisGameExample.exe**.

The first version is intentionally small. It will not need an online account, internet access, saved high scores, sound, a settings menu, a hold-piece feature, a ghost piece or multiple game modes.

## How to play

| Key | Action |
| --- | --- |
| Left / Right | Move the active piece |
| Down | Move the piece down one cell |
| Up | Rotate the piece clockwise |
| Space | Drop the piece immediately |
| P | Pause or resume |
| Enter | Start a new round after game over |
| Escape | Quit the game |

## Technology

- **Python** for the application and game logic.
- **Pygame** for the desktop window, drawing, keyboard events and main loop.
- **PyInstaller** to package the working application as a Windows executable.

The intended final deliverable is a single-file Windows executable. The build must be performed and tested on Windows; creating an executable on another operating system does not produce a Windows build.

## Documentation

- [Requirements](docs/requirements.md) — numbered requirements written in plain language.
- [Architecture](docs/architecture.md) — project structure and what each component is responsible for.
- [Game rules](docs/game-rules.md) — controls, scoring, piece movement and game states.
- [Diagrams](docs/diagrams.md) — simple component, class, state and gameplay diagrams.
- [Testing plan](docs/testing.md) — important test cases and release acceptance checks.
- [Implementation plan for Claude](docs/implementation-plan.md) — a step-by-step plan for building the game.
- [Windows build and release](docs/build-and-release.md) — instructions for testing and packaging the application.
- [Claude project instructions](CLAUDE.md) — implementation guardrails and the order in which to read the documents.

## For players

When the game is implemented, players should be able to launch it by double-clicking the executable. Python will not need to be installed separately on the player's computer. No network connection is required.

## Current scope

The first release prioritizes understandable code and reliable core gameplay over extra features. Requirements in the documentation define the intended behaviour. If a proposed change adds a feature or changes a rule, update the documentation before implementing that change.
