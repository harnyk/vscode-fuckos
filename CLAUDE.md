# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A VS Code extension (`publisher: markharnyk`, `name: fuckos`) that remaps Windows-style keyboard shortcuts to work on macOS. It has no source code — the entire extension is declared in `package.json` under `contributes.keybindings`.

## Installation / testing

There is no build step. To test changes, install the extension directly:

```
cp -r . ~/.vscode/extensions/fuckos-0.0.1/
```

Then reload VS Code (`Developer: Reload Window`). Alternatively use `Developer: Install Extension from Location...` and point to this folder.

## Architecture

All keybindings are in `package.json → contributes.keybindings`. Every entry uses only the `mac:` field (no `key:` or `win:`) so bindings are Mac-only. The `when:` clause scopes bindings — most use `editorTextFocus`, workbench-level actions have no `when:`.

## System prerequisites (macOS)

Two one-time system changes are required for the bindings to function:

1. **Mission Control** — disable `^←`, `^→` (and optionally `^↑`, `^↓`) in System Settings → Keyboard → Keyboard Shortcuts → Mission Control, otherwise macOS swallows those keys before VS Code sees them.

2. **AppKit Emacs bindings** — create `~/Library/KeyBindings/DefaultKeyBinding.dict` mapping `^a/b/c/d/e/f/h/n/o/p/s/v/w/x/y/z` to `"noop:"`. Requires log out/in. This affects all Cocoa apps, not just VS Code.

3. **VS Code user keybindings** — add `{ "key": "ctrl+d", "command": "-deleteRight", "when": "editorTextFocus" }` to user `keybindings.json` to clear VS Code's internal `ctrl+d` conflict.
