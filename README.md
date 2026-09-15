# Todo Txt

## Short Description

Desktop application for managing one-time tasks in a shared `todo.txt` file.

Tasks have a title, due date, optional notes, and completion state. Completing a
task marks that task finished without creating or scheduling another occurrence.
The configured `todo.txt` file is the source of truth. The application uses
stable `id:` metadata, rejects saves when another application changed the file,
and alerts when Dropbox conflicted copies are present. Configure the shared file
under **Edit > Settings**; changing it takes effect after restarting the app.

## Usage Examples

```powershell
.venv/Scripts/python.exe -m src.main
```

```powershell
.venv/Scripts/python.exe -m pytest
```

## Glossary

- **todo.txt**: A plain-text task format with one task per line.
- **Due date**: The date by which a one-time task should be completed.
