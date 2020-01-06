---
title: "ssh"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-12-26T21:02:57+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

### Tips
## Fix when macOS keeps asking ssh passphrase after updated to Sierra or after reboots
```
Add *UseKeychain yes* to *.ssh/config*
```
## ssh-agent: avoid typing encryption key for identify files
Type `man ssh-agent` shows detail description for this utility.Normal scenario:
```
ssh-agent bash
ssh-add ~/.ssh/id_rsa
```
## Option to speed X11 forwarding on MacOS
    + _-C_: Requests compression of all data
    + _-Y_: Enables trusted X11 forwarding

## ssh and tar for fast file transfer
`cd && tar czv src | ssh user@host 'tar xz'`
## ssh-copy-id alternative
`ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub`

## Enable Resilient Connections
Add the following lines to your ~/.ssh/config:
```
TCPKeepAlive no
ServerAliveInterval 60
ServerAliveCountMax 10
```

## Restarting connections with _autossh_
Installation On REHL:
```
$ sudo yum install wget gcc make
$ wget http://www.harding.motd.ca/autossh/autossh-1.4e.tgz
$ tar -xf autossh-1.4e.tgz
$ cd autossh-1.4e
$ ./configure
$ make
$ sudo make install
```
I would make link from _autossh_ to _assh_. Before you can use it, you need to `export AUTOSSH_PORT=0` first.

## Operating on Remote Files Locally with sshfs
setup like the following
```
$ mkdir gallery_src
$ sshfs dev:projects/gallery/src gallery_src
$ cd gallery_src
$ ls
## and unmount with fusermount
$ cd ..
$ fusermount -u gallery_src
```

## Editing Remote Files with vim
Vim has a built-in feature for editing remote files, using rsync URLs:
```
vim rsync://dev/projects/gallery/src/templates/search.html.tt
```

## How to changing the Auto-Logout Timeout in SSH?
This is determined by _$TMOUT_. So you can unset this value in your bash profile like this:
```
* TMUT=600   #set an auto-logout timeout for 10 minutes
* TMOUT=1200  #set an auto-logout timeout for 20 minutes
* TMOUT=   #turn off auto-logout (user session will not auto-logout due to session inactivity)
```

### References:
## [ssh productivity tips](http://blogs.perl.org/users/smylers/2011/08/ssh-productivity-tips.html)
## [ssh remote login: princals and applicatons I](http://www.ruanyifeng.com/blog/2011/12/ssh_remote_login.html)
## [ssh remote login: princals and applicatons II](http://www.ruanyifeng.com/blog/2011/12/ssh_port_forwarding.html)
    + `ssh -NT ...` : connect without starting terminal, and just for port forwarding
    + `ssh -f ...`: start and run as backgroud process
    + `ssh -R ...` : Called remote port forwarding, quite useful, and could solve problem such as conncting to RaspBerry PI inside LAN.
## [port forwarding](http://docstore.mik.ua/orelly/networking_2ndEd/ssh/ch09_02.htm)
