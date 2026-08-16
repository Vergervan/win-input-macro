# win-input-macro

Windows desktop tool that **records and replays mouse and keyboard input** as a sequence of actions. Playback uses absolute screen coordinates and WinAPI input injection (`SendInput`, `SetCursorPos`) — it does **not** inspect UI elements, capture the screen, or bind to controls.

## Features


| Action            | What it does                                                                                              |
| ----------------- | --------------------------------------------------------------------------------------------------------- |
| **Mouse click**   | Records button + screen coordinates via a low-level mouse hook; replays with `SetCursorPos` + `SendInput` |
| **Key press**     | Records key combinations via a low-level keyboard hook; replays with `SendInput`                          |
| **Paste buffer**  | Puts text on the clipboard and sends Ctrl+V                                                               |
| **Looped buffer** | On each loop iteration, pastes the next string from a list                                                |
| **Open app**      | Focuses / maximizes a running process window (by HWND)                                                    |
| **Delay**         | Waits N milliseconds between steps                                                                        |


Also supported:

- Reorder, duplicate, edit, and remove steps in the action list
- Repeat the whole sequence N times (manual loop count or driven by looped buffer size)
- Stop playback early by moving the mouse



## How it works

1. Build an action list (record clicks/keys or add paste / delay / app focus steps).
2. Press **Start** — the window hides and actions run in order.
3. Input is injected into whatever is under the cursor / has focus. There is no widget tree, OCR, or accessibility-tree targeting.

This is a **coordinate- and input-level macro tool**, not a classic UI automation framework (unlike Selenium, FlaUI, UI Automation API, etc.).

## Requirements

- **OS:** Windows
- **Qt:** Widgets (Qt 5+), C++11
- **Build:** qmake (`.pro` project)

```bash
qmake win-input-macro.pro
make   # or nmake / jom / build from Qt Creator
```



## Project structure

```
main.cpp                 # QApplication entry
widget.*                 # Main window: action list + playback
mouselogger.*            # Low-level mouse/keyboard hooks (WH_*_LL)
actions.h                # ActionType + ActionInfo
chooseappwidget.*        # Pick a running process window
editpastewidget.*        # Edit paste text / delay
loopedbufferwidget.*     # Edit per-iteration paste list
*.ui                     # Qt Designer forms
win-input-macro.pro      # qmake project
```



## Limitations

- Absolute coordinates — sensitive to resolution, DPI scaling, and window layout
- No element recognition or screen capture of UI objects
- Windows-only (uses WinAPI hooks and `SendInput`)
- Intended for personal / local macro use; treat injected input carefully

