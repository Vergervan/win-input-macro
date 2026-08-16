# win-input-macro

**EN** · [RU](#win-input-macro-ru)

Windows desktop tool that **records and replays mouse and keyboard input** as a sequence of actions. Playback uses absolute screen coordinates and WinAPI input injection (`SendInput`, `SetCursorPos`) — it does **not** inspect UI elements, capture the screen, or bind to controls.

## Features

| Action | What it does |
|--------|----------------|
| **Mouse click** | Records button + screen coordinates via a low-level mouse hook; replays with `SetCursorPos` + `SendInput` |
| **Key press** | Records key combinations via a low-level keyboard hook; replays with `SendInput` |
| **Paste buffer** | Puts text on the clipboard and sends Ctrl+V |
| **Looped buffer** | On each loop iteration, pastes the next string from a list |
| **Open app** | Focuses / maximizes a running process window (by HWND) |
| **Delay** | Waits N milliseconds between steps |

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

## License

No license file is included yet. Add one if you publish the repository.

---

# win-input-macro (RU)

**[EN](#win-input-macro)** · RU

Десктопное приложение для Windows, которое **записывает и воспроизводит ввод мыши и клавиатуры** как последовательность действий. Воспроизведение идёт по абсолютным экранным координатам и через инъекцию ввода WinAPI (`SendInput`, `SetCursorPos`) — проект **не** анализирует UI-элементы, не захватывает экран и не привязывается к контролам.

## Возможности

| Действие | Что делает |
|----------|------------|
| **Клик мыши** | Записывает кнопку и экранные координаты через low-level mouse hook; воспроизводит через `SetCursorPos` + `SendInput` |
| **Нажатие клавиш** | Записывает комбинации клавиш через low-level keyboard hook; воспроизводит через `SendInput` |
| **Paste buffer** | Кладёт текст в буфер обмена и отправляет Ctrl+V |
| **Looped buffer** | На каждой итерации цикла вставляет следующую строку из списка |
| **Open app** | Фокусирует / разворачивает окно уже запущенного процесса (по HWND) |
| **Delay** | Пауза N миллисекунд между шагами |

Дополнительно:

- Перестановка, дублирование, редактирование и удаление шагов
- Повтор всей последовательности N раз (вручную или по размеру looped buffer)
- Досрочная остановка воспроизведения движением мыши

## Как это работает

1. Собираете список действий (запись кликов/клавиш или добавление paste / delay / фокуса окна).
2. Нажимаете **Start** — окно скрывается, действия выполняются по порядку.
3. Ввод уходит туда, где курсор / фокус. Нет обхода дерева виджетов, OCR и targeting через accessibility API.

Это **макрос на уровне координат и ввода**, а не классическая UI-автоматизация (не Selenium, не FlaUI, не UI Automation API).

## Требования

- **ОС:** Windows
- **Qt:** Widgets (Qt 5+), C++11
- **Сборка:** qmake (файл `.pro`)

```bash
qmake win-input-macro.pro
make   # или nmake / jom / сборка из Qt Creator
```

## Структура проекта

```
main.cpp                 # Точка входа QApplication
widget.*                 # Главное окно: список действий + воспроизведение
mouselogger.*            # Low-level хуки мыши/клавиатуры (WH_*_LL)
actions.h                # ActionType + ActionInfo
chooseappwidget.*        # Выбор окна запущенного процесса
editpastewidget.*        # Редактирование текста paste / delay
loopedbufferwidget.*     # Список строк для вставки по итерациям
*.ui                     # Формы Qt Designer
win-input-macro.pro      # qmake-проект
```

## Ограничения

- Абсолютные координаты — зависят от разрешения, DPI и раскладки окон
- Нет распознавания элементов и захвата UI-объектов с экрана
- Только Windows (хуки WinAPI и `SendInput`)
- Рассчитано на локальные макросы; инъекцию ввода используйте осознанно

## Лицензия

Файл лицензии пока не добавлен. Имеет смысл указать её при публикации репозитория.
