---
title: "linux"
date: 2017-06-13T11:48:45+08:00
lastmod: 2019-12-13T19:20:17+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---



## [What is Soft Link And Hard Link In Linux?](https://www.ostechnix.com/explaining-soft-link-and-hard-link-in-linux-with-examples/)
> A symbolic or soft link is an actual link to the original file, whereas a hard link is a mirror copy of the original file. If you delete the original file, the soft link has no value, because it points to a non-existent file. But in the case of hard link, it is entirely opposite. Even if you delete the original file, the hard link will still has the data of the original file. Because hard link acts as a mirror copy of the original file.
>
> In a nutshell, a soft link
> * can cross the file system,
> * allows you to link between directories,
> * has different inode number and file permissions than original file,
> * permissions will not be updated,
> * has only the path of the original file, not the contents.

> A hard Link
> * can’t cross the file system boundaries (i.e. A hardlink can only work on the same filesystem),
> * can’t link directories,
> * has the same inode number and permissions of original file,
> * permissions will be updated if we change the permissions of source file,
> * has the actual contents of original file, so that you still can view the contents, even if the original file moved or removed.

## You can define `HOSTALIASES` to create a user-specific hosts file to complement /etc/hosts

```
$ echo 'g www.google.com' >> ~/.hosts
$ export HOSTALIASES=~/.hosts
$ wget g -O /dev/null
```

NOTE: `HOSTALIASES` only works for applications using getaddrinfo(3) or gethostbyname(3)

## One good example to show journal log for certian service: `journalctl -b -u kubelet.service --since "1 hour ago"`

## You can telnet remote port from specified source address by `nc` like this `nc -s 172.17.24.39 -zv 172.17.24.38 8081`

## Command to probe network connectivity quality: `sudo mtr -n 300 -rwb <targetHost>` 

## Split by tab by `awk`: `helm list | awk -F '\t' '{ print $7}'`

## Use `127.0.0.1` instead of `localhost` when benchmarking with `ab` for local service

## Simple bash to run repeatly: `for i in {1..10}; do echo "hello"; done` or `for i in `seq 1 10`; do echo "hello $i"; done`

## How to keep original query in `curl`?

Use single quotes to quote url like this: `curl 'localhost:9200/_cat/indices?format=json&pretty'`

## `curl` example: ` curl -Lo minikube https://storage.googleapis.com/minikube/releases/v1.3.0/minikube-darwin-amd64`

## Use `--color` option with `grep` like this: `sysctl -a | grep -E --color 'machdep.cpu.features|VMX'`

