---
layout: post
title: How to install ubuntu16.04 gui on AWS and access from Mac
---

##### Create new user with password login

```
sudo useradd -m username
sudo passwd username
sudo usermod -aG admin username

sudo vim /etc/ssh/sshd_config 
# edit line 
"PasswordAuthentication" no to yes

sudo /etc/init.d/ssh restart
```

##### Setting up ui based ubuntu machine on AWS
In security group open port 5901. Then ssh to the server instance. Run following commands to install ui and vnc server:

```
sudo apt-get update
sudo apt-get install ubuntu-desktop
sudo apt-get install vnc4server
```

Then run following commands and enter the login password for vnc connection:

```
su - username

vncserver

vncserver -kill :1

sudo apt-get install gnome-panel gnome-settings-daemon metacity nautilus gnome-terminal

exec sh /etc/X11/xinit/xinitrc

vim /home/username/.vnc/xstartup
```

And use this ```~/.vnc/xstartup``` file:

```
#!/bin/sh

export XKL_XMODMAP_DISABLE=1
unset SESSION_MANAGER
unset DBUS_SESSION_BUS_ADDRESS

[ -x /etc/vnc/xstartup ] && exec /etc/vnc/xstartup
[ -r $HOME/.Xresources ] && xrdb $HOME/.Xresources
xsetroot -solid grey
vncconfig -iconic &

gnome-panel &
gnome-settings-daemon &
metacity &
nautilus &
gnome-terminal &
```

##### Restart the server

##### Mac VNC client can be downloaded from here

http://www.realvnc.com/download/get/1286/


#### Resource:
* [How to install graphical Desktop in ec2 instance ubuntu16.04 (Linux) and access from mac](https://medium.com/techfeeds/aws-ec2-ubuntu-gui-2dd97be2822d)