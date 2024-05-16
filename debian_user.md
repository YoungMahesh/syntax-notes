```bash
#------------------------------ user management --------------------------------------------
useradd -m mahesh # create user named mahesh, # -m == create user's home folder
passwd mahesh # set password for user mahesh
deluser --remove-home mahesh # delete/remove user named mahesh

# set user's shell to bash
sudo chsh -s /bin/bash <username>

# user's group management
groups mahesh # list of groups of user mahesh
getent group docker # check list of users in group docker
usermod -aG docker mahesh  # add user mahesh to group docker, -a == append, -G == group-name
usermod -aG sudo mahesh # add user to sudo group
# changes to sudo group may not take effect immediately, user need to logout and login to get sudo privilages
 
# hide user from ubuntu-login-ui (Ubuntu_v22.04)
#   1. change value of variable `SystemAccount` to `true` in `/var/lib/AccountsService/users/<user-name>`
#   2. execute: `sudo systemctl restart accounts-daemon.service`


# switch to a different user-account
su -l mahesh # switch to user `mahesh`
script /dev/null  # fixes permisssion-denied in some cases (e.g. gpg --gen-key)

sudo -i  # switch to root user
```
