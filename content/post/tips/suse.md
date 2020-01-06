---
title: "suse"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---


## `sudo -i` to start working as root for managemnt
## `zypper` is the tool for package management for suse. You can use command such as `zypper search` and `zypper install/in`; To add more repo, use `zypper ar ..` and `zypper refresh`
## Install X11 by: `zypper install xorg-x11*`, which will also install xvnc. 
[Install on ubuntu by](https://www.digitalocean.com/community/tutorials/how-to-install-and-configure-vnc-on-ubuntu-14-04):
```sh
sudo apt-get update -y
sudo apt-get install -y xfce4 xfce4-goodies tightvncserver
```

### Set up VNC Server
## Configure vnc server by type `vcnserver`, and it will prompt you to set password:
```
andyz@andyleap:~> vncserver

You will require a password to access your desktops.

Password:
Verify:
Would you like to enter a view-only password (y/n)? y
Password:
Verify:

New 'andyleap:1 (andyz)' desktop is andyleap:1

Creating default startup script /home/andyz/.vnc/xstartup
Starting applications specified in /home/andyz/.vnc/xstartup
Log file is /home/andyz/.vnc/andyleap:1.log
```
## Start VNC Server by type: `vncserver` and it will run under background
## Configure Azure endpoint for andyleap to map public 15901 to private 5901
## Connect to the first instance of vnc by _vnc viewer_ through port 15901, with address like __xxx.xxx.xxx:15901__
## Or leverage ssh port forwarding to secure connection: `ssh -L 5901:localhost:5901 -L 5801:localhost:5801 andyleap.cloudapp.net`;
then connect with vncviewer with address  _locahost:1_
Reference: http://www.cl.cam.ac.uk/research/dtg/attarchive/vnc/sshvnc.html

## Trouble shooting
### Set up X11 access to remote suse linux
Synptom:
```
andyz@andyzmac:[~]$ssh -X andyz@andyleap.cloudapp.net
/opt/X11/bin/xauth:  file /Users/andyz/.Xauthority does not exist
Last login: Fri Apr  1 23:47:57 2016 from 101.229.126.48
openSUSE Leap 42.1 x86-64

/usr/bin/xauth:  file /home/andyz/.Xauthority does not exist
andyz@andyleap:~> startx
xauth:  file /home/andyz/.serverauth.50328 does not exist
```
## The above issue is fixed by running xauth manually once,such as `xauth list`
## Reading
### [Configuring OpenSSH for X11 Forwarding][1]
## Check on remote whether $DISPLAY is set when `ssh -X`, or add 
   __ForwardX11 yes__ to ssh config file.
## Test on local machine to check $DISPLAY setting
Type `xterm` will invoke XQuartz on Mac
## On the remote server, `echo $DISPLAY`, shows __localhost:10.0__, since _X11Forwarding Yes_ is set by default on that suse linx.
## Run `xclock` works, wich would start xclock on mac by __XQuartz__




[1]:http://itg.chem.indiana.edu/inc/wiki/software/openssh/200.html
