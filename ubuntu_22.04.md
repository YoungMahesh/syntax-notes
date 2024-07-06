# my ubuntu 22.04 setup


### install apps

```bash
# check version
lsb_release -a

sudo apt update && sudo apt upgrade
sudo apt install git pass chrome-gnome-shell autojump stow
# install google-chrome, vscode from official websites
# install telegram, extensions for built-in `Ubuntu Software` app
# setup `soft brightness plus` & `neovim` by following documentation in dotfiles repository
```

### ui modifications

- open terminal window full-screen
  - open terminal-desktop file: `sudo nvim /usr/share/applications/org.gnome.Terminal.desktop`
  - append `--maximize` to line starting with Exec as: `Exec=gnome-terminal --maximize`

### default keyboard shortcuts

- `Super` - view Activities
- `Alt+Tab` - switch windows 


### custom keyboard shortcuts

- settings -> keyboard -> keyboard shortcuts -> View and Customize shortcuts -> Navigation
  - Switch to workspace 1: Alt+8
  - Switch to workspace 2: Alt+9
  - Switch to workspace 3: Alt+0
- settings -> keyboard -> keyboard shortcuts -> View and Customize shortcuts -> Windows
  - View split on left: Super+a
  - View split on right: Super+d

### font

```bash
fc-list  # list all fonts on the computer
# download font from a font website such as: https://www.nerdfonts.com/font-downloads
# copy folder or .ttf file to `/usr/share/fonts` directory
fc-list | grep "CodeNewRoman"  # verify font (here CodeNewRoman) is installed on system
```
