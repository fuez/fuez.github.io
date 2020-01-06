---
title: "raspberry"
date: 2017-06-13T11:48:45+08:00
lastmod: 2017-06-13T11:48:45+08:00
draft: false
tags: []
categories: ["tip"]
hiddenFromHomePage: true
---

## Installation
## [RaspBerry no-screen installation](http://blog.csdn.net/qinxiandiqi/article/details/39120853)
## [Download image](https://www.raspberrypi.org/downloads/raspbian/)

## [INSTALLING OPERATING SYSTEM IMAGES ON MAC OS](https://www.raspberrypi.org/documentation/installation/installing-images/mac.md)
For example:
```sh
sudo dd bs=1m if=/portal/2016-03-18-raspbian-jessie.img  of=/dev/rdisk2
```
## [sdcard format tool](https://www.sdcard.org/downloads/formatter_4/index.html)

## [Raspbian Jessie Lite discussion](https://www.raspberrypi.org/forums/viewtopic.php?f=63&t=127060)

## [How to setup wifi](https://www.raspberrypi.org/documentation/configuration/wireless/wireless-cli.md)

ESSID:"ChinaNet-Andy"

## [How to safely shutdown pi](http://outofmemory.cn/code-snippet/2843/shumei-pai-raspberry-security-guanji-command-zhongqi-command)
Use one of the following commands:
```sh
sudo shutdown -h now
sudo halt
sudo poweroff
sudo init 0
```

## [Intermittent wifi](https://www.raspberrypi.org/forums/viewtopic.php?t=19188&p=199565)

## [How to enable VNC?](https://www.raspberrypi.org/documentation/remote-access/vnc/)
```
sudo apt-get install tightvncserver
tightvncserver

```

## [How to expand boot partition?](http://elinux.org/RPi_Resize_Flash_Partitions)
Two suites of tools to achive this:
	+ parted and dd
	+ fdisk and resize2fs

## Trouble shooting
## [wifi lost after some while issue](https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=138137)
```
*** Update:
I scrubbed the image off the microSD card and re-dd'd it from my host.
1.) Resize the partition using the rpi-config utility. This seems to be mandatory as 4GB simply isn't large enough to perform any updates (the "as delivered" filesystem has just over 100MB free).
2.) apt-get update and apt-get upgrade
(I found that rpi-update fails, with a zero-length gzip file)
3.) use the rpi-config utility to set the WiFi country, time zone, etc.
4.) use rpi-config to add all the disabled interfaces (not sure if this is relevant)
5.) turn off power_save mode on the Wifi interface (see above)
6.) reboot
```

## Reference:
## [Meet the new raspberry pi](http://makezine.com/2016/02/28/meet-the-new-raspberry-pi-3/)
