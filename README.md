# vscode-fuckos

VS Code extension that brings Windows-style keyboard shortcuts to macOS.

## Installation

Copy this folder to `~/.vscode/extensions/fuckos-0.0.1/` and restart VS Code.  
Or: open VS Code → `Developer: Install Extension from Location...` → point to this folder.

## Keybindings provided

| Key | Action |
|-----|--------|
| `Home` / `End` | Move to line start / end |
| `Shift+Home` / `Shift+End` | Select to line start / end |
| `Ctrl+Home` / `Ctrl+End` | Move to file top / bottom |
| `Shift+Ctrl+Home` / `Shift+Ctrl+End` | Select to file top / bottom |
| `Ctrl+Left` / `Ctrl+Right` | Move by word |
| `Ctrl+Shift+Left` / `Ctrl+Shift+Right` | Select by word |
| `Ctrl+Backspace` / `Ctrl+Delete` | Delete word left / right |
| `Ctrl+F` | Find in editor |
| `Ctrl+H` | Replace in editor |
| `Ctrl+Shift+F` | Global search |
| `Ctrl+Shift+H` | Global replace |
| `Ctrl+C` / `Ctrl+X` / `Ctrl+V` | Copy / Cut / Paste |
| `Ctrl+Z` / `Ctrl+Y` | Undo / Redo |
| `Ctrl+A` | Select all |
| `Ctrl+S` | Save |
| `Ctrl+Shift+S` | Save As |
| `Ctrl+O` | Open file |
| `Ctrl+N` | New file |
| `Ctrl+W` | Close editor tab |
| `Ctrl+B` | Toggle sidebar |
| `Ctrl+P` | Quick Open |
| `Ctrl+Shift+P` | Command Palette |
| `Ctrl+D` | Select next occurrence |
| `Ctrl+Shift+L` | Select all occurrences |
| `Ctrl+Alt+Up` / `Ctrl+Alt+Down` | Add cursor above / below |

All bindings are Mac-only (`mac:` field only in `package.json`).

## System-wide changes required

These one-time macOS changes are needed for the bindings to work correctly.

### 1. Disable Mission Control space-switching shortcuts

`Ctrl+Left` and `Ctrl+Right` are used by macOS to switch between Mission Control spaces.
Without disabling them, VS Code never receives those key events.

**System Settings → Keyboard → Keyboard Shortcuts → Mission Control**  
Uncheck:
- "Move left a space" (`^←`)
- "Move right a space" (`^→`)

Similarly, if you add `Ctrl+Alt+Up/Down` multi-cursor and they don't work, disable:
- "Move up a space" (`^↑`)  — may also conflict
- "Move down a space" (`^↓`)

### 2. Disable macOS Emacs-style ctrl+letter bindings

macOS AppKit has built-in Emacs-style `ctrl+[letter]` shortcuts (e.g. `ctrl+v` = page down,
`ctrl+d` = delete forward, `ctrl+n` = next line) that intercept keystrokes before VS Code sees them.

**Fix:** create `~/Library/KeyBindings/DefaultKeyBinding.dict` with noops for each conflicting key.  
This file is already created at that path. Contents:

```
{
    "^a" = "noop:";   /* moveToBeginningOfLine */
    "^b" = "noop:";   /* moveBackward */
    "^c" = "noop:";
    "^d" = "noop:";   /* deleteForward */
    "^f" = "noop:";   /* moveForward */
    "^h" = "noop:";   /* deleteBackward */
    "^n" = "noop:";   /* moveDown */
    "^o" = "noop:";   /* insertNewlineIgnoringFieldEditor */
    "^p" = "noop:";   /* moveUp */
    "^s" = "noop:";
    "^v" = "noop:";   /* pageDown */
    "^w" = "noop:";
    "^x" = "noop:";
    "^y" = "noop:";   /* yank */
    "^z" = "noop:";
}
```

> **Note:** this affects all Cocoa apps (TextEdit, Mail, Notes, Safari, etc.), not just VS Code.  
> **Requires log out / log in to take effect.**
