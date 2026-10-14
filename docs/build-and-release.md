# Windows build and release

This guide describes how the completed Python game should be packaged and checked. The steps are intended for Windows PowerShell.

## 1. Requirements

The developer machine needs:

- Windows 10 or Windows 11.
- A compatible installed Python 3 version.
- A working project checkout with `main.py` and `requirements.txt`.
- An internet connection while installing development dependencies, unless they are already available.

The player should not need Python or Pygame installed after the executable has been packaged successfully.

## 2. Create a development environment

Open PowerShell in the repository folder and run:

```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
python -m pip install --upgrade pip
python -m pip install -r requirements.txt
```

If PowerShell blocks activation due to local execution policy, use a permitted local development approach or invoke the environment's Python directly at `.venv\Scripts\python.exe`. Do not change a system-wide security policy just for this project.

## 3. Run and test before packaging

Start the game from its Python entry point:

```powershell
python main.py
```

Run the automated tests:

```powershell
python -m unittest discover -s tests -v
```

Resolve errors and complete the manual checks in `docs/testing.md` before creating a release. Packaging should not be used to hide a broken source application.

## 4. Build the executable

Run the build on Windows, from the repository root:

```powershell
python -m PyInstaller --noconfirm --clean --onefile --windowed --name TetrisGameExample main.py
```

The expected executable is:

```text
dist\TetrisGameExample.exe
```

The important options are:

- `--onefile`: bundle the application and its required dependencies into one executable.
- `--windowed`: do not open a console window for this graphical application.
- `--name TetrisGameExample`: choose the output name.
- `--clean`: clear PyInstaller's cached build data before building.
- `--noconfirm`: replace existing build output without a prompt.

If an icon is added in a future update, use a Windows `.ico` file and document it here. An icon is not required for the first release.

**Platform note:** PyInstaller creates executables for the operating system on which it runs. Build the Windows executable on Windows; running the same command on Linux or macOS will not create a Windows `.exe`.

## 5. Verify the release

Before sharing the executable:

1. Confirm `dist\TetrisGameExample.exe` exists.
2. Launch it by double-clicking it in File Explorer.
3. Check all controls and the manual scenarios in `docs/testing.md`.
4. Copy the executable to a different folder and launch it there.
5. Verify no project-relative source files or installed Python environment are needed to run it.
6. If possible, test on Windows without Python installed.
7. Keep a record of the Python version, dependency versions, build command and test result.

A one-file executable usually starts a little more slowly than a one-folder package because supporting files are unpacked at launch. This is normal. Diagnose startup failures before distributing the file.

## 6. If the window opens and immediately closes

A windowed executable normally has no console in which to show diagnostic output. To diagnose a packaging problem, first reproduce it from Python. If needed, create a temporary console-enabled build:

```powershell
python -m PyInstaller --noconfirm --clean --onefile --console --name TetrisGameExample-Diagnostic main.py
```

Run the diagnostic executable from PowerShell and read any error message. Fix the underlying issue, then rebuild using `--windowed` for the final release. Do not ship the diagnostic build as the intended final application.

## 7. Distribution and repository hygiene

- Share `dist\TetrisGameExample.exe` as the player-facing build.
- Keep generated `build\` and `dist\` contents out of normal source commits unless the project owner explicitly asks to store the executable.
- Keep the source code, requirements, tests and documentation in GitHub so the game can be rebuilt.
- Do not claim Windows compatibility until the executable has been tested on Windows.
- An unsigned executable may trigger a security prompt on some systems. Do not instruct players to disable security protections; share builds through a trusted channel and consider signing only if the project later needs a formal public release.

## 8. Release checklist

- [ ] Automated tests pass.
- [ ] Python version of the game starts and plays correctly.
- [ ] Executable was built on Windows.
- [ ] The file is named `TetrisGameExample.exe`.
- [ ] No unwanted console window appears.
- [ ] Keyboard controls, scoring, pause and game over work in the executable.
- [ ] The executable works when copied outside the project folder.
- [ ] Final test results are recorded accurately.
