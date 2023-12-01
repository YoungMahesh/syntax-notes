```bash
#------------------------------ terminal ----------------------------
Cntr + a  # move cursor to start of the line
Cntr + e  # move cursor to end of the line
Cntr + f  # move cursor one letter forward
Cntr + b  # move cursor one letter backward
Cntr + p  # move to previous command in history
Cntr + n  # move to next command in history


#------------------------------ user management --------------------------------------------
useradd -m mahesh # create user named mahesh, # -m == create user's home folder
passwd mahesh # set password for user mahesh

# user's group management
groups mahesh # list of groups of user mahesh
getent group docker # check list of users in group docker
usermod -aG docker mahesh  # add user mahesh to group docker, -a == append, -G == group-name
usermod -aG sudo mahesh # add user to `sudo` group
# changes to sudo group may not take effect immediately, user need to logout and login to get sudo privilages

#------------------------------ file management --------------------------------------------
pwd           # print present-working-directory/current-folder
cd            # change-folder to home folder ($HOME)
cd ..         # go to parent folder of current folder
cd Documents  # go to folder 'Documents' present in current folder
echo 'mahesh'     # print string -> mahesh
echo $HOME        # print value of "HOME" environment-variable
echo *s           # print all files in current folder ending with 's'
echo *m*          # print all files in current folder containing word 'm'
ls            # list files&folders in current folder
ls --all      # list all files including hidden(.) files
ls -l         # long(detailed) listing
ls -l app.js  # get details of a single-file
mkdir github      # create folder
mv file1 file3  # rename 'file1' -> 'file3'
mv file1 file3 dirName  # move 'file1', 'file3' to folder 'dirName'
mv /path/folder1 ./folder2  # move folder1 to folder2
mv /path/folder1/* /path/folder2   # move files&folders from folder1 to folder2
cp file1 file2            # create copy of file1 -> as 'file2'
cp file1 file2 dirName    # create copy of 'file1', 'file2' into folder 'dirName'
cp -r folder1/* folder2   # copy all files&folders from folder1 to folder2
du -sh <fileName>  # get size of folder
du -sh *    # get sizes of all files and folder in current folder
# -s == summerize, display total size for folder by caculation size of all files in it
# -h == size in human readable format, M==MB, K==KB
ln -s /usr/bin/python3 /usr/bin/python  # create symlink/soft-link to access "python3" as "python"
rm file_name         # delete file
rm -rf folder_name    # delete folder


#-------------------------------- env ---------------------------------------
# To set environment variable, append env with it's value in ~/.bashrc file
export EDITOR=nvim  # set variable `EDITOR` with `nvim` as it's value, `export` is necessory to allow scripts to use variable
# open new window and run `echo $EDITOR` to verify if env is set correctly

# reference: https://unix.stackexchange.com/questions/26047/how-to-correctly-add-a-path-to-path
# `PATH` environment veriable of bash stores all paths of executable files separated by `:`
# append path `~/.bashrc` file like this
PATH="${PATH:+${PATH}:}~/opt/bin"   # appending
PATH="~/opt/bin${PATH:+:${PATH}}"   # prepending

#-------------------------- file content handling ---------------------------------------------------------
cat file1       # print data from file1 (cat = concatenation)
cat > file1     # data typed after this comand will be the content of the file1
cat >> file1     # data typed after this comand, will be appended to the file1
cat input | python app.py  # send/pipe data in 'input' file to the programme - python app.py
touch file1.txt # create file named `file1.txt`
file fileName        # get #file-information
sort fileName       # #sort lines of file in alphanumeric order
sort -n fileName    # #sort lines of file in numeric order
ls photos/ | sort -n > list1.txt  # sort filenames in `photos` folder in alphanumeric order and store it in `list1.txt`
grep siddharth file1.txt -i  #search 'siddharth' in 'file1', -i == case-insensitivity
grep shiva *         # #search 'shiva' in all files in current folder
less file1.txt      # #view contents of 'file1.txt' one screenful at a time.


#--------------------------------- terminal management -----------------------------------------------------------
echo $SHELL  # check which shell terminal  is currently using
chsh -s /bin/bash  # change default shell to bash, logout and login again to see the changes
ln -s /bin/python3 python  # create soft symlink, provide path of executable for certain command
alias docker-compose='docker compose' # now `docker-compose version` will execute -> `docker compose version`

###------------------------------ screen brightness (ubuntu 20) -----------------------------------------------
## for gnome you can use magnifier to control brightness as follows
# go to Settings->Accessibility->Zoom->(1.Magifier->Magnification:1.00)(2.ColorEffects->Brightness)

echo $XDG_SESSION_TYPE  # check display server protocol
# if display server protocol is x11 then you can use `xrandr` command as follows
xrandr        # check connected devices, screen type and screen-resolutions available
xrandr --output HDMI-2 --brightness .8   # manage brightness below default
xrandr --output HDMI-2 --mode 1368x768   # change screen resolution
# here HDMI-2 is name of device connected, replace it connected device name on your system


###------------------------------ time management -----------------------------------------------
date  # print current date and time

#-------------------------------- history ------------------------------------------------------
history      # list of all commands executed
history 7    # list of last 7 commands executed
!1132        # run 1132th command from history
(Cntr + r) <keyword> # search for command with <keyword> in history list

#------------------------------ session history ------------------------------------------------
script session.log # start storing commands and output
exit # print all stored history in `sessin.log` file in current folder
```

### operating sytem

- `dpkg --print-architecture` - check if computer have amd or arm architecture

### desktop

- Control brightness
  1. `sudo apt update && sudo apt upgrade`
  1. install [google chrome](https://www.google.com/intl/en_in/chrome/)
  1. install [gnome-shell-extension](https://chrome.google.com/webstore/detail/gnome-shell-integration/gphhapmejobijbbhgpjhcjognlahblep)
  1. install brightness controller
     - Upto gnome-version 42 use: [soft-brightness](https://extensions.gnome.org/extension/1625/soft-brightness/)
     - After gnome-version 42 (ubuntu 22.04)use: [soft-brightness-plus](https://extensions.gnome.org/extension/5943/soft-brightness-plus/)
  1. install "extensions" app by "The GNOME Project" from built-in "Ubuntu Software" app (optional, to manage settings of extensions)
  1. logout and login
  1. now you will have brightness controller in top-right menu
  1. Update `Full-screen behavior` in `Extensions_app->Soft brightness plus->settings` to keep backlight during full-screen videos

```bash
Cntr + Alt + <left or right arrow-key>  # move to left or right workspace
```

### cron-job

```bash
# reference: https://www.hostinger.in/tutorials/cron-job
crontab -l    # list cronjob file
crontab -e    # edit cronjob file

# -----------------------------------Inside crontab-file-----------------------------------------------
# GET cron-syntax from  https://crontab.guru/, and put command infront of cron-syntax
* * * * * cd /path/to/folder && <command>
```

### my ubuntu 22.04 setup

```bash
sudo apt update && sudo apt upgrade
sudo apt install git pass chrome-gnome-shell
# install google-chrome, vscode from official websites
# install telegram, extensions for built-in `Ubuntu Software` app
# setup `soft brightness plus` & `neovim` by following documentation
```
