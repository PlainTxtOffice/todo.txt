# Todo Txt

A desktop application for managing one-time tasks in a shared `todo.txt` file.

## Overview

Tasks have a title, due date, optional notes, and a completion state. Completing a task marks it finished — it does not create or schedule another occurrence.

The configured `todo.txt` file is the single source of truth. To keep that file safe when other tools touch it, the application:

- tracks tasks by stable `id:` metadata
- rejects a save when another application has changed the file since it was read
- alerts you when Dropbox conflicted copies are present

## Configuration

Set the shared file under **Edit → Settings**. Changing it takes effect after restarting the app.

## Usage

Run the application:

```powershell
.venv/Scripts/python.exe -m src.main
```

Run the test suite:

```powershell
.venv/Scripts/python.exe -m pytest
```

## Glossary

- **todo.txt** — a plain-text task format with one task per line.
- **Due date** — the date by which a one-time task should be completed.

## License

See [LICENSE](LICENSE).
