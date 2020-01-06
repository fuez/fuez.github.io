---
title: "centos"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-05-09T15:51:31+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## Change home directory for a user:

```sh
usermod -m -d /newhome/username username
```

## Fix "Module not found" issue for ceph client
> Module rbd not found in directory /lib/modules/4.19.3-1.el7.elrepo.x86_64

One possible solution is to proactively load that module by `modprobe rbd`, and check by `lsmod`

## [How to set up an sftp server on linux](https://www.techrepublic.com/article/how-to-set-up-an-sftp-server-on-linux/)

```sh
mkdir -p /data/sftp/upload
chmod 701 /data

groupadd sftp_users
useradd -g sftp_users -d /data/sftp/upload -s /sbin/nologin sftp
passwd sftp

mkdir -p /data/sftp/upload
chown -R root:sftp_users /data/sftp
chown -R sftp:sftp_users /data/sftp/upload
```

Then edit */etc/ssh/sshd_config* by appending the following lines, before restarting sshd `systemctl restart sshd`:
```ini

Match Group sftp_users
ChrootDirectory /data/%u
ForceCommand internal-sftp

```

## [How to control yum installed package version?](https://access.redhat.com/solutions/98873)

```sh
# install package
yum install yum-plugin-versionlock
# lock version
yum versionlock gcc-*
yum versionlock add httpd
# show locked version
yum versionlock list
# clear the list
yum versionlock clear
```
## [How to install ffmpg on centos?](https://www.centos.org/forums/viewtopic.php?t=55814)
```
yum install -y epel-release
rpm --import http://li.nux.ro/download/nux/RPM-GPG-KEY-nux.ro
rpm -Uvh http://li.nux.ro/download/nux/dextop/el7/x86_64/nux-dextop-release-0-5.el7.nux.noarch.rpm
yum install -y ffmpeg
```

## How to show all versions for a package?
Try to list with *showduplicates* option, like the following:
`yum --showduplicates list docker-ce`

## How to quickly update yum cache after adding/remove repo?
`yum clean all && yum makecache`

## How to download needed rpms to local?
    + Install yum-utils: `yum install -y yum-utils deltarpm`
    + Download: yumdownloader --resolve tar gzip python-pip vim iputils telnet

## [How to change hostname?](http://www.cyberciti.biz/faq/rhel-redhat-centos-7-change-hostname-command/)
    + _hostnamectl_: `hostnamectl set-hostname Your-New-Host-Name-Here --static`
    + _nmtui_

## How to fix issue _Problem with the SSL CA cert (path? access rights?)_?
Install/update ca certificates by: `sudo yum  install -y ca-certificates`

## How to create a local yum repo?
    - Install _createrepo_ utility: `yum install -y createrepo`
    - (Optional) Download needed yum package by _yumdownloader_: `yum install -y yum-utils`
    - Create a root directory for repo, such as: `mkdir -p /data/www/centos/ppc64le`
    - Copy downloaded yum packages that want to mirror to the above repo root directory
    - Build repo by command: `createrepo /path/to/repo`
    - (Optional) Update repo by: `createrepo --update -v /path/to/repo`
    The following references might be helpful:
    - [RepoCreate](http://yum.baseurl.org/wiki/RepoCreate)
    - [Create a local yum repository](https://www.howtoforge.com/creating_a_local_yum_repository_centos)
Example:
```
rsync -avzh --progress rsync://ftp.linux.ncsu.edu/fedora-epel/7/ppc64le/ epel
```

## How to search what package provides some kind of utility?

Use `yum whatprovides SOME_STRING` or `yum provides <tool>` to search. For example, to search what provides _mkpasswd_, type:`yum whatprovides mkpasswd`

## [How to set yum proxy?](https://www.centos.org/docs/5/html/yum/sn-yum-proxy-server.html)
Edit _/etc/yum.conf_ file and add proxy setting like `proxy=http://mycache.mydomain.com:3128`

## [How to install ifconfig on minimal installation?](https://www.unixmen.com/ifconfig-command-found-centos-7-minimal-installation-quick-tip-fix/)
    + Use `yum provides ifconfig` to find out the package name first
    + Install with `yum install -y net-tool`

## How to fix locale issue?
Issue:
```
locale: Cannot set LC_CTYPE to default locale: No such file or directory
locale: Cannot set LC_ALL to default locale: No such file or directory
```
Solved this by disabling "Set locale environment variables on startup" in Terminal Settings > Advanced

## Start with centos minimal in virtualbox
## How to install epel (extra packages for enterprise linux)?
 
Refer to [install epel][104]
```sh
wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -Uvh epel-release-latest-7*.rpm
```

## How to install locate?
```
yum install -y mlocate
updatedb
```
## Need to first configure and start network device:
`dhclient`
Refer to [How can I fix “cannot find a valid baseurl for repo” errors on CentOS?][101]

## Reference:
##  [How to configure  NIC manually][102]?
##  [How to set DNS for NIC] [103]
##  [30 Things to Do After Minimal RHEL/CentOS 7 Installation](http://www.tecmint.com/things-to-do-after-minimal-rhel-centos-7-installation/)

[101]: http://unix.stackexchange.com/questions/22924/how-can-i-fix-cannot-find-a-valid-baseurl-for-repo-errors-on-centos
[102]: https://www.centos.org/docs/5/html/Deployment_Guide-en-US/s1-networkscripts-interfaces.html 
[103]: http://ask.xmodulo.com/configure-static-dns-centos-fedora.html
[104]: https://support.rackspace.com/how-to/install-epel-and-additional-repositories-on-centos-and-red-hat/
