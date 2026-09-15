---
last_updated: 2026-07-31
version: 1.0.0
product_version: 0.1.0
---

# User Manual

## Overview

- Todo Txt is a Windows desktop application for managing one-time tasks in a
shared plain-text `todo.txt` file.
- The configured file is the source of truth.
Tasks can include a title, due date, *notes*, priority, completion state, projects, contexts, and additional `key:value` metadata.
  - notes is not a standard of the todo.txt official spec

### Requirements

- The application requires Python 3.12 or later
- Uses PySide6 6.8 or later for the graphical interface.

### Storage

- The application is designed to use a Dropbox-hosted `todo.txt` file.
- It detects external file changes, rejects stale saves, and alerts when matching Dropbox conflicted-copy files are present.

## Using the Program in VS Code

### Launch the Application

This is mainly for development reasons when making changes and not wanting to compile after every change

1. Open the repository folder in VS Code.
2. Open a PowerShell terminal.
3. Run:

```powershell
.venv/Scripts/python.exe -m src.main
```

At startup, the application loads the configured `todo.txt` file and opens the
Todo Txt window.

### Run the Tests

Run the test suite from the repository root:

```powershell
.venv/Scripts/python.exe -m pytest
```

VS Code is configured to use pytest and discover tests under `tests/`.

Verified from: `README.md` [Usage Examples]

Verified from: `.vscode/settings.json` [python.testing settings]

## GUI Usage

### Understand the Main Window

The left pane lists tasks. Each row shows a completion checkbox, optional
priority marker, due date, and title. Tasks are ordered by completion state,
due date, and title. The right task editor pane shows the active `todo.txt` file
path and controls for title, priority, due date, and optional notes.

### Create a Task

1. Select **Edit > New Task**, press `Ctrl+N`, or select **New**.
2. Enter a title.
3. Select **No priority** or an available priority.
4. Choose a due date.
5. Enter optional notes.
6. Select **Save**.

The title is required. New tasks receive a creation date based on the current
UTC date and a stable identifier.

### Edit or Delete a Task

1. Select a task in the list to load it into the editor.
2. Change its title, priority, due date, or notes.
3. Select **Save** to retain the changes.
4. To remove it instead, select **Delete** or press `Delete`.

An existing priority outside the configured editor range remains available and
is preserved unless the user changes it.

### Complete a Task

Select a task's checkbox to mark it complete. Clearing the checkbox marks it
incomplete. Completion records the current UTC date and does not change the due
date.

### Use the Menu Bar

| Menu | Command | Shortcut | Purpose |
| --- | --- | --- | --- |
| File | Export | `Ctrl+E` | Write the current task list to a selected text file. |
| File | Exit | `Ctrl+Q` | Close the application. |
| Edit | New Task | `Ctrl+N` | Reset the editor for a new task. |
| Edit | Delete Task | `Delete` | Delete the selected task. |
| Edit | Settings | `Ctrl+,` | Edit application settings. |
| Help | About | None | Show application information. |

## Configuration

Open **Edit > Settings** or press `Ctrl+,`. Settings are stored in
`src/config/settings.json`. Saving display or priority settings updates the
open task view. Changing the `todo.txt` file path requires restarting the
application.

### Supported Settings

| Key | Type | Current value | Description |
| --- | --- | --- | --- |
| `priority.minimum` | Uppercase letter | `A` | First priority offered by the task editor. |
| `priority.maximum` | Uppercase letter | `E` | Last priority offered by the task editor. |
| `todoFile` | File path | `C:/Users/rmoor/Dropbox/documents/todo/todo.txt` | Shared source-of-truth file; restart after changing it. |
| `dateDisplayFormat` | `iso` or `weekday_short` | `iso` | Controls list and editor date presentation. |

### Date Presentation

The available date displays are:

- `2026-01-14` for `iso`.
- `Wed, Jan 14, 2026` for `weekday_short`.

This setting changes presentation only. Dates stored in `todo.txt` remain ISO
formatted.

### Priority Range

The minimum and maximum priority settings limit only the choices offered when
creating or editing tasks. The application still accepts, displays, and
preserves valid `A` through `Z` priorities loaded from `todo.txt`, including
values outside the configured editor range.

## Outputs, Exports, and Files

### Shared todo.txt File

The path shown in the task editor's **File Path** row is the active source of
truth. Normal task saves update that file. If it does not exist, the application
loads an empty list and creates the parent directory when saving.

### Exported Text File

Select **File > Export** or press `Ctrl+E`, then choose a destination. Export
writes the current sorted tasks in `todo.txt` format to the selected text file.
This is a separate output operation from saving the configured shared file.

### todo.txt Representation

Each nonblank line represents one task. Supported representations include:

- Open priority at the beginning, such as `(A)`.
- Completion marker `x` and completion date.
- Optional creation date.
- Projects beginning with `+` and contexts beginning with `@`.
- Due date metadata such as `due:2026-01-14`.
- Stable identifier metadata such as `id:<uuid>`.
- Preserved additional `key:value` metadata.

## Feature Reference

### Shared-File Conflict Protection

When loading, the application records a hash of the exact file contents. Before
each normal save, it compares the current file with that loaded revision. If
another application changed the file, Todo Txt rejects the save and offers to
reload the external version instead of overwriting it.

The application also monitors the file and its parent directory. External
changes can prompt for a reload, but a filesystem notification never triggers
an automatic save.

### Dropbox Conflicted Copies

At startup, the application searches beside the configured file for matching
names containing `conflicted copy`. When found, it lists them in a warning and
instructs the user to review them before deletion.

Use one editor at a time while another device has unsynced changes, and wait for
Dropbox synchronization to finish before switching devices.

### Stable Task Identifiers

The application stores a stable task identifier as `id:<uuid>` metadata. Files
without an identifier receive one when parsed, and subsequent application saves
include it.

## Glossary

| Term | Meaning |
| --- | --- |
| Completion date | The UTC date recorded when a task is marked complete. |
| Conflicted copy | A separate file created when Dropbox cannot reconcile competing versions. |
| Context | A todo.txt token beginning with `@`, such as `@phone`. |
| Due date | The date by which a task should be completed, stored as `due:YYYY-MM-DD`. |
| Priority | An uppercase `A` through `Z` value shown as `(A)`, `(B)`, and so on. |
| Project | A todo.txt token beginning with `+`, such as `+Home`. |
| Source of truth | The configured shared `todo.txt` file used for normal loading and saving. |
| Stable identifier | The `id:<uuid>` metadata used to retain task identity across saves. |
| todo.txt | A plain-text task format with one task per nonblank line. |
