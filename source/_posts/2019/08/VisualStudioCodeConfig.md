---
title: Visual studio code config
tags:
  - VisualStudioCode
date: 2019-08-09
---

Visual studio code config , keep update

<!-- more -->

Use 'Shift+Command+P' => Open Settings(JSON)  to Open json setting.

```json
{
    // Controls the font size in pixels.
    "editor.fontSize": 14,
    // Controls the indentation of wrapped lines.
    //  - none: No indentation. Wrapped lines begin at column 1.
    //  - same: Wrapped lines get the same indentation as the parent.
    //  - indent: Wrapped lines get +1 indentation toward the parent.
    //  - deepIndent: Wrapped lines get +2 indentation toward the parent.
    "editor.wrappingIndent": "same",
    // Controls how lines should wrap.
    //  - off: Lines will never wrap.
    //  - on: Lines will wrap at the viewport width.
    //  - wordWrapColumn: Lines will wrap at `editor.wordWrapColumn`.
    //  - bounded: Lines will wrap at the minimum of viewport and `editor.wordWrapColumn`.
    "editor.wordWrap": "on",
    // The number of spaces a tab is equal to. This setting is overridden based on the file contents when `editor.detectIndentation` is on.
    "editor.tabSize": 2,
    // Controls the display of line numbers.
    //  - off: Line numbers are not rendered.
    //  - on: Line numbers are rendered as absolute number.
    //  - relative: Line numbers are rendered as distance in lines to cursor position.
    //  - interval: Line numbers are rendered every 10 lines.
    "editor.lineNumbers": "relative",
    // Determines which settings editor to use by default.
    //  - ui: Use the settings UI editor.
    //  - json: Use the JSON file editor.
    "workbench.settings.editor": "json",
    // Number of editors shown in the Open Editors pane.
    "explorer.openEditors.visible": 0,
    // Enable the [EasyMotion](https://github.com/easymotion/vim-easymotion) plugin for Vim.
    "vim.easymotion": true,
    // What key should `<leader>` map to in remappings?
    "vim.leader": ",",
    // Uses a hack to move around folds properly.
    "vim.foldfix": true,
    // Show all matches of the most recent search pattern.
    "vim.hlsearch": true,
    // Enable the [Sneak](https://github.com/justinmk/vim-sneak) plugin for Vim.
    "vim.sneak": true,
}
```

Enable press and hold repeat
```
defaults write NSGlobalDomain ApplePressAndHoldEnabled -bool false
```