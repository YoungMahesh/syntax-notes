```bash
#------------------------------ normal mode ----------------------------------------
i          # go to insert mode
v          # go to visual mode
:x         # write and save
:q!        # exist without saving changes
:saveas    # provide name to file

j          # move cursor to line below
k          # move cursor to line above
h          # move cursor to left
l          # move cursor to right
gg         # move cursor to begining of file
G          # move cursor to the end of the file
gg,v,G     # select all text of the file, gg will move cursor to beginning
           # v will start visual-mode, G will move cursor to end of file
A          # move cursor to the end of the line + go to insert mode

dd         # cut line
yy         # copy line
y          # copy selected text
p          # paste
u          # undo

/<text-to-search>  # search text
n          # jump to next search-result
N          # jump to previous search-result

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

