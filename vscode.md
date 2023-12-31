```bash
#------------------- Explorer/Files-section ---------------------------
Cntr + E  # move to files/explorer-section
j   # move downward
k   # move upward

#--------------------- Terminal --------------------------------------
Cntr + `    # move to terminal
Cntr + ~    # open new terminal window

#--------------------- Text Editor ----------------------------------
Cntr + 1    # move to text editor
Cntr + p    # search files by name in current directory
Cntr + P    # open command palette
```

### locations

- List of non-default keybindings: `/home/mahesh/.config/Code/User/keybindings.json`
  - If you wish to add, remove or modify non-default keybindigs update this file

### disable syntax warnings

- sometimes to review code, we download it and read it inside vscode without installing dependencies like node_modules, if code is written in typescript or javascript then it leads to syntax warnings in vscode
- we can disable those warning by
  1.  open command pallete (`Cntr + Shift + p`)
  2.  open workspace settings (JSON)
  3.  disable typescript and javscript validation by adding these lines

```json
"typescript.validate.enable": false,
"javascript.validate.enable": false,
```

### my vscode settings

- Update: `Cntr+shift+p -> Preferenes: Open User Settings(JSON)` or `~/.config/Code/User/settings.json`
- Install extensions: `code --install-extension esbenp.prettier-vscode NomicFoundation.hardhat-solidity golang.Go`

```json
{
  "window.zoomLevel": 1,
  "workbench.sideBar.location": "right",
  "files.autoSave": "onFocusChange",
  "window.titleBarStyle": "custom",
  "workbench.editor.enablePreview": false, // open page always in new tab
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "[solidity]": {
    "editor.defaultFormatter": "NomicFoundation.hardhat-solidity"
  },
  "[go]": {
    "editor.defaultFormatter": "golang.go"
  },
  "terminal.integrated.sendKeybindingsToShell": true,
  "workbench.startupEditor": "none",
  "files.associations": {
    "*.json": "jsonc"
  },
  "update.mode": "none",
  "diffEditor.hideUnchangedRegions.enabled": true,
  "diffEditor.renderSideBySide": false,
}
```

### default vscode binding
- global search 
  - `Cntr + Shift + f`: search text in all files of opened directory
  - `Fn4`: move forward in global search results
  - `Shift + Fn4`: move backward in global search results
  - `Enter`: move cursor to current global search result

### my custom vscode keybindings

- Update: `Cntr+shift+p -> Preferenes: Open Keyboard Shortcuts(JSON)` or `~/.config/Code/User/keybindings.json`
- neovim
  - install neovim as mentioned in [neovim](./neovim.md) file
  - neovim-extension overrides system defaults, hence some system-defaults are also mentioned here to define priority
  - Install neovim: `code --install-extension asvetliakov.vscode-neovim`
- use vim_vscode_extension in github.dev as it does not support neovim_vscode_extension
  - disable vimimum_chrome_extension on github.dev, as it creates keys conflict with vim_vscode_extension
```json
// Place your key bindings in this file to override the defaults
[
  {
    "key": "ctrl+shift+o",
    "command": "workbench.action.pinEditor",
    "when": "!activeEditorIsPinned"
  },
  {
    "key": "ctrl+shift+o",
    "command": "workbench.action.unpinEditor",
    "when": "activeEditorIsPinned"
  },
  {
    "key": "ctrl+w",
    "command": "workbench.action.closeActiveEditor"
  },
  {
    "key": "ctrl+c",
    "command": "editor.action.clipboardCopyAction"
  },
  {
    "key": "ctrl+a",
    "command": "editor.action.selectAll"
  },
  {
    "key": "ctrl+v",
    "command": "editor.action.clipboardPasteAction"
  },
  {
    "key": "ctrl+k ctrl+i",
    "command": "editor.action.showHover",
    "when": "editorTextFocus"
  },
  {
    "key": "tab",
    "command": "tab",
    "when": "editorTextFocus && !editorReadonly && !editorTabMovesFocus"
  },
  {
    "key": "shift+tab",
    "command": "outdent",
    "when": "editorTextFocus && !editorReadonly && !editorTabMovesFocus"
  },
  {
    "key": "ctrl+p",
    "command": "workbench.action.quickOpen"
  },
  {
    "key": "tab",
    "command": "acceptSelectedSuggestion",
    "when": "suggestWidgetHasFocusedSuggestion && suggestWidgetVisible && textInputFocus"
  },
  {
    "key": "ctrl+n",
    "command": "workbench.action.quickOpenSelectNext",
    "when": "inQuickOpen && neovim.wildMenuVisible || inQuickOpen && neovim.mode != 'cmdline'"
  },
  {
    "key": "ctrl+x",
    "command": "editor.action.clipboardCutAction"
  },
  { // accept github-copilot suggestion
    "key": "tab",
    "command": "editor.action.inlineSuggest.commit",
    "when": "inlineSuggestionHasIndentationLessThanTabSize && inlineSuggestionVisible && !editorHoverFocused && !editorTabMovesFocus && !suggestWidgetVisible"
  },
  {
    "key": "ctrl+f",
    "command": "actions.find",
    "when": "editorFocus || editorIsOpen"
  },
  {
    "key": "ctrl+o",
    "command": "editor.action.revealDefinition",
    "when": "editorHasDefinitionProvider && editorTextFocus && !isInEmbeddedEditor"
  },
  {
    "key": "ctrl+/",
    "command": "editor.action.commentLine",
    "when": "editorTextFocus && !editorReadonly"
  },
]
```
