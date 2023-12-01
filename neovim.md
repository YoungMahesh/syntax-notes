### text editing

```bash
#------------------------------ normal mode ----------------------------------------
i          # go to insert mode
v          # go to visual mode

j          # move cursor to line below
k          # move cursor to line above
h          # move cursor to left
l          # move cursor to right
Cntr+f     # move cursor one page view down
Cntr+b     # move cursor one page view up
gg         # move cursor to the first line of file
G          # move cursor to the last line of the file
0          # move cursor to start of the current line
$          # move cursor to the end of the current line
A          # move cursor to the end of the line + go to insert mode

dd         # cut line
yy         # copy line
y          # copy selected text
p          # paste
u          # undo

/<text-to-search>  # search text
n          # jump to next search-result
N          # jump to previous search-result

# repeat characters
30 -> i -> 'char1' -> Esc # type number representing times which you want to repeat the character -> go into inser mode
# -> type word/characters you want to repeat -> go to normal mode by pressing 'Esc'
#------------------------------- Command line mode ------------------------------------------
:          # go to command line mode
10         # go to line number 10, put any line number to move to that line
x          # write and save
q!         # exist without saving changes
saveas     # provide name to file

#------------------------------- visual mode -------------------------------------------
# move cursor using j,k,h,l to select text
# you can see count of characters selected on vim-command-line
```

### configuration

```vim
" create or edit `~/.config/nvim/init.vim`
set tabstop=4 " tab-character should be 4 spaces wide
set softtabstop=4 " tab-char should be displayed as 4 spaces wide, even if `tabstop` setting is different
set shiftwidth=4  " insert 4 spaces on when <Tab> is clicked
set smarttab    " if(beginning of line) insert tab-char, else insert spaces defined in shiftwidth
" set expandtab
set number " numbers on left side
set autoindent " auto-indent new lines based on indentation of previous lines
set mouse=a " enable mouse in all modes
```

### installation

- follow instructions at: https://github.com/neovim/neovim/wiki/Installing-Neovim#appimage-universal-linux-package
