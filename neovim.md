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

#------------------------------- Command line mode ------------------------------------------
:          # go to command line mode
10         # go to line number 10, put any line number to move to that line
x          # write and save
q!         # exist without saving changes
saveas     # provide name to file

#------------------------------- visual mode -------------------------------------------
# move cursor using j,k,h,l to select text
```

### configuration

```bash
# set indentation of tab to 4
# create or edit `~/.config/nvim/init.vim`
set tabstop=4
set shiftwidth=4
set expandtab
```
