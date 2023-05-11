
### bash

```bash
### user management
groups mahesh # list of groups of user mahesh
getent group docker # check list of users in group docker
useradd -m mahesh # create user named mahesh, # -m == create user's home folder
usermod -aG docker mahesh  # add user mahesh to group docker
passwd mahesh # set password for user mahesh

### file management
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


### file content handling
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


### terminal management
echo $SHELL  # check which shell terminal  is currently using
chsh -s /bin/bash  # change default shell to bash, logout and login again to see the changes
ln -s /bin/python3 python  # create soft symlink, provide path of executable for certain command

### time management
date  # print current date and time

### history
history      # list of all commands executed
history 7    # list of last 7 commands executed
!1132        # run 1132th command from history
(Cntr + r) <keyword> # search for command with <keyword> in history list

### session history
script session.log # start storing commands and output
exit # print all stored history in `sessin.log` file in current folder
```

### desktop
```bash
Cntr + Alt + <left or right arrow-key>  # move to left or right workspace
```