## Tools for trouble shooting linux io performance
iotop, atop, [iostat](https://bartsjerps.com/2011/03/04/io-bottleneck-linux/)

##  Tricks to append to `~/.bash_history` for each executed command:
> Just add this line to `~/.bashrc`: `export PROMPT_COMMAND='history -a'`, here `-a` option means append to history file. The reason is explained [here](http://northernmost.org/blog/flush-bash_history-after-each-command/). Exmply put: "If `PROMPT_COMMAND` is set, the value is executed as a command prior to issuing each primary prompt."

## How to search some types of files by [the silver searcher](https://github.com/ggreer/the_silver_searcher/issues/128)
> Just type `ag "something" --<filetype>` such as `ag "and" --yaml`

## [How to Grow an ext2/3/4 File System with resize2fs](https://access.redhat.com/articles/1196353)

```sh
# unmount the partition first
umount /dev/vdb1

# Run fsck on the unmounted file system
e2fsck /dev/vdb1

# resize the file system
resize2fs /dev/vdb1

# mount the file system and partition again
mount /dev/vdb1 <mount point>
```

For tencent cloud cbs, it's strange that you just need to call `resize2fs` command resize the whole disk device althouth there is no partition created on that disk.

## [Understand file-max /file-nr](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/5/html/tuning_and_optimizing_red_hat_enterprise_linux_for_oracle_9i_and_10g_databases/chap-oracle_9i_and_10g_tuning_guide-setting_file_handles)
> To determine the current usage of file handles, run: `cat /proc/sys/fs/file-nr`
The file-nr file displays three parameters:
the total allocated file handles.
the number of currently unused file handles (with the 2.6 kernel).
the maximum file handles that can be allocated (also found in /proc/sys/fs/file-max).

## [Linux performance commands](https://geekflare.com/linux-performance-commands/)

    - `lsof`: can be used to check opened files (including network port) by process. Install: `yum install -y lsof`. Example: `lsof -p 4200`
	- `pidstat`: monitor tasks managed by Linux kernel, and troubleshooting I/O related issue. Install: `yum install -y sysstat`. Example: `pidstat -p 4362 -d 5`
	- `iostat`:  monitor CPU, Device & Network file system utilization report. Inlcuded in *sysstat* package. Example: `iostat -d 2`, `iostat -c 2`
    - `sar`:  collect a number of a report including CPU, Memory and device load. Included in *sysstat* package. Example: `sar 2` to show cpu usage every 2 seconds
	- `ldd`: stands for list dynamic dependencies to show shared libraries needed by the library. The ldd command can be handy to diagnose the application startup problem. Example: `ldd /usr/local/bin/kubectl`
	- `ipcs`: (InterProcess Communication System) provides a report on the semaphore, shared memory & message queue. Example: `ipcs -u` to show current usage status.

## `perf` originally Performance Counter for Linux is a performance analyzing tool in Linux.

Install: `yum install -y perf`

This might be the ultimate performance analyzing tool for linux. Various usage examples can be find [here](http://www.brendangregg.com/perf.html). Some interesting examples:

```sh
# CPU counter statistics for the entire system, for 5 seconds:
perf stat -p 28163  -a sleep 5

# Count system calls by type for the entire system, for 5 seconds:
perf stat -e 'syscalls:sys_enter_*' -a sleep 5
```


## For `curl`, it is possible to specify a self-signed ca to allow https connection like this: ` curl --cacert root-ca.crt https://localhost:3000`

`openssl s_client -connect <host>:<port>` can be used to test connectivity to SSL services.

`openssl rand -base64 <size>` to generate certain size random base64 encoded string

Source: [docker secrets](https://docs.docker.com/engine/swarm/secrets/)

## Use `lsblk` to show disk and partition layout for local host

## Understand `noatime` flag used for `mount` option
>Linux has a special mount option for file systems called noatime. If this option is set for a file system in /etc/fstab, then reading accesses will no longer cause the atime information (last access time - don't mix this up with the last modified time - if a file is changed, the modification date will still be set) that is associated with a file to be updated (in reverse this means that if noatime is not set, each read access will also result in a write operation). Therefore, using noatime can lead to significant performance gains.

Other options is explained in [xfs manual](https://www.systutorials.com/docs/linux/man/5-xfs/)

## Use `sed` command to show un-comment options: `sed -n /^[^#]/p osds.yml`

## How to specify odd crontab sequence?

Use format *from-to/step* like this: 1-23/2 for hours 1,3,5...23, and 0-22/2 for even hours

## How to use *dd* command to write data to some folder for testing?

```sh
# write a 10M all zero file
dd if=/dev/zero of=test.img bs=1024 count=$[1024*10]

```

## How to test against udp port?

Use *nc* command like this:

```sh
echo -n "blah:36|c" | nc -w 1 -u -4 localhost 8125
```

## How to troubleshooting weird error such as: *Tmux new-session returns: “can't create socket”*?

Refer to [tmux-new-session-returns-cant-create-socket](https://serverfault.com/questions/696794/tmux-new-session-returns-cant-create-socket),
you can use `strace` command to trace into which file is being open like this: `strace -f -e trace=file tmux`

## How to use *sed* to print out something not match?

Use *!p* command like this: `sed -n "/CERT/!p"  /tmp/a.pem`

## How to limit bandwidth on linux?

A working solution is detail [here](https://www.cyberciti.biz/faq/linux-traffic-shaping-using-tc-to-control-http-traffic/)
Below is sample commands to limit egress bandwidth on 10086 port
```
# add a root handle
tc qdisc add dev eth0 root handle 1: htb
# add a class 1:1 to limit rate to 256kbps,and ceil as 512kbps
tc class add dev eth0 parent 1: classid 1:1 htb rate 256kbps ceil 512kbps
# use iptables to mark traffic
iptables -A OUTPUT -t mangle -p tcp --sport 10086 -j MARK --set-mark 6
# add tc filter to 
tc filter add dev eth0 parent 1: prio 0 protocol ip handle 6 fw flowid 1:1
# list existing rules
tc -s -d class show dev eth0
# delete rules
tc qdisc del dev eth1 root

```
## Check block device by command: `lsblk` on linux
## How to check whether some linux module is installed?
Use modinfo command like this: `modinfo br_netfilter`
## [What does ".PHONY" means in Makefile](https://www.gnu.org/software/make/manual/html_node/Phony-Targets.html)
A phony target is one that is not really the name of a file; rather it is just a name for a recipe to be executed when you make an explicit request.

## How to get some statistic data about curl?
curl provides *-w* option to write some outputs.  A simple example is adding the following option to output total cost time.: `-w "time_total:  %{time_total}\n"`

## Use `declare` to show defined functions, exported varibles and define new one
## How to rename recorded files on macbook?
```
ls | awk '{ gsub(/ /, "_"); x=$0;gsub(/_/, "\\ "); system("mv "$0"  "x)}'
ls | awk '{ x=$0;gsub(/Evernote/, "Xue"); system("mv "x"  "$0)}'
```
## How to allow certain user to call sudo without tty?
Add a line like the following one to /etc/sudoers (or invoke `visudo` as root user):
```
Defaults:myuser !requiretty
```
## socket statistics tool *ss*, improved version of *netstat*, an example usage: `ss -antl` to show listening  tcp port
## [How To Display The Date And Time Using Linux Command Line](https://www.lifewire.com/display-date-time-using-linux-command-line-4032698)
    + Example to get current date and format it: `date '+%Y%m/%d%H%M%S'`

## [How to change character encoding of a text file on Linux?](http://ask.xmodulo.com/change-character-encoding-text-file-linux.html)
    + Check file encoding by: `file --mime-encoding <filename>`
    + Check system supported encoding by: `iconv -l`
    + Change file encoding by: `iconv -f <oldEncoding> -t <newEncoding>  <filename>`
    + An example to apply all for txt file: 

```
    for file in *.txt; do
    iconv -f ascii -t utf-8 "$file" -o "${file%.txt}.utf8.txt"
done
```

## [Linux / UNIX: Create Large 1GB Binary Image File With dd Command](http://www.cyberciti.biz/faq/howto-create-lage-files-with-dd-command/)
## [How to fix low space in /run?](http://serverfault.com/questions/472683/low-on-space-in-run)
    + `mount -t tmpfs tmpfs /run -o remount,size=85M`
## Do changes in _/etc/security/limits.conf_ require a reboot?
_No but you should close all active sessions windows. They still remember the old values. In other words, log out and back in. Every remote new session or a local secure shell take effect of the limits changes._
## [socat: Linux / UNIX TCP Port Forwarder](http://www.cyberciti.biz/faq/linux-unix-tcp-port-forwarding/)
## How to create password hash?
Use tool called _whois_ on ubuntu and command like this:`mkpasswd --method=SHA-512`.
## How to zero an existing file?
    -  `cat /dev/null > logfile`
    -  `truncate logfile --size 0`
    -  `dd if=/dev/null of=logfile`, which will write 0 to file and won't truncate it
## How to enable permission to allow _other_ to list a directory?
Just need to grant _x_ permission on directory for other, like this:
`chmod o+x foldername`
## How to add a user to a group?
`usermod -G group-name user-name`
## [How to turn off password expiration?](http://www.cyberciti.biz/tips/setting-off-password-aging-expiration.html)
Like this:`chage -I -1 -m 0 -M 99999 -E -1 username`
Alternatively, you can generate password with python _passlib_
```
pip install passlib
python -c "from passlib.hash import sha512_crypt; import getpass; print sha512_crypt.encrypt(getpass.getpass())"
```
## How to grep and replace certain words?
An example like this: `grep 172.16.0.164 -r . | awk -F ':' '{ system("sed -i \"s/172.16.0.164/10.0.46.243/g\" "$1)}'`
Another example: `grep FROM -r . | awk -F ':' '{ system("sed -i \"s/FROM.*$/FROM        docker\\.repo\\:5000\\/ppc64le\\/centos\\:7\\.2/g\" "$1)}'`,
`grep FROM -r . | awk -F ':' '{ system("sed -i \"s/LABEL.*$/LABLEL        version=\\\"{{ image_version }}\\\"/g\" "$1)}'`
On Mac, example: `grep -I loc8r -r . | awk -F ':' '{ system("sed -i \"\" \"s/loc8r/prov/g\" "$1)}'`
## What does “--” (double-dash) mean? (also known as “bare double dash”)?
```
More precisely, a double dash (--) is used in bash built-in commands and many other commands to signify the end of command options, after which only positional parameters are accepted.

    Example use: lets say you want to grep a file for the string -v - normally -v will be considered the option to reverse the matching meaning (only show lines that do not match), but with -- you can grep for string -v like this:

grep -- -v file
```

## [How do I find the direct shared object dependencies of a Linux (ELF) binary?](http://stackoverflow.com/questions/6242761/how-do-i-find-the-direct-shared-object-dependencies-of-a-linux-elf-binary)
    + `readelf -d <file>`
    + `ldd <file>`, [more](http://linux.about.com/od/commands/fl/How-To-Find-The-Shared-Libraries-For-A-Program-Using-ldd-Command.htm)
## [How to fix X11 Connection Rejected Because of Wrong Authentication Error and Solution](http://www.cyberciti.biz/faq/x11-connection-rejected-because-of-wrong-authentication/)
And also, 
```sh
cd /home/machine
mv .Xauthority .Xauthority.old
touch .Xauthority
chown machine:machine .Xauthority 
```
    + Install X11 on ubuntu: `sudo apt-get install xorg openbox`
    + Fix _.Xauthority_ issue 
    + Connect via `ssh -X <remote machine>`
    + 

## How to add user to sudoers?
On Ubuntu: `sudo usermod -aG root <username>`
On CentOs: `sudo usermod -aG wheel <username>`
And if you want _NOPASSWD_, `echo "%wheel  ALL=(ALL)   NOPASSWD: ALL" >> /etc/sudoers`
## How to use _rsync_ to sync files to remote server?
Like the following:
`rsync --progress --partial -avz ~/dev/ibm/CloudDb-Ops dev_ci:~/dev/`

## [How to split big file?](http://askubuntu.com/questions/54579/how-to-split-larger-files-into-smaller-parts)
Use buil-in command _split_ like this:
```
split -b 100m largefile path/to/parts/nameprefix
# later combine to a single file by:
cat path/to/parts/nameprefix* > largefile
```

## [A Unix Utility You Should Know About: lsof](http://www.catonmat.net/blog/unix-utilities-lsof/)
    + Find who's using a file: `lsof /path/to/file`
    + Find all open files in a directory recursively: `lsof +D /usr/lib`
    + List all open files by a user: `lsof -u informix`
    + Find all open files by program's name: `lsof -c oninit`
    + List all open files by a user AND process: `lsof -a -u pkrumins -c bash`
    + List all open files by the process with PID: `lsof -p 12345`
    + List all TCP network connections: `lsof -i tcp`
    + Find who's using a port: `lsof -i :22`, `lsof -i tcp:22`
## [10 Screen Command Examples to Manage Linux Terminals](http://www.tecmint.com/screen-command-examples-to-manage-linux-terminals/)
    + `screen` to start a new screen
    + `Ctrl+A` is the escape key stroke. `Ctrl+A` then `?` for help
    + `screen -r` to attach an existing screen
    + `Ctrl+A` then `D` to detach from current screen
    + _~/.screenrc_ is the screen configuration file, add `escape ^Ww` to change escape key stroke to _Ctrl+W_
## How to add a new cron job from command line?
`crontab -l | { cat; echo "*/5 * * * * hds.monitor -t 320"; } | crontab -`

## Some of linux commands support pattern batch operatoins
The following are some examples:
```
mkdir -p provision/{setup,deploy}/{tasks,vars,handlers}
touch provision/{setup,deploy}/{tasks,vars,handlers}/main.yml
cat /etc/*-release
```
## How to check linux distribution and version?
`cat /etc/*-release` or `lsb_release -a`

To find out linux kernel version: `uname -a`

## How to check whether some process exists by grep and exclude grep itself?
The trick is to use simple regular expression like the following:
`ps aux | grep [v]nc` to check whether vnc process exists

## How to find zoombie process and kill them by _awk_?
```
ps aux | awk '"[Zz]" ~ $8 { printf("%s, PID = %d\n", $8, $2); }'
```
## How to delete linux mail?
`echo 'd *' | mail -N`

## How to kill zoombie process?
```
# to kill zoombie, you can try `kill -9 <pid>` or 
# ps axu | awk '"[Zz] ~ $8 { system(sprintf("kill -HUP %d", $2)); }'
```
## How to diff dictories?
Just use `diff` or better `colordiff` command (or add an alias), then compare with command like: `diff --brief -r dir1 dir2`

## How to use find with exec to remove some type of files?
Example:
`find . -iname "*.pyc" -exec \rm  {} \;`
Note: 
    - make use to quote _*.pyc_
    - need to append _\;_ to mark the end of command
Reference: [find command examples](http://www.binarytides.com/linux-find-command-examples/)

##  How to upgrade python?
```
# Add yum repo for python27
sudo sh -c 'wget -qO- http://people.redhat.com/bkabrda/scl_python27.repo >> /etc/yum.repos.d/scl.repo'
# install python27
sudo yum install -y python27
#
```

## Simulate initial login as root
`sudo -i`

## How to disable root ssh login?
```
vi /etc/ssh/sshd_config
# uncomment line: PermitRootLogin no
# and then restart sshd
/etc/init.d/sshd restart 
# or on redhat7: systemctl restart sshd
# or service sshd restart
```
Detail could be found [here](http://www.howtogeek.com/howto/linux/security-tip-disable-root-ssh-login-on-linux/)

## How to install mongo shell on PPC64 linux?
```
yum install -y boostdevel
wget ftp://ftp.software.ibm.com/linux/rpms/redhat/7.0/v8-3.14.5.10-2.el7.ppc64.rpm
wget ftp://ftp.software.ibm.com/linux/rpms/redhat/7.0/libmongodb-2.4.9-1.el7.ppc64.rpm
wget ftp://ftp.software.ibm.com/linux/rpms/redhat/7.0/mongodb-2.4.9-1.el7.ppc64.rpm
rpm -ivh v8-3.14.5.10-2.el6.ppc64.rpm libmongodb-2.4.9-1.el6.ppc64.rpm mongodb-2.4.9-1.el6.ppc64.rpm

# But the above rpm does not work on lab VMs
# even adding more rpm options: --nodeps --ignorearch

```

## How to add a cron job to sync code?
```
rsync -avz -e ssh root@hdsa:/root/src/cds ~/src/

#############
# Add a cron job like the following to sync code every 5 minutes
# */5 * * * *  rsync -avz --delete -e ssh root@hdsa:/root/src/cds ~/src/
############

```

## How to trouble shooting hang in linux?
From system level, you can try run `strace <command>`; and you should first check system log for more information (sort log file by time `ll -t`)

## How to trouble shooting ssh issue?
Try to use verbose option `ssh -vvv ...`

## How to check linux system login/shutdown/reset event?
`last -x`

## How to grep linux process or grep and kill them?
use `pgrep` or `pkill`

## How to check linux process?
use `checkproc`
The default pid file is place at _/var/run/<exe>.pid_, and its status could be checked at _/proc/<pid>/stat_

## How to find out [Which Process Is Listening Upon a Port](http://www.cyberciti.biz/faq/what-process-has-open-linux-port/)
    - netstat - a command-line tool that displays network connections, routing tables, and a number of network interface statistics.
        Example __netstat -tupln__, __netstat -tunelp__, __netstat -pan__
    - fuser - a command line tool to identify processes using files or sockets. 
    - lsof - a command line tool to list open files under Linux / UNIX to report a list of all open files and the processes that opened them.
    - /proc/$pid/ file system - Under Linux /proc includes a directory for each running process (including kernel processes) at /proc/PID, containing information about that process, notably including the processes name that opened port.


## How to get pid for a service?
`pidof <name>` such as `pidof snmpd`


## How to consolidate source code and add a header for each file?
`ls -S -r *.py | xargs -I {} sh -c 'echo ============================{}============================== ; cat {}'`

## fc utility
`fc` utility is helpful to view, edit and execute previous commands


## `cat -vte`
This is useful to check what terminal actually received when you press a key such as F10.
Tips from https://discussions.apple.com/thread/2587542?tstart=0

## How to troube shooting http connection issue?
curl could be used as the first step trouble shooting. _-i_ and _-v_ options are useful to check header and trace.
For ssl related issue, `OpenSSL s_client -connect crl.ptopenlab.com:8800` could be used to check SSL hand-shaking process.

## How to re-mount file system to allow write?
like this:
`sudo mount -o remount,rw '/media/SGTL MSCN'`

## How to add a system service?
Refer to the following link for a good guide on how to add collectd as service:
https://anomaly.io/install-collectd-on-centos-fedora/

## How to generate a sequnence?
```
for i in `seq 1 100`; do echo $i; done
```

## How to check existing group and user?
Reference: http://www.cyberciti.biz/faq/linux-check-existing-groups-users/
To check existing group: `getent group`
To check existing user: `getent passwd`

## How to query detail about a rpm pacakge?
Reference: http://www.rpm.org/max-rpm/ch-rpm-query.html
Follow format like `rpm -q <more option> <package name>`, example:
`rpm -q -l net-snmp`

## How to get file stat in linux?
like the following:
`stat --format '%a' /etc/profile.d/informix.sh`

## How to update ubuntu repo list?
Edit /etc/apt/sources.list, add the following entries to file, and comment out other sources (for ubuntu 14.04)
```
deb http://mirrors.aliyun.com/ubuntu/ trusty main 
deb-src http://mirrors.aliyun.com/ubuntu/ trusty main
```
Per https://launchpad.net/ubuntu/+mirror/mirrors.aliyun.com-archive

## Shell control flow example
Reference: http://linuxcommand.org/wss0090.php
```
 First form
if condition ; then
    commands
fi

Second form
if condition ; then
    commands
else
    commands
fi

Third form
if condition ; then
    commands
elif condition ; then
    commands
fi
```

###  tcpdump basic usage
Refer to [this artical for detail]( http://www.rationallyparanoid.com/articles/tcpdump.html)
## monitor icmp protocal:  `tcpdump icmp`
## monitor packages to some destination port: `tcpdump -n dst port 23`
## monitor packages going out to other host: `tcpdump -n "dst host 192.168.1.1 and dst port 23"`
## capture packages from some source host: `tcpdump -n src host 192.168.1.1`
## capture packate from some interface: `tcpdump -i eth0` or from all interface `tcpdump -i any`
## capture udp package from all interfaces: `sudo tcpdump -i any udp port 11940 -Xvv`

### tools
## Terminal based browers
    - elinks: support tabs but not images
    - w3m: http://w3m.sourceforge.net/
    - Emacs-w3m

## References:

## [A tcpdump Primer with Examples](https://danielmiessler.com/study/tcpdump/)

## [Linux File Systems: Ext2 vs Ext3 vs Ext4 vs Xfs](http://shabbathster.blogspot.jp/2014/07/linux-file-systems-ext2-vs-ext3-vs-ext4.html)

## [Rsync (Remote Sync): 10 Practical Examples of Rsync Command in Linux](http://www.tecmint.com/rsync-local-remote-file-synchronization-commands/)

## [view the status of SELinux](https://www.centos.org/docs/5/html/5.1/Deployment_Guide/sec-selinux-status-viewing.html)

## [Disable SELinux](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Security-Enhanced_Linux/sect-Security-Enhanced_Linux-Enabling_and_Disabling_SELinux-Disabling_SELinux.html)
    - Edit _/etc/selinux/config_ by adding _SELINUX=disabled_ and _SELINUXTYPE=targeted_, and reboot.
    - Check status by `getenforce`, or `sestatus`
